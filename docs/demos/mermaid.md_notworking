# mermaid

Mermaid ist eine Online(und Offline) Graphen-Library, bei der sich aus Text-Befehlen Graphen rendern lassen. Im Wesentlichen funktioniert es so, dass Sie einen Code-Listing mit der "Programmiersprache" 'mermaid' versehen. Docsify generiert dann daraus ein Bild über einen Online-Dienst.

**Beispiel : Basic sequence diagram**

Folgender Code:
```
sequenceDiagram
    Alice ->> Bob: Hello Bob, how are you?
    Bob-->>John: How about you John?
    Bob--x Alice: I am good thanks!
    Bob-x John: I am good thanks!
    Note right of John: Bob thinks a long<br/>long time, so long<br/>that the text does<br/>not fit on a row.

    Bob-->Alice: Checking with John...
    Alice->John: Yes... John, how are you?
```

generiert folgendes Bild:

```mermaid
sequenceDiagram
    Alice ->> Bob: Hello Bob, how are you?
    Bob-->>John: How about you John?
    Bob--x Alice: I am good thanks!
    Bob-x John: I am good thanks!
    Note right of John: Bob thinks a long<br/>long time, so long<br/>that the text does<br/>not fit on a row.

    Bob-->Alice: Checking with John...
    Alice->John: Yes... John, how are you?
```

> [!TIP]
> Der Vorteil von dieser Art der Doku-Erstellung liegt auf der Hand! Sie können Bilder "mitversioniert" direkt in der Dokumentation erstellen. D.h.: Ihre Bilder in der Doku sind immer aktuell!

## Weitere Beispiele und Anwendungen

* [README](https://mermaidjs.github.io/#/README)
* [Commandline um Graphen zu exportieren](https://github.com/mermaidjs/mermaid.cli)
* [LiveEditor](https://github.com/mermaidjs/mermaid-live-editor)


## Beispiele

### Basic flowchart

```mermaid
graph LR
    A[Square Rect] -- Link text --> B((Circle))
    A --> C(Round Rect)
    B --> D{Rhombus}
    C --> D
```


### Larger flowchart with some styling

```mermaid
graph TB
    sq[Square shape] --> ci((Circle shape))

    subgraph A
        od>Odd shape]-- Two line<br/>edge comment --> ro
        di{Diamond with <br/> line break} -.-> ro(Rounded<br>square<br>shape)
        di==>ro2(Rounded square shape)
    end

    %% Notice that no text in shape are added here instead that is appended further down
    e --> od3>Really long text with linebreak<br>in an Odd shape]

    %% Comments after double percent signs
    e((Inner / circle<br>and some odd <br>special characters)) --> f(,.?!+-*ز)

    cyr[Cyrillic]-->cyr2((Circle shape Начало));

     classDef green fill:#9f6,stroke:#333,stroke-width:2px;
     classDef orange fill:#f96,stroke:#333,stroke-width:4px;
     class sq,e green
     class di orange
```




### Flowchart

```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
```

### Sequence diagram

```mermaid
sequenceDiagram
    participant Alice
    participant Bob
    Alice -> John: Hello John, how are you?
    loop Healthcheck
        John -> John: Fight against hypochondria
    end
    Note right of John: Rational thoughts <br> prevail...
    John -> Alice: Great!
    John -> Bob: How about you?
    Bob -> John: Jolly good!
```

#### Loops, alt and opt

```mermaid
sequenceDiagram
    loop Daily query
        Alice->>Bob: Hello Bob, how are you?
        alt is sick
            Bob->>Alice: Not so good :(
        else is well
            Bob->>Alice: Feeling fresh like a daisy
        end

        opt Extra response
            Bob->>Alice: Thanks for asking
        end
    end
```


### Gantt diagram

```mermaid
gantt
        dateFormat  YYYY-MM-DD
        title Adding GANTT diagram functionality to mermaid
        section A section
        Completed task            :done,    des1, 2014-01-06,2014-01-08
        Active task               :active,  des2, 2014-01-09, 3d
        Future task               :         des3, after des2, 5d
        Future task2               :         des4, after des3, 5d
        section Critical tasks
        Completed task in the critical line :crit, done, 2014-01-06,24h
        Implement parser and jison          :crit, done, after des1, 2d
        Create tests for parser             :crit, active, 3d
        Future task in critical line        :crit, 5d
        Create tests for renderer           :2d
        Add to mermaid                      :1d
```