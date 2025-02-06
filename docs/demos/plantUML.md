# PlantUML - Rendering

PlantUML funktioniert wie Mermaid ([Siehe Abschnitt: Mermaid](docsify/mermaid.md)) - im Wesentlichen wird textuell eine Graphik beschrieben.
So wird zum Beispiel aus:

```markdown
@startuml
Alice -> Bob: Authentication Request
Bob --> Alice: Authentication Response

Alice -> Bob: Another authentication Request
Alice <-- Bob: another authentication Response
@enduml
```

das hier:
```plantuml
@startuml
Alice -> Bob: Authentication Request
Bob --> Alice: Authentication Response

Alice -> Bob: Another authentication Request
Alice <-- Bob: another authentication Response
@enduml
```

> [!TIP]
> Sie müssen als Listing-Sprache "plantuml" eintragen.


## Beispiele von PlantUML

PlantUML ist relativ alt und hat entsprechend viele Features und Graphen zur Auswahl. Der Fokus liegt auf UML-Diagrammen, und da sind sie auch wirklich gut. 

Hier soll mal nur eine kleine Auswahl von Möglichkeiten gezeigt werden. Alle Beispiele finden Sie [hier](http://plantuml.com/de/index) - den jeweiligen Beispielcode finden Sie der Quelltextanzeige von dieser Doku.

### Mindmaping

```plantuml
@startmindmap
* root node
** some first level node
***_ second level node
***_ another second level node
***_ foo
***_ bar
***_ foobar
** another first level node
@endmindmap
```

### Organigramme

```plantuml
@startwbs
* Business Process Modelling WBS
** Launch the project
*** Complete Stakeholder Research
*** Initial Implementation Plan
** Design phase
*** Model of AsIs Processes Completed
**** Model of AsIs Processes Completed1
**** Model of AsIs Processes Completed2
*** Measure AsIs performance metrics
*** Identify Quick Wins
** Complete innovate phase
@endwbs
```

### ERM

```plantuml
@startuml

' hide the spot
hide circle

' avoid problems with angled crows feet
skinparam linetype ortho

entity "Entity01" as e01 {
  *e1_id : number <<generated>>
  --
  *name : text
  description : text
}

entity "Entity02" as e02 {
  *e2_id : number <<generated>>
  --
  *e1_id : number <<FK>>
  other_details : text
}

entity "Entity03" as e03 {
  *e3_id : number <<generated>>
  --
  *e1_id : number <<FK>>
  other_details : text
}

e01 ||..o{ e02
e01 |o..o{ e03

@enduml
```

### Komponentendiagramm

```plantuml
@startuml

package "Some Group" {
  HTTP - [First Component]
  [Another Component]
}
 
node "Other Groups" {
  FTP - [Second Component]
  [First Component] --> FTP
} 

cloud {
  [Example 1]
}


database "MySql" {
  folder "This is my folder" {
	[Folder 3]
  }
  frame "Foo" {
	[Frame 4]
  }
}


[Another Component] --> [Example 1]
[Example 1] --> [Folder 3]
[Folder 3] --> [Frame 4]

@enduml
```

## Include external Puml-Files (TODO : not yet working)

To include a .puml file into your doc, need to use an sitaxe like the !include from default PUML, but surrounded by [[ and ]].

Ex.:

### Section X

```plantuml
@startuml

[[!include ./demo/my-chart.puml]]


@enduml
```