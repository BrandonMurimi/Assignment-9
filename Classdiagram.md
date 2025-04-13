classDiagram
    %% Health Tracker Class Diagram
    direction TB
    
    class User {
        <<Main Entity>>
        -userId: String
        -name: String
        -email: String
        -fitnessLevel: Enum[Beginner,Intermediate,Advanced]
        +register()
        +updateProfile()
        +addDevice()
        +removeDevice()
    }

    class Activity {
        <<Entity>>
        -activityId: String
        -type: Enum[Run,Swim,Cycle]
        -startTime: DateTime
        -caloriesBurned: Float
        +startTracking()
        +pauseTracking()
        +stopTracking()
    }

    class Goal {
        <<Entity>>
        -goalId: String
        -target: Float
        -deadline: Date
        -progress: Float
        +updateProgress()
        +checkCompletion()
        +generateMotivation()
    }

    class Device {
        <<Entity>>
        -deviceId: String
        -name: String
        -lastSync: DateTime
        +pair()
        +sync()
        +disconnect()
    }

    class Report {
        <<Value Object>>
        -reportId: String
        -period: Enum[Weekly,Monthly]
        +generate()
        +export(format: String)
    }

    %% Relationships
    User "1" --> "0..3" Device : "uses"
    User "1" --> "1..*" Activity : "logs"
    User "1" --> "0..5" Goal : "sets"
    Activity "1..*" --> "0..1" Report : "included in"
    Goal "1" --> "1..*" Activity : "tracks"

    %% Composition
    Report *-- Activity : contains

    %% Notes
    note for User "Max 3 devices per user\n(FR-004 from Assignment 4)"
    note for Activity "Auto-archives after 1 year\n(NFR-001)"
    note for Goal "Notifies at 50%/90% progress\n(UC-203 from Assignment 5)"
