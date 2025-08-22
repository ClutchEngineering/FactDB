# Bathroom Facts

Bathroom facts provide detailed information about restroom facilities at gas stations, including configurations, cleanliness ratings, and amenities.

## Prerequisites

Bathroom facts require `amenity_restrooms = "true"` to be set first.

## Fact Categories

### 1. Bathroom Facilities Overview

#### `bathroom_facilities`
Comma-separated list of available bathroom types.

**Values**: `"mens,womens,family,accessible"` (any combination)

**Example**:
```swift
factEdits["bathroom_facilities"] = FactEdit(value: "mens,womens,accessible", currency: nil, volume: nil)
```

#### `bathroom_facilities_unavailable`  
Comma-separated list of unavailable bathroom types.

**Values**: `"mens,womens,family,accessible"` (any combination)

### 2. Individual Bathroom Configuration

#### Pattern: `bathroom_{type}_config`

**Types**: `mens`, `womens`, `family`, `accessible`

**Values**:
- `"single_stall"` - Single toilet room
- `"multi_stall"` - Multiple stalls
- `"single_unisex"` - Single unisex facility

**Examples**:
```swift
factEdits["bathroom_mens_config"] = FactEdit(value: "multi_stall", currency: nil, volume: nil)
factEdits["bathroom_womens_config"] = FactEdit(value: "multi_stall", currency: nil, volume: nil) 
factEdits["bathroom_family_config"] = FactEdit(value: "single_stall", currency: nil, volume: nil)
```

### 3. Changing Tables

#### Pattern: `bathroom_{type}_changing_table`

**Values**: `"true"` if changing table is available

**Examples**:
```swift
factEdits["bathroom_mens_changing_table"] = FactEdit(value: "true", currency: nil, volume: nil)
factEdits["bathroom_family_changing_table"] = FactEdit(value: "true", currency: nil, volume: nil)
```

### 4. Cleanliness Ratings

#### Pattern: `bathroom_{type}_cleanliness`

**Values**:
- `"bad"` - Poor cleanliness
- `"neutral"` - Average cleanliness  
- `"good"` - Good cleanliness
- `"out_of_order"` - Not functioning

**Examples**:
```swift
factEdits["bathroom_mens_cleanliness"] = FactEdit(value: "good", currency: nil, volume: nil)
factEdits["bathroom_womens_cleanliness"] = FactEdit(value: "neutral", currency: nil, volume: nil)
```

#### Pattern: `bathroom_{type}_last_rated`

Timestamp when cleanliness was last rated (ISO 8601 format).

## Complete Example

```swift
// Step 1: Mark restrooms as available (prerequisite)
factEdits["amenity_restrooms"] = FactEdit(value: "true", currency: nil, volume: nil)

// Step 2: Define available facilities
factEdits["bathroom_facilities"] = FactEdit(value: "mens,womens,family", currency: nil, volume: nil)

// Step 3: Configure each bathroom type
factEdits["bathroom_mens_config"] = FactEdit(value: "multi_stall", currency: nil, volume: nil)
factEdits["bathroom_womens_config"] = FactEdit(value: "multi_stall", currency: nil, volume: nil)
factEdits["bathroom_family_config"] = FactEdit(value: "single_stall", currency: nil, volume: nil)

// Step 4: Specify changing table availability
factEdits["bathroom_family_changing_table"] = FactEdit(value: "true", currency: nil, volume: nil)

// Step 5: Rate cleanliness
factEdits["bathroom_mens_cleanliness"] = FactEdit(value: "good", currency: nil, volume: nil)
factEdits["bathroom_womens_cleanliness"] = FactEdit(value: "good", currency: nil, volume: nil)
factEdits["bathroom_family_cleanliness"] = FactEdit(value: "neutral", currency: nil, volume: nil)

// Submit all facts together
await GroundTruthService.saveFacts(identifier: stationIdentifier, facts: factEdits)
```

## Removal Pattern

When bathroom types are removed, their associated facts are cleared with empty values:

```swift
// Remove family bathroom
factEdits["bathroom_family_config"] = FactEdit(value: "", currency: nil, volume: nil)
factEdits["bathroom_family_changing_table"] = FactEdit(value: "", currency: nil, volume: nil)
factEdits["bathroom_family_cleanliness"] = FactEdit(value: "", currency: nil, volume: nil)
factEdits["bathroom_family_last_rated"] = FactEdit(value: "", currency: nil, volume: nil)
```

## Code References

- **Definition**: `apps/Scout/Scout/Core/Configuration/FactDefinitions.swift:182-206`
- **Configuration Submission**: `apps/Scout/Scout/Core/Services/GroundTruthServiceExtensions.swift:390-439`
- **Rating Submission**: `apps/Scout/Scout/Core/Services/GroundTruthServiceExtensions.swift:590-600`
- **UI Components**:
  - Configuration: `apps/Scout/Scout/Features/Stations/BathroomConfigurationView.swift`
  - Rating: `apps/Scout/Scout/Features/Stations/BathroomRatingView.swift`

## Dependencies

1. `amenity_restrooms` must be `"true"` before any bathroom facts can be set
2. Bathroom configuration must be set before cleanliness ratings
3. Changing table facts depend on the bathroom type being configured

## Validation

- Bathroom types must be: `mens`, `womens`, `family`, `accessible`
- Configuration values must be valid stall types
- Cleanliness ratings must be valid enum values
- Timestamps must be in ISO 8601 format