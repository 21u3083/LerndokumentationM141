# Alerts 

## Styles 

Es gibt zwei Styles: 

* 'flat' : Hier wird das ganze Feld mit der Farbe gefüllt
* 'callout' : Hier wird das Feld markiert, aber nicht ganzflächtig eingefärbt

> **Default-mässig (wenn nichts angegeben wird) ist der Style auf 'callout' gestellt**  

...dies kann allerdings in der [index.html](../index.html) bei den Optionen angepasst werden. Lassen Sie bei der Definition den Style weg, wird der default genommen.

Beispiele:

```markdown
> [!NOTE|style:flat]
> Dies ist eine Bemerkung vom Typ 'NOTE' mit dem Style 'flat'
```
> [!NOTE|style:flat]
> Dies ist eine Bemerkung vom Typ 'NOTE' mit dem Style 'flat'

```markdown
> [!NOTE|style:callout]
> Dies ist eine Bemerkung vom Typ 'NOTE' mit dem Style 'callout'
```
> [!NOTE|style:callout]
> Dies ist eine Bemerkung vom Typ 'NOTE' mit dem Style 'callout'


## Types

Es gibt per Default 4 verschiedene Bemerkungen/Alerts. Sie sind über 

* NOTE
* TIP
* WARNING
* DANGER

zu nutzen.

Beispiele:

```markdown
> [!NOTE]
> Dies ist eine Bemerkung vom Typ 'NOTE'. Der Style ist defaultmässig auf callout.

> [!TIP]
> Dies ist eine Bemerkung vom Typ 'NOTE'. Der Style ist defaultmässig auf callout.

> [!WARNING]
> Dies ist eine Bemerkung vom Typ 'NOTE'. Der Style ist defaultmässig auf callout.

> [!DANGER]
> Dies ist eine Bemerkung vom Typ 'NOTE'. Der Style ist defaultmässig auf callout.
```

generiert:

> [!NOTE]
> Dies ist eine Bemerkung vom Typ 'NOTE'. Der Style ist defaultmässig auf callout.

> [!TIP]
> Dies ist eine Bemerkung vom Typ 'NOTE'. Der Style ist defaultmässig auf callout.

> [!WARNING]
> Dies ist eine Bemerkung vom Typ 'NOTE'. Der Style ist defaultmässig auf callout.

> [!DANGER]
> Dies ist eine Bemerkung vom Typ 'NOTE'. Der Style ist defaultmässig auf callout.

## Eigene Headings / keine Icons

Alle Optionen bei der Definition lassen sich überschreiben.

Das folgende Beispiel generiert einen Alert, im Type 'flat', mein eigenem Titel, ohne Icon.

```markdown
> [!TIP|style:flat|label:Eigener Titel|iconVisibility:hidden]
> An alert of type 'tip' using alert specific style 'flat' which overrides global style 'callout'.
> In addition, this alert uses an own heading and hides specific icon.
```

> [!TIP|style:flat|label:Eigener Titel|iconVisibility:hidden]
> An alert of type 'tip' using alert specific style 'flat' which overrides global style 'callout'.
> In addition, this alert uses an own heading and hides specific icon.

## Eigene Alerts

...es lassen sich auch eigene Alerts definieren. Genaueres unter [https://github.com/zanfab/docsify-plugin-flexible-alerts](https://github.com/zanfab/docsify-plugin-flexible-alerts)

