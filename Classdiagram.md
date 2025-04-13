classDiagram
    %% Main Classes
    class User {
        -userId: String
        -name: String
        -email: String
        -fitnessLevel: Enum
        +register()
        +updateProfile()
        +addDevice()
    }

    class Activity {
        -activityId: String
        -type: Enum
        -startTime: DateTime
        -duration: Int
        +startTracking()
        +stopTracking()
        +calculateCalories()
    }

    class Goal {
        -goalId: String
        -targetValue: Float
        -deadline: Date
        +updateProgress()
        +checkCompletion()
        +sendNotification()
    }

    class Device {
        -deviceId: String
        -manufacturer: String
        -connectionType: Enum
        +pair()
        +sync()
        +checkBattery()
    }

    class Report {
        -reportId: String
        -generationDate: DateTime
        +generateWeekly()
        +exportPDF()
        +sendByEmail()
    }

    %% Relationships
    User "1" -- "0..3" Device : "uses"
    User "1" -- "1..*" Activity : "logs"
    User "1" -- "0..*" Goal : "sets"
    Activity "1..*" -- "0..*" Report : "included in"
    Goal "1" -- "1..*" Activity : "tracks progress"

    %% Composition
    Report *-- Activity : "contains"

    %% Notes
    note for User "Maximum 3 devices\nper user (FR-004)"
    note for Activity "Auto-stops after\n24h (UC-102)"
    note for Goal "Notifies at 50%/90%\nprogress (UC-203)"
