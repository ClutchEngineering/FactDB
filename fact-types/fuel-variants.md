# Fuel Variant Facts

Fuel variant facts specify the exact composition of fuels available at stations, including ethanol percentages for gasoline and biodiesel percentages for diesel.

## Gasoline Variants

### Fact Key Pattern
```
fuel_variant_{octane}_e{ethanol_percentage}
```

### Examples
- `fuel_variant_87_e0` - 87 octane with 0% ethanol (pure gasoline)
- `fuel_variant_87_e10` - 87 octane with 10% ethanol (E10)
- `fuel_variant_91_e15` - 91 octane with 15% ethanol (E15)
- `fuel_variant_93_e85` - 93 octane with 85% ethanol (E85)

### Supported Ranges
- **Octane**: 85-115
- **Ethanol**: 0-100% (common: 0, 5, 10, 15, 25, 85, 100)

### Value
- `"available"` - This specific variant is available

## Diesel Variants

### Fact Key Pattern
```
diesel_b{biodiesel_percentage}
```

### Examples
- `diesel_b0` - Pure petroleum diesel (0% biodiesel)
- `diesel_b5` - 5% biodiesel blend (B5)
- `diesel_b7` - 7% biodiesel blend (B7)
- `diesel_b20` - 20% biodiesel blend (B20)
- `diesel_b100` - Pure biodiesel (B100)

### Supported Range
- **Biodiesel**: 0-100% (common: 0, 5, 7, 20, 100)

### Value
- `"available"` - This specific variant is available

## Example Submissions

### Gasoline Station with E10
```swift
// Mark base fuel type as available first
factEdits["fuel_type_octane_87"] = FactEdit(value: "available", currency: nil, volume: nil)

// Then specify the variant
factEdits["fuel_variant_87_e10"] = FactEdit(value: "available", currency: nil, volume: nil)
```

### Diesel Station with Biodiesel
```swift
// Mark base diesel as available first  
factEdits["fuel_type_diesel"] = FactEdit(value: "available", currency: nil, volume: nil)

// Then specify the biodiesel variant
factEdits["diesel_b5"] = FactEdit(value: "available", currency: nil, volume: nil)
```

## Parsing Logic

### Gasoline Variants
```swift
// Extract from: fuel_variant_87_e10
static func parseGasolineVariant(_ factKey: String) -> (octane: Int, ethanol: Int)? {
    guard factKey.hasPrefix("fuel_variant_") else { return nil }
    let variant = String(factKey.dropFirst("fuel_variant_".count))
    let components = variant.split(separator: "_")
    guard components.count == 2,
          let octane = Int(components[0]),
          components[1].hasPrefix("e"),
          let ethanol = Int(components[1].dropFirst()) else {
        return nil
    }
    return (octane: octane, ethanol: ethanol)
}
```

### Diesel Variants
```swift
// Extract from: diesel_b5
static func parseBiodieselVariant(_ factKey: String) -> Int? {
    guard factKey.hasPrefix("diesel_") else { return nil }
    let variant = String(factKey.dropFirst("diesel_".count))
    guard variant.hasPrefix("b"),
          let percentage = Int(variant.dropFirst()) else {
        return nil
    }
    return percentage
}
```

## Code References

- **Definition**: `apps/Scout/Scout/Core/Configuration/FactDefinitions.swift:68-146`
- **Gasoline Submission**: `apps/Scout/Scout/Features/PriceSubmission/FuelVariantEditorView.swift:535-545`
- **Diesel Submission**: `apps/Scout/Scout/Features/PriceSubmission/DieselVariantEditorView.swift`

## Dependencies

- Fuel variant facts require the base fuel type to be marked as available first
- Example: `fuel_variant_87_e10` requires `fuel_type_octane_87` = `"available"`
- Example: `diesel_b5` requires `fuel_type_diesel` = `"available"`

## Validation

- Octane must be in range 85-115
- Ethanol percentage must be 0-100
- Biodiesel percentage must be 0-100
- Base fuel type must be available before variants can be set