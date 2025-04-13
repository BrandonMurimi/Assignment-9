classDiagram
    %% Main Classes
    class User {
        -userId: String
        -name: String
        -email: String
        +register()
        +login()
    }

    class Activity {
        -activityId: String
        -type: Enum
        -duration: Int
        +start()
        +stop()
    }

    %% Relationships
    User "1" -- "0..3" Device : "uses"
    User "1" -- "1..*" Activity : "logs"
    Activity "1..*" -- "0..1" Report : "generates"

    %% Notes
    note for User "Max 3 devices (FR-004)"
    note for Activity "Auto-archives after 1 year"
