classDiagram
 Main Classes with Detailed Members
    class User {
        <<Entity>>
        -userId: String
        -name: String
        -email: String
        -passwordHash: String
        -fitnessLevel: Enum(Beginner, Intermediate, Advanced)
        -birthDate: Date
        +register(email: String, password: String): Boolean
        +login(credentials: String): Session
        +updateProfile(name: String, level: Enum): void
        +addDevice(device: Device): Boolean
        +removeDevice(deviceId: String): void
    }

    class Activity {
        <<Entity>>
        -activityId: String
        -type: Enum(Running, Swimming, Cycling)
        -startTime: DateTime
        -endTime: DateTime
        -caloriesBurned: Float
        -heartRate: Int[]
        +startTracking(): void
        +stopTracking(): void
        +pauseTracking(): void
        +calculateCalories(): Float
        +exportGPX(): File
    }

    class Goal {
        <<Entity>>
        -goalId: String
        -targetValue: Float
        -currentValue: Float
        -deadline: Date
        -isAchieved: Boolean
        +updateProgress(value: Float): void
        +checkCompletion(): Boolean
        +extendDeadline(days: Int): void
        +getProgressPercentage(): Float
    }

    class Device {
        <<Entity>>
        -deviceId: String
        -manufacturer: String
        -model: String
        -connectionType: Enum(Bluetooth, WiFi)
        -firmwareVersion: String
        -batteryLevel: Int
        +pair(): Boolean
        +sync(): ActivityData[]
        +checkBattery(): Int
        +updateFirmware(): Boolean
    }

    class Report {
        <<ValueObject>>
        -reportId: String
        -period: Enum(Weekly, Monthly)
        -creationDate: DateTime
        -metrics: Map~String, Object~
        +generate(userId: String): void
        +export(format: Enum(PDF, CSV)): File
        +sendByEmail(recipient: String): Boolean
    }

    class Notification {
        <<Service>>
        -notificationId: String
        -type: Enum(Email, Push)
        -content: String
        +send(user: User): Boolean
        +schedule(time: DateTime): void
        +logDelivery(): void
    }

    %% Relationships with Multiplicity
    User "1" -- "0..3" Device : "uses"
    User "1" -- "1..*" Activity : "logs"
    User "1" -- "0..*" Goal : "sets"
    Activity "1..*" -- "1" Report : "included in"
    Goal "1" -- "1..*" Activity : "tracks progress"
    User "1" -- "0..*" Notification : "receives"

    %% Composition/Strong Aggregation
    Report *-- Activity : "contains"

    %% Dependency
    Notification ..> User : "depends on"
    Notification ..> Goal : "listens to"

    %% Notes for Business Rules
    note for User "Max 3 devices per user (FR-004)\nPassword encryption (NFR-002)"
    note for Activity "Auto-stop after 24h (UC-102)\nMin 5min duration (BR-205)"
    note for Goal "Daily progress updates (UC-203)\n5% tolerance for achievement (BR-307)"

    %% Stereotypes
    note for Notification : <<Service>>\nHandles all system notifications
