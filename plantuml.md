# PlantUML

[PlantUML](https://plantuml.com/) is a highly versatile tool that facilitates the rapid and straightforward creation of a wide array of diagrams.

See: [PlantUML Sandbox](https://www.plantuml.com/plantuml/uml)

## Sequence diagram

```plantuml
@startuml
User -> "Site Front": User starts \nfilling form
"Site Front" -> "Hints Service": Calls API method \ngetSuggestions
"Hints Service" --> "Hints Service": Find results
"Hints Service" -> "Site Front": max. 10 items
"Site Front" -> User: Show max. \n10 items
@enduml
```
