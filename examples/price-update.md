# Price Update Example

Example showing how to update just fuel prices without changing other station facts.

## Scenario
- Existing station with established fuel types and amenities
- Price changes due to market fluctuations
- Only prices need updating

## Price-Only Update

```swift
var factEdits: [String: FactEdit] = [:]

// Update only fuel prices - no need to resubmit fuel types or amenities
factEdits["octane_87_cash"] = FactEdit(value: "3.579", currency: "USD", volume: .gallons)
factEdits["octane_87_credit"] = FactEdit(value: "3.609", currency: "USD", volume: .gallons)
factEdits["octane_91_cash"] = FactEdit(value: "3.979", currency: "USD", volume: .gallons)
factEdits["octane_91_credit"] = FactEdit(value: "4.009", currency: "USD", volume: .gallons)
factEdits["diesel_cash"] = FactEdit(value: "3.769", currency: "USD", volume: .gallons)
factEdits["diesel_credit"] = FactEdit(value: "3.799", currency: "USD", volume: .gallons)

await GroundTruthService.saveFacts(identifier: stationIdentifier, facts: factEdits)
```

## Advantages of Granular Updates

1. **Efficiency**: Only submit changed data
2. **Bandwidth**: Smaller payloads
3. **Audit Trail**: Clear record of what changed
4. **Dependencies**: Existing fuel types remain valid
5. **Atomicity**: All price changes applied together

## Price Confirmation Flow

After submission, prices can be confirmed by other users:

```swift
// Price confirmation (separate from original submission)
factEdits["octane_87_cash"] = FactEdit(value: "3.579", currency: "USD", volume: .gallons)
await GroundTruthService.saveFacts(identifier: stationIdentifier, facts: factEdits)
```

Users can confirm prices at different intervals based on app logic and user permissions.