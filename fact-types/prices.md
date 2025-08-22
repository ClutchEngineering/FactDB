# Price Facts

Price facts store the cost of fuel at gas stations with currency and volume metadata.

## Fact Key Patterns

### US Regions (Cash/Credit Distinction)
```
{fuel_identifier}_cash
{fuel_identifier}_credit
```

### Non-US Regions (Single Price)
```
{fuel_identifier}
```

## Fuel Identifiers

### Gasoline
- `octane_87` - Regular gasoline (87 octane)
- `octane_89` - Mid-grade gasoline (89 octane) 
- `octane_91` - Premium gasoline (91 octane)
- `octane_93` - Super premium gasoline (93 octane)
- `octane_87_e10` - 87 octane with 10% ethanol
- `octane_91_e15` - 91 octane with 15% ethanol

### Diesel
- `diesel` - Regular diesel
- `premium_diesel` - Premium diesel
- `diesel_b5` - Diesel with 5% biodiesel
- `diesel_b20` - Diesel with 20% biodiesel

### Alternative Fuels
- `lpg` - Liquefied petroleum gas
- `hydrogen` - Hydrogen fuel
- `e85` - High ethanol blend

## Value Format

Prices are stored as decimal strings with 3 decimal places:
- `"1.459"` - $1.459 per gallon/liter
- `"4.329"` - $4.329 per gallon/liter

## Metadata

All price facts include:
- **currency**: ISO currency code (`"USD"`, `"CAD"`, etc.)
- **volume**: Unit volume (`.gallons` for US, `.liters` for most others)

## Example Submissions

### US Station (Cash/Credit)
```swift
// 87 octane cash price
factEdits["octane_87_cash"] = FactEdit(
    value: "3.459", 
    currency: "USD", 
    volume: .gallons
)

// 87 octane credit price  
factEdits["octane_87_credit"] = FactEdit(
    value: "3.489", 
    currency: "USD", 
    volume: .gallons
)
```

### Canadian Station (Single Price)
```swift
// 87 octane single price
factEdits["octane_87_e10"] = FactEdit(
    value: "1.679", 
    currency: "CAD", 
    volume: .liters
)
```

## Regional Logic

The payment method suffix is determined by `PriceRegion.supportsCashCreditDistinction`:

```swift
if region.supportsCashCreditDistinction {
    // US: Include payment method
    factKey = "\(baseFuelKey)_\(paymentMethod.rawValue)"
} else {
    // Non-US: Send without payment method suffix  
    factKey = baseFuelKey
}
```

## Code References

- **Conversion Logic**: `apps/Scout/Scout/Core/Models/Extensions/FuelPrice+GroundTruth.swift:8-54`
- **Submission**: `apps/Scout/Scout/Features/PriceSubmission/PriceSubmissionViewModel.swift:106-117`
- **Pattern Definition**: `apps/Scout/Scout/Core/Configuration/FactDefinitions.swift:210-227`

## Validation

- Price values must be positive decimal numbers
- Currency must match the station's region
- Volume unit must match the region's fuel unit
- Payment method suffixes only valid in US regions