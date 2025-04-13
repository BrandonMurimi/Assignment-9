
## UML Class Diagram (Mermaid.js)

```mermaid
classDiagram
    class User {
        -String id
        -String name
        -String email
        -String fitnessLevel
        +register()
        +updateProfile()
        +addDevice()
    }

    class Activity {
        -String id
        -String type
        -DateTime startTime
        -Float calories
        +startTracking()
        +stopTracking()
    }

    class Goal {
        -String id
        -Float target
        -Date deadline
        +updateProgress()
        +checkCompletion()
    }

    class Device {
        -String id
        -String manufacturer
        +pair()
        +sync()
    }

    class Report {
        -String id
        -String period
        +generate()
        +export()
    }

    User "1" -- "0..3" Device : uses
    User "1" -- "*" Activity : performs
    User "1" -- "*" Goal : sets
    Report *-- Activity : contains
    Goal ..> Activity : tracks
