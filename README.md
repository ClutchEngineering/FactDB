# GroundTruth Fact Database Documentation

This directory contains comprehensive documentation of all facts used in the GroundTruth/Scout system, based on the current implementation in the codebase.

## Directory Structure

- **README.md** - This overview document
- **fact-types/** - Detailed documentation for each fact type
- **examples/** - Real-world examples of fact submission patterns

## Fact Types Overview

Scout submits facts to the GroundTruth backend using the `GroundTruthService.saveFacts()` API. All facts follow a key-value structure with optional currency and volume metadata.

### Core Fact Categories

1. **[Fuel Types](fact-types/fuel-types.md)** - Fuel availability at stations
2. **[Fuel Variants](fact-types/fuel-variants.md)** - Specific fuel compositions (ethanol %, biodiesel %)
3. **[Prices](fact-types/prices.md)** - Fuel pricing with regional differences
4. **[Amenities](fact-types/amenities.md)** - Station amenities and services
5. **[Bathrooms](fact-types/bathrooms.md)** - Bathroom facilities and cleanliness ratings
6. **[Business Hours](fact-types/business-hours.md)** - Operating hours and schedule

## Fact Submission Pattern

All facts are submitted using this structure:

```swift
await GroundTruthService.saveFacts(identifier: stationIdentifier, facts: factEdits)
```

Where `factEdits` is a dictionary of `[String: FactEdit]`:

```swift
let factEdit = FactEdit(
    value: "actual_value",     // Required: The fact value
    currency: "USD",           // Optional: For price facts
    volume: .gallons          // Optional: For price facts
)
```

## Regional Differences

- **US Regions**: Support cash vs credit price distinctions
- **Non-US Regions**: Single price per fuel type, no payment method suffix

## Examples

The `examples/` directory contains real-world submission patterns:

- **[Complete Station Setup](examples/complete-station-setup.md)** - Comprehensive US station with all fact types
- **[Canadian Station](examples/canadian-station.md)** - Regional differences in pricing format
- **[Price Update](examples/price-update.md)** - Updating only fuel prices
- **[Bathroom Rating](examples/bathroom-rating.md)** - Bathroom configuration and cleanliness

## Validation

Facts are validated against:
- Predefined patterns in `FactDefinitions.swift`
- Regional compatibility (e.g., cash/credit pricing)
- Fuel type dependencies (e.g., diesel variants require diesel availability)