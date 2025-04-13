# Health Tracker Domain Model

## Core Entities

| Entity        | Attributes                          | Methods                     | Relationships                     | Business Rules (Linked to Prior Work)          |
|---------------|-------------------------------------|-----------------------------|-----------------------------------|------------------------------------------------|
| **User**      | `-id: UUID`<br>`-name: String`<br>`-email: String`<br>`-fitnessLevel: Enum` | `+register()`<br>`+updateProfile()` | `1` → `*` Activity<br>`1` → `*` Goal | Max 3 devices (FR-004)<br>30-day account expiration (NFR-003) |
| **Activity**  | `-id: UUID`<br>`-type: Enum`<br>`-startTime: DateTime`<br>`-calories: Float` | `+startTracking()`<br>`+stopTracking()` | `*` ← `1` User<br>`*` → `1` Report | Auto-stop after 24h (UC-102)                   |
| **Goal**      | `-id: UUID`<br>`-targetValue: Float`<br>`-deadline: Date`<br>`-currentProgress: Float` | `+updateProgress()`<br>`+checkCompletion()` | `*` ← `1` User | Notify at 50%/90% progress (UC-203)            |
| **Device**    | `-id: UUID`<br>`-manufacturer: String`<br>`-lastSynced: DateTime` | `+pair()`<br>`+sync()`      | `*` ← `1` User<br>`1` → `*` Activity | Require re-auth every 30d (STD-008)            |
| **Report**    | `-id: UUID`<br>`-period: Enum`<br>`-data: JSON` | `+generate()`<br>`+export()` | `*` ← `1` User<br>`*` ← `*` Activity | Retain for 7 days (UC-501)                     |

## Key Relationships

1. **Association**:
   - `User` → `Activity`: One-to-many (A user can have many activities)
   ```mermaid
   classDiagram
       User "1" -- "*" Activity : "logs"


