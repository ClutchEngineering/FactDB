# GroundTruth Fact Database

Complete documentation of all facts used in the GroundTruth/Scout system. Every single possible fact combination is documented as individual concrete YAML files.

## Structure

**facts/** - Organized directories containing individual YAML files for every concrete fact
- **fuel-types/** - 36 fuel type availability facts
- **fuel-variants/** - 226 fuel composition variant facts  
- **amenities/** - 22 station amenity facts
- **bathrooms/** - 25 bathroom facility facts
- **prices/** - 19 price facts (sample common fuels)
- **business-hours/** - 1 operating schedule fact

329+ individual fact files total, each named with exact fact key (e.g., `fuel_type_octane_87.yaml`)  
Minimal format: `values: ["supported", "values"]`

## Fact Categories

### Fuel Types (`fuel-types/` - 36 facts)
- Octane gasoline: `fuel_type_octane_85.yaml` through `fuel_type_octane_115.yaml` (31 files)
- Alternative fuels: `fuel_type_diesel.yaml`, `fuel_type_premium_diesel.yaml`, `fuel_type_e85.yaml`, `fuel_type_hydrogen.yaml`, `fuel_type_lpg.yaml` (5 files)

### Fuel Variants (`fuel-variants/` - 226 facts)
- Gasoline ethanol blends: `fuel_variant_{octane}_e{ethanol}.yaml` (217 files)
  - All octanes 85-115 × ethanol percentages [0, 5, 10, 15, 25, 85, 100]
- Diesel biodiesel blends: `diesel_b{percentage}.yaml` (9 files)
  - Biodiesel percentages: [0, 2, 5, 7, 10, 15, 20, 30, 100]

### Amenities (`amenities/` - 22 facts)
- All station amenities: `amenity_{id}.yaml`
- Includes car wash sub-types, air pump pricing options, and specialized services

### Bathrooms (`bathrooms/` - 25 facts)
- General facilities: `bathroom_facilities.yaml`
- Per-bathroom-type facts: `bathroom_{type}_{attribute}.yaml`
  - Types: mens_room, womens_room, family_room, accessible, gender_neutral, single_unisex
  - Attributes: cleanliness, config, changing_table, last_rated

### Prices (`prices/` - 19 facts)
- Format: `price_{fuel}.yaml` and `price_{fuel}_{payment}.yaml`
- Regional differences: US regions support cash/credit, non-US single price

### Business Hours (`business-hours/` - 1 fact)
- `business_hours.yaml` - OpenStreetMap format schedule

## File Format

Each YAML file contains:
```yaml
values: ["available", "unknown"]  # All supported values
```

Optional fields only when necessary:
- `description: "Brief description"` 
- `notes: "Essential usage notes"`

## Usage

All facts are submitted using:
```swift
await GroundTruthService.saveFacts(identifier: stationIdentifier, facts: factEdits)
```

Where `factEdits` is `[String: FactEdit]` with:
- `value`: Required string value
- `currency`: Optional for price facts
- `volume`: Optional for price facts

## Completeness

This database contains every single concrete fact combination supported by Scout, with zero legacy content and full codebase accuracy.