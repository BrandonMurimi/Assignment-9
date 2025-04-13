Reflection on Domain Modeling and Class Diagram Design
1. Challenges Faced
Abstraction Challenges
One of the most significant challenges was determining the right level of abstraction for each class. Initially, I considered making Activity an abstract class with subclasses like RunningActivity, SwimmingActivity, and CyclingActivity. However, this introduced unnecessary complexity since the differences between activity types were minimal (mostly just metadata like caloriesBurned per type). Instead, I opted for a single Activity class with an Enum type field, simplifying the model while still meeting functional requirements (FR-001).

Key Struggle:

Over-engineering vs. practicality – balancing strict OOP principles with real-world usability.

Deciding whether to model Goal as a stateful entity (tying it to User and Activity) or as an independent class.

Relationship Definitions
Defining relationships between classes required careful consideration:

Composition vs. Aggregation:

Initially, I used aggregation (User ◇─ Device) but realized that if a User is deleted, their paired Device should also be removed from the system. This led me to use composition (User ◆─ Device) instead.

For Report and Activity, composition made sense because reports should not exist without activities.

Multiplicity Constraints:

The business rule "A user can connect up to 3 devices" (FR-004) was enforced via User "1" → "0..3" Device.

Deciding between 1..* (at least one) vs. 0..* (optional) for User → Activity was tricky. I chose 1..* because the system requires at least one logged activity to be useful.

Method Definitions
Overloading vs. Explicit Naming:

Example: Should Activity have a single track() method or separate start(), pause(), stop()?

I chose explicit methods for clarity, aligning with use case steps (UC-102).

Where to Place Business Logic:

Initially, calculateCalories() was in User, but moving it to Activity better followed the Single Responsibility Principle (SRP).

2. Alignment with Prior Assignments
Use Cases (Assignment 5)
The class diagram directly implements user stories from A6:

US-004 (Sync Wearable Data) → Device.sync() method

US-003 (Set Goals) → Goal.updateProgress()

US-005 (Generate Reports) → Report.generate()

State Diagrams (Assignment 8)
The User class’s status field (e.g., Active, Suspended) mirrors the state diagram from A8.

Goal states (InProgress, Achieved, Failed) were modeled as attributes rather than subclasses to avoid inheritance complexity.

Functional Requirements (Assignment 4)
FR-002 (User Profiles) → User.fitnessLevel (Enum)

FR-009 (Data Export) → Report.export(format: String)

NFR-003 (Performance) → Capped User → Device multiplicity to prevent system overload.

3. Trade-offs Made
Inheritance vs. Composition
Option	Pros	Cons	Decision
Inheritance	Clean hierarchy	Tight coupling	Avoided (used Enums instead)
Composition	Flexible, follows SRP	Slightly more boilerplate	Used for Report → Activity
Example Trade-off:
Instead of creating PremiumUser : User (inheritance), I added a subscriptionType: Enum field to User. This:
✅ Simplified the model
✅ Avoided empty method overrides
❌ But meant some conditional checks in methods like generateReport()

Normalization vs. Denormalization
Normalized: Kept Activity and Goal separate for flexibility (supports future analytics).

Denormalized: Added progress directly in Goal (instead of calculating dynamically) for faster reads.

4. Lessons Learned
About Object-Oriented Design
Start Simple, Then Refine

My first draft had 15+ classes; iterating revealed many could be merged (e.g., RunningActivity → Activity with type).

Relationships > Inheritance

Favoring composition reduced fragile hierarchies.

Traceability Is Critical

Linking classes to FR/US numbers prevented scope creep.

Practical Takeaways
Mermaid.js Limitations:

No interfaces → documented them in notes (e.g., <<NotificationService>>).

Agile Feedback Loop:

Early diagram reviews caught missing methods (e.g., Device.checkBattery() wasn’t in initial specs).

What I’d Do Differently
Model Key Exceptions Early

Added DeviceConnectionFailedException after realizing the diagram didn’t handle sync errors.

Use More Value Objects

Could’ve extracted Calories as a value object instead of a primitive Float.


Conclusion
This exercise transformed my approach to OOP:
🔹 Diagrams are living docs – they exposed gaps in requirements.
🔹 Agile + UML work best together – user stories guided method definitions, while state diagrams informed attributes.
🔹 Trade-offs are inevitable – but documenting them (as above) ensures informed future refactoring.

Final Tip: Always validate class diagrams against both use cases (user perspective) and state diagrams (system perspective).
