# Fuel Type Availability Facts

Fuel type facts indicate which fuel types are available at a gas station.

## Fact Key Pattern

```
fuel_type_{fuel_identifier}
```

## Supported Values

- `"available"` - Fuel type is available
- `"unavailable_temporary"` - Temporarily out of stock
- `"unavailable_permanent"` - Permanently removed
- `"unknown"` - Status unknown

## Standard Fuel Types

### Gasoline (Octane-based)
- `fuel_type_octane_87` - Regular gasoline (87 octane)
- `fuel_type_octane_89` - Mid-grade gasoline (89 octane)
- `fuel_type_octane_91` - Premium gasoline (91 octane)
- `fuel_type_octane_93` - Super premium gasoline (93 octane)
- `fuel_type_octane_85` through `fuel_type_octane_115` - Full octane range

### Alternative Fuels
- `fuel_type_diesel` - Regular diesel fuel
- `fuel_type_premium_diesel` - Premium diesel fuel
- `fuel_type_lpg` - Liquefied petroleum gas
- `fuel_type_hydrogen` - Hydrogen fuel
- `fuel_type_e85` - High ethanol blend (85% ethanol)

## Example Submissions

```swift
// Mark 87 octane as available
factEdits["fuel_type_octane_87"] = FactEdit(value: "available", currency: nil, volume: nil)

// Mark diesel as temporarily unavailable
factEdits["fuel_type_diesel"] = FactEdit(value: "unavailable_temporary", currency: nil, volume: nil)

// Mark premium diesel as permanently removed
factEdits["fuel_type_premium_diesel"] = FactEdit(value: "unavailable_permanent", currency: nil, volume: nil)
```

## Code References

- **Definition**: `apps/Scout/Scout/Core/Configuration/FactDefinitions.swift:12-66`
- **Submission**: `apps/Scout/Scout/Features/Stations/FuelTypeEditorView.swift:370-376`
- **AI Detection**: `apps/Scout/Scout/Features/PriceSubmission/StreamlinedPriceInputView.swift:2571-2583`

## Dependencies

Fuel type availability is required before:
- Setting fuel variants (ethanol %, biodiesel %)
- Submitting prices for that fuel type
- Marking specific fuel variants as available