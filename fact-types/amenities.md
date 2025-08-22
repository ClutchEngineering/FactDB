# Amenity Facts

Amenity facts indicate which services and facilities are available at gas stations.

## Fact Key Pattern

```
amenity_{amenity_id}
```

## Standard Amenities

- `amenity_restrooms` - Public restrooms/bathrooms
- `amenity_atm` - ATM machine
- `amenity_car_wash` - Car wash facility
- `amenity_air_pump` - Air pump for tires
- `amenity_convenience_store` - Convenience store/shop
- `amenity_restaurant` - Restaurant or food service
- `amenity_ev_charging` - Electric vehicle charging stations

## Supported Values

- `"true"` - Amenity is available
- `"false"` - Amenity is not available
- `"unknown"` - Status unknown
- `"unavailable_temporary"` - Temporarily out of service
- `"unavailable_permanent"` - Permanently removed

## Example Submissions

```swift
// Station has restrooms and convenience store
factEdits["amenity_restrooms"] = FactEdit(value: "true", currency: nil, volume: nil)
factEdits["amenity_convenience_store"] = FactEdit(value: "true", currency: nil, volume: nil)

// Station does not have car wash
factEdits["amenity_car_wash"] = FactEdit(value: "false", currency: nil, volume: nil)

// ATM is temporarily out of service
factEdits["amenity_atm"] = FactEdit(value: "unavailable_temporary", currency: nil, volume: nil)

// Air pump status unknown
factEdits["amenity_air_pump"] = FactEdit(value: "unknown", currency: nil, volume: nil)
```

## Submission Flow

The amenity editor processes all amenities in a single batch:

```swift
func submitAmenities() async {
    var factEdits: [String: FactEdit] = [:]
    
    for amenity in amenitiesStore.allAmenities {
        let factKey = "amenity_\(amenity.id)"
        
        switch amenity.state {
        case .unknown:
            factEdits[factKey] = FactEdit(value: "unknown", currency: nil, volume: nil)
        case .available:
            factEdits[factKey] = FactEdit(value: "true", currency: nil, volume: nil)
        case .unavailable:
            factEdits[factKey] = FactEdit(value: "false", currency: nil, volume: nil)
        case .unavailableTemporary:
            factEdits[factKey] = FactEdit(value: "unavailable_temporary", currency: nil, volume: nil)
        case .unavailablePermanent:
            factEdits[factKey] = FactEdit(value: "unavailable_permanent", currency: nil, volume: nil)
        }
    }
    
    await GroundTruthService.saveFacts(identifier: stationIdentifier, facts: factEdits)
}
```

## Special Relationships

### Restrooms → Bathroom Details
When `amenity_restrooms` is set to `"true"`, it enables submission of detailed bathroom facts:
- Bathroom facility configurations
- Cleanliness ratings
- Changing table availability

See [Bathroom Facts](bathrooms.md) for details.

### EV Charging
The `amenity_ev_charging` fact indicates basic EV charging availability. More detailed charging facts may be added in future versions.

## Code References

- **Definition**: `apps/Scout/Scout/Core/Configuration/FactDefinitions.swift:148-180`
- **Submission Logic**: `apps/Scout/Scout/Features/Stations/AmenityEditorView.swift:288-298`
- **UI**: `apps/Scout/Scout/Features/Stations/AmenityEditorView.swift`

## Validation

- Amenity IDs must match predefined list in `FactDefinitions.Amenities`
- Values must be one of the supported state strings
- No currency or volume metadata is used for amenity facts