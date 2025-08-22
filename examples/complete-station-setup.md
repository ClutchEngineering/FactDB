# Complete Station Setup Example

This example shows a comprehensive fact submission for a typical US gas station with multiple fuel types, amenities, and facilities.

## Station Profile
- **Location**: US (supports cash/credit pricing)
- **Type**: Full-service station with convenience store
- **Fuel Types**: Regular (87), Premium (91), Diesel
- **Amenities**: Restrooms, convenience store, ATM
- **Hours**: 24/7 operation

## Complete Fact Submission

```swift
var factEdits: [String: FactEdit] = [:]

// STEP 1: Fuel Type Availability
factEdits["fuel_type_octane_87"] = FactEdit(value: "available", currency: nil, volume: nil)
factEdits["fuel_type_octane_91"] = FactEdit(value: "available", currency: nil, volume: nil)
factEdits["fuel_type_diesel"] = FactEdit(value: "available", currency: nil, volume: nil)

// STEP 2: Fuel Variants (Ethanol Content)
factEdits["fuel_variant_87_e10"] = FactEdit(value: "available", currency: nil, volume: nil)
factEdits["fuel_variant_91_e10"] = FactEdit(value: "available", currency: nil, volume: nil)
factEdits["diesel_b5"] = FactEdit(value: "available", currency: nil, volume: nil)

// STEP 3: Fuel Prices (US format with cash/credit)
factEdits["octane_87_cash"] = FactEdit(value: "3.459", currency: "USD", volume: .gallons)
factEdits["octane_87_credit"] = FactEdit(value: "3.489", currency: "USD", volume: .gallons)
factEdits["octane_91_cash"] = FactEdit(value: "3.859", currency: "USD", volume: .gallons)
factEdits["octane_91_credit"] = FactEdit(value: "3.889", currency: "USD", volume: .gallons)
factEdits["diesel_cash"] = FactEdit(value: "3.649", currency: "USD", volume: .gallons)
factEdits["diesel_credit"] = FactEdit(value: "3.679", currency: "USD", volume: .gallons)

// STEP 4: Amenities
factEdits["amenity_restrooms"] = FactEdit(value: "true", currency: nil, volume: nil)
factEdits["amenity_convenience_store"] = FactEdit(value: "true", currency: nil, volume: nil)
factEdits["amenity_atm"] = FactEdit(value: "true", currency: nil, volume: nil)
factEdits["amenity_car_wash"] = FactEdit(value: "false", currency: nil, volume: nil)
factEdits["amenity_air_pump"] = FactEdit(value: "true", currency: nil, volume: nil)

// STEP 5: Bathroom Details (requires amenity_restrooms = "true")
factEdits["bathroom_facilities"] = FactEdit(value: "mens,womens", currency: nil, volume: nil)
factEdits["bathroom_mens_config"] = FactEdit(value: "multi_stall", currency: nil, volume: nil)
factEdits["bathroom_womens_config"] = FactEdit(value: "multi_stall", currency: nil, volume: nil)
factEdits["bathroom_mens_cleanliness"] = FactEdit(value: "good", currency: nil, volume: nil)
factEdits["bathroom_womens_cleanliness"] = FactEdit(value: "good", currency: nil, volume: nil)

// STEP 6: Business Hours
factEdits["business_hours"] = FactEdit(value: "24/7", currency: nil, volume: nil)

// Submit all facts together
await GroundTruthService.saveFacts(identifier: stationIdentifier, facts: factEdits)
```

## Validation Rules Applied

1. **Fuel Dependencies**: Fuel variants require base fuel types to be available
2. **Price Dependencies**: Prices require fuel types to be available
3. **Bathroom Dependencies**: Bathroom details require `amenity_restrooms = "true"`
4. **Regional Logic**: US region enables cash/credit price distinction
5. **Currency/Volume**: Prices include metadata, other facts don't

## Result

This submission creates a fully documented gas station with:
- 3 fuel types with pricing
- 5 amenities (4 available, 1 unavailable)
- Bathroom facilities with cleanliness ratings
- 24/7 operating hours
- Regional compliance for US pricing standards