# Canadian Station Example

Example fact submission for a Canadian gas station showing regional differences in pricing format.

## Station Profile
- **Location**: Canada (single pricing, no cash/credit distinction)
- **Currency**: CAD
- **Volume**: Liters
- **Fuel**: Regular and Premium with ethanol blends

## Fact Submission

```swift
var factEdits: [String: FactEdit] = [:]

// Fuel Type Availability
factEdits["fuel_type_octane_87"] = FactEdit(value: "available", currency: nil, volume: nil)
factEdits["fuel_type_octane_91"] = FactEdit(value: "available", currency: nil, volume: nil)

// Fuel Variants (Ethanol Content)
factEdits["fuel_variant_87_e10"] = FactEdit(value: "available", currency: nil, volume: nil)
factEdits["fuel_variant_91_e10"] = FactEdit(value: "available", currency: nil, volume: nil)

// Prices (Canadian format - single price per fuel)
factEdits["octane_87_e10"] = FactEdit(value: "1.679", currency: "CAD", volume: .liters)
factEdits["octane_91_e10"] = FactEdit(value: "1.879", currency: "CAD", volume: .liters)

// Basic Amenities
factEdits["amenity_restrooms"] = FactEdit(value: "true", currency: nil, volume: nil)
factEdits["amenity_convenience_store"] = FactEdit(value: "true", currency: nil, volume: nil)

// Business Hours (limited hours)
factEdits["business_hours"] = FactEdit(value: "Mo-Su 06:00-23:00", currency: nil, volume: nil)

await GroundTruthService.saveFacts(identifier: stationIdentifier, facts: factEdits)
```

## Key Differences from US Stations

1. **Single Pricing**: No `_cash` or `_credit` suffixes
2. **Currency**: CAD instead of USD  
3. **Volume**: Liters instead of gallons
4. **Fact Keys**: Use base fuel identifiers without payment method
5. **Regional Logic**: Determined by `region.supportsCashCreditDistinction == false`

## Implementation Note

The same Swift code handles both US and Canadian stations automatically:

```swift
if region.supportsCashCreditDistinction {
    // US: Include payment method suffix
    factKey = "\(baseFuelKey)_\(paymentMethod.rawValue)"
} else {
    // Canada: Use base fuel key without suffix
    factKey = baseFuelKey
}
```