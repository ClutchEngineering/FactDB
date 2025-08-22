# Bathroom Rating Example

Example showing how to submit bathroom cleanliness ratings and configurations.

## Scenario
- Station with confirmed restroom amenity
- User rating bathroom cleanliness and facilities
- Mixed bathroom types with different configurations

## Complete Bathroom Fact Submission

```swift
var factEdits: [String: FactEdit] = [:]

// PREREQUISITE: Restrooms must be marked as available first
factEdits["amenity_restrooms"] = FactEdit(value: "true", currency: nil, volume: nil)

// Bathroom Facilities Overview
factEdits["bathroom_facilities"] = FactEdit(value: "mens,womens,family,accessible", currency: nil, volume: nil)

// Individual Bathroom Configurations
factEdits["bathroom_mens_config"] = FactEdit(value: "multi_stall", currency: nil, volume: nil)
factEdits["bathroom_womens_config"] = FactEdit(value: "multi_stall", currency: nil, volume: nil)
factEdits["bathroom_family_config"] = FactEdit(value: "single_stall", currency: nil, volume: nil)
factEdits["bathroom_accessible_config"] = FactEdit(value: "single_stall", currency: nil, volume: nil)

// Changing Table Availability
factEdits["bathroom_family_changing_table"] = FactEdit(value: "true", currency: nil, volume: nil)
factEdits["bathroom_accessible_changing_table"] = FactEdit(value: "true", currency: nil, volume: nil)

// Cleanliness Ratings
factEdits["bathroom_mens_cleanliness"] = FactEdit(value: "good", currency: nil, volume: nil)
factEdits["bathroom_womens_cleanliness"] = FactEdit(value: "neutral", currency: nil, volume: nil)
factEdits["bathroom_family_cleanliness"] = FactEdit(value: "good", currency: nil, volume: nil)
factEdits["bathroom_accessible_cleanliness"] = FactEdit(value: "good", currency: nil, volume: nil)

await GroundTruthService.saveFacts(identifier: stationIdentifier, facts: factEdits)
```

## Rating-Only Update

If bathrooms are already configured, users can update just cleanliness ratings:

```swift
var factEdits: [String: FactEdit] = [:]

// Update only cleanliness ratings
factEdits["bathroom_mens_cleanliness"] = FactEdit(value: "bad", currency: nil, volume: nil)
factEdits["bathroom_womens_cleanliness"] = FactEdit(value: "good", currency: nil, volume: nil)

await GroundTruthService.saveFacts(identifier: stationIdentifier, facts: factEdits)
```

## Removing Bathroom Types

When bathroom facilities change, remove types by clearing their facts:

```swift
var factEdits: [String: FactEdit] = [:]

// Update facilities list (remove family bathroom)
factEdits["bathroom_facilities"] = FactEdit(value: "mens,womens,accessible", currency: nil, volume: nil)

// Clear all family bathroom facts
factEdits["bathroom_family_config"] = FactEdit(value: "", currency: nil, volume: nil)
factEdits["bathroom_family_changing_table"] = FactEdit(value: "", currency: nil, volume: nil)
factEdits["bathroom_family_cleanliness"] = FactEdit(value: "", currency: nil, volume: nil)
factEdits["bathroom_family_last_rated"] = FactEdit(value: "", currency: nil, volume: nil)

await GroundTruthService.saveFacts(identifier: stationIdentifier, facts: factEdits)
```

## Valid Configuration Values

- **Config**: `"single_stall"`, `"multi_stall"`, `"single_unisex"`
- **Cleanliness**: `"bad"`, `"neutral"`, `"good"`, `"out_of_order"`
- **Changing Table**: `"true"` (only set if available)
- **Bathroom Types**: `"mens"`, `"womens"`, `"family"`, `"accessible"`