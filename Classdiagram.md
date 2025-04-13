## 2. UML Class Diagram (Mermaid.js)
```mermaid
classDiagram
    class UserAccount {
        +String userID
        +String name
        +String email
        +String password
        +String role
        +register()
        +login()
        +updateProfile()
        +deleteAccount()
    }

    class Location {
        +String locationID
        +String name
        +Float latitude
        +Float longitude
        +saveLocation()
        +updateLocation()
        +deleteLocation()
    }

    class Sensor {
        +String sensorID
        +String type
        +String status
        +String lastReading
        +readData()
        +calibrate()
        +reportError()
    }

    class WeatherStation {
        +String stationID
        +String location
        +String status
        +initialize()
        +sendData()
        +performCheck()
    }

    class WeatherReport {
        +String reportID
        +Date timestamp
        +Float temperature
        +Float humidity
        +Float windSpeed
        +generateReport()
        +validateReport()
        +archiveReport()
    }

    class ForecastRequest {
        +String requestID
        +Date requestTime
        +String forecastType
        +submitRequest()
        +processRequest()
        +returnForecast()
    }

    class Alert {
        +String alertID
        +String type
        +Int severity
        +Date issuedTime
        +Date expiryTime
        +triggerAlert()
        +cancelAlert()
        +notifyUsers()
    }

    UserAccount --> "1" Location : manages
    UserAccount --> "1" ForecastRequest : submits
    Location --> "*" WeatherReport : generates
    WeatherStation --> "*" Sensor : contains
    WeatherReport --> "1" Alert : triggers
    ForecastRequest --> "1" Location : requests
    ForecastRequest --> "1" UserAccount : initiated by
    Sensor --> "1" WeatherStation : belongs to
    WeatherReport --> "1" Location : linked to
```

### 💡 Design Rationale
- **Encapsulation:** Attributes and methods are grouped logically per entity.
- **Relationships:** Arrows express multiplicity and direction (e.g., one station has many sensors).
- **Traceability:** Reflects entity relationships in the domain model and system behavior.
