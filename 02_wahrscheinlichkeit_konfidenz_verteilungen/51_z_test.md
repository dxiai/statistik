# Der z-Test


<div class="alert alert-info" markdown=1>
Dieser Abschnitt ergänzt das Kapitel 7 in Bortz und Schuster (2010). 
</div>

<div markdown=1 class="alert alert-primary">
Der *z*-Test prüft Hypothesen im Vergleich zu einer *normalverteilten **Grundgesamtheit*** mit bekanntem Mittelwert und bekannter Standardabweichung. 
</div>


#### Der z-Wert

Der z-Wert ist das Gegenstück zum p-Wert. 

<div class="alert alert-primary" markdown=1>
Der z-Wert beschreibt den Wert, der für die *Dichtefunktion* der Standardnormalverteilung einen bestimmten p-Wert ergibt.
</div>

Wir können genauso ausgehend vom p-Wert den zugehörigen z-Wert bestimmen bzw. ausgehend vom z-Wert den zugehörigen p-Wert der Standardnormalverteilung bestimmen. 

<div class="alert alert-success" markdown=1>
Streng genommen ist der z-Wert nur für die Standardnormalverteilung definiert, d.h. für die Normalverteilung mit dem Mittelwert 0 und der Standardabweichung 1. Aus diesem Wert können wir für ein gegebenes Paar von Mittelwerten und Standardabweichungen die zugehörigen Merkmalsausprägung durch eine Lineartransformation bestimmen. Diese Rechenoperation nimmt uns `R` ab, indem wir der `qnorm`  oder der `pnorm`-Funktion die tatsächlichen Werte für den Mittelwert und die Standardabweichung mitgeben.

Wenn ich also im Folgenden vom *z-Wert* schreibe, dann meine ich streng genommen den ***transponierten** z-Wert*.
</div>


#### Durchführung von rechtsseitigen *z*-Tests

Der einfachste *z*-Test ist für rechtsseitige Hypothesen. Unsere Annahme ist also: $$ \bar{x} > \mu_0 $$

Dabei gehen wir wie folgt vor:

1. Das Signifikanzniveau \\( \alpha \\) festlegen, z.B. \\( \alpha = .05 \\) für das 95% Konfidenzintervall. 
2. *z*-Wert für das Konfidenzintervall bestimmen. Dazu brauchen wir den Mittelwert und die Standardabweichung der Grundgesamtheit. 
3. Prüfen, ob die gemessene Merkmalsausprägung grösser (bei rechtsseitigen Hypothesen) bzw. kleiner (bei linksseitigen Hypothesen) des ermittelten z-Werts ist. 

**Beispiel**

Gegeben ist eine Grundgesamtheit mit dem Mittelwert \\( \mu =10 \\) und der Standardabweichung \\( \sigma = 5 \\). Zu prüfen ist das 99% Konfidenzintervall.

Wir haben die folgenden Merkmalsausprägungen:  24, 25, 26, 27, 28 und 29.

Schritt 1: \\( \alpha = .01 \\) 

Schritt 2: *z*-Wert ermitteln

```R
z = qnorm(.99, mean = 10, sd = 7)
```
Das Ergebnis ist `26.2844351182859`

Dieser Wert beschreibt die obere Grenze des Konfidenzintervalls. 

Schritt 3: Überprüfen der Werte 

```R
c(24, 25, 26, 27, 28, 29)  > z
```

In der Praxis werden wir wie folgt vorgehen: 

```R
z = qnorm(.99, mean = 10, sd = 7)

tibble(x = c(24, 25, 26, 27, 28, 29)) %>% 
    mutate(
        abgelehnt = x > z
    )
```

| x | abgelehnt |
|---|---|
| 24 | FALSE |
| 25 | FALSE |
| 26 | FALSE |
| 27 |  TRUE |
| 28 |  TRUE |
| 29 |  TRUE |

#### Durchführung von linksseitigen *z*-Tests

Beim linksseitigen z-Test liegt der Ablehnungsbereich Links vom Mittelwert. In diesem Fall leiten wir *z* aus dem Signifikanzniveau \\( \alpha \\) ab und setzen diesen Wert in die Funktion `qnorm()` ein. 

Diese Vorgehensweise ist korrekt, weil \\( 1 - \alpha \\) unserem Konfidenzintervall entspricht. 

```R
z = qnorm(.01, mean = 10, sd = 7)
```

Bei der Auswertung müssen wir ausserdem beachten, dass nun die Werte zur Ablehnung der Hypothese *kleiner*  als der ermittelte *z*-Wert sind. Entsprechend zur Hypothese dreht sich der Vergleich mit dem z-Wert ebenfalls um.

```R
tibble(x = c(-4, -5, -6, -7, -8, -9)) %>% 
    mutate(
        abgelehnt = x < z
    )
``` 

| x | abgelehnt |
|---|---|
| -4 | FALSE |
| -5 | FALSE |
| -6 | FALSE |
| -7 |  TRUE |
| -8 |  TRUE |
| -9 |  TRUE |

#### Durchführung von ungerichteten *z*-Tests

Beim ungerichteten *z*-Test liegt der Ablehnungsbereich zu gleichen Teilen Links und Rechts vom Mittelwert. In diesem Fall ist der linke und rechte Ablehnungsbereich jeweils \\( \frac{\alpha}{2} \\) gross. Weil wir zwei Ablehnungsbereiche haben, müssen wir auch zwei Grenzen für unser Konfidenzintervall bestimmen.

Die untere (linke)  Grenze des Intervalls ergibt sich wie beim Linksseitigen Test direkt aus dem Ablehnungsbereich, d.h. \\( \frac{ \alpha}{2} \\). Die obere Grenze des Konfidenzintervalls verschiebt sich um diesen Betrag nach oben. Wir dürfen also nicht wie beim rechtsseitigen Hypothesentest das Konfidenzintervall als Grenze verwenden, sondern zusätzlich den Betrag hinzuzählen, den wir am unteren Ende abgezogen haben. Daraus ergibt sich die obere Grenze des Konfidenzintervalls bei \\( I_k + \frac{\alpha}{2} \\).

Bleiben wir bei unserer Beispiel Verteilung, dann bestimmen wir die Grenzen des Konfidenzintervalls in R so: 

```R
z = c(.01/2, .99 + .01/2) %>% qnorm(mean = 10, sd = 7)
```

* -8.0308051248423
* 28.0308051248423

Jetzt können wir unsere Werte mit diesen Grenzen überprüfen.

```R
tibble(x = c(24, 25, 26, 27, 28, 29)) %>% 
    mutate(
        abgelehnt = x < z[1] | x > z[2]
    )
```

| x | abgelehnt |
|---|---|
| 24 | FALSE |
| 25 | FALSE |
| 26 | FALSE |
| 27 |  FALSE |
| 28 |  FALSE |
| 29 |  TRUE |
