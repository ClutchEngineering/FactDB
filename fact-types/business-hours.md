# Business Hours Facts

Business hours facts store the operating schedule for gas stations using OpenStreetMap format.

## Fact Key

```
business_hours
```

## Value Format

Business hours are stored using OpenStreetMap opening hours format:

### Common Formats
- `"24/7"` - Open 24 hours, 7 days a week
- `"Mo-Su 06:00-23:00"` - Monday to Sunday, 6 AM to 11 PM
- `"Mo-Fr 06:00-22:00; Sa-Su 07:00-21:00"` - Different weekday/weekend hours
- `"Mo-Su 05:30-23:30"` - Monday to Sunday with 30-minute intervals

### OpenStreetMap Format Rules
- Days: `Mo` (Monday), `Tu`, `We`, `Th`, `Fr`, `Sa`, `Su`
- Time: 24-hour format `HH:MM`
- Ranges: `Mo-Fr` (Monday to Friday), `Sa-Su` (Saturday to Sunday)
- Multiple periods: Separated by semicolon `;`
- Always on: `24/7`

## Example Submissions

```swift
// 24/7 gas station
factEdits["business_hours"] = FactEdit(
    value: "24/7", 
    currency: nil, 
    volume: nil
)

// Standard weekday/weekend hours
factEdits["business_hours"] = FactEdit(
    value: "Mo-Fr 06:00-22:00; Sa-Su 07:00-21:00", 
    currency: nil, 
    volume: nil
)

// Same hours every day
factEdits["business_hours"] = FactEdit(
    value: "Mo-Su 05:30-23:30", 
    currency: nil, 
    volume: nil
)
```

## Implementation Details

### Data Conversion
The Swift app uses `PlaceDataModel.PlaceDetails.WeeklyHours` which converts to OpenStreetMap format:

```swift
let openStreetMapHours = hours?.openStreetMapFormat()
factEdits[BusinessHoursEditorSheet.factKey] = FactEdit(
    value: openStreetMapHours, 
    currency: nil, 
    volume: nil
)
```

### Backend Processing
Business hours facts trigger special audit tasks:
- `review_required` - Manual review needed
- `osm_sync_required` - Sync to OpenStreetMap

## Code References

- **UI Editor**: `apps/Scout/Scout/Features/Stations/BusinessHours/BusinessHoursEditorSheet.swift:13-34`
- **Fact Key Definition**: `apps/Scout/Scout/Features/Stations/BusinessHours/BusinessHoursEditorSheet.swift:13`
- **Backend Detection**: `backend/groundtruth/backend/services/audit.py:287-298`
- **Test Cases**: `backend/groundtruth/backend/tests/test_cases/submit_business_hours.yaml`

## Validation

- Value must be valid OpenStreetMap opening hours format
- No currency or volume metadata used
- Triggers audit tasks for review and OSM synchronization
- Format validation handled by OpenStreetMap specification

## OpenStreetMap Integration

Business hours facts are designed for synchronization with OpenStreetMap:
- Uses standard OSM opening hours syntax
- Creates `osm_sync_required` audit tasks
- Enables data sharing with OSM community