# Lineare Regression


<div class="col-12 alert alert-primary" markdown=1>
<i class="fa fa-lg fa-info-circle"></i>
Dieser Abschnitt ergänzt das Kapitel 11 "Einfache Lineare Regression" in Bortz und Schuster (2010). 
</div>

Die Lineare Regression hängt eng mit der Korrelation zusammen. Mit der Korrelation prüfen wir wie eng die Werte von zwei Variablen gemeinsam verlaufen. Mit der Linearen Regression untersuchen wir die Zusammenhänge zwischen zwei oder mehr Variablen.

<p class="alert alert-primary" markdown="1">
**Definition:** Die Regressionsanalyse untersucht, ob zwischen zwei oder mehreren Variablen ein *linearer* Zusammenhang festgestellt werden kann. 
</p>

Die Grundlage für die lineare Regression ist die lineare Transformation. In der einfachsten Form basiert diese auf der folgenden Gleichung. 

$$ 
y = c_i + ax 
$$

Dabei sind `x` und `y` jeweils statistische Variablen, die wir empirisch erhoben haben (bzw. erheben wollen).

\\( c_i \\) steht dabei für den sog. Intercept-Wert. Damit wird der Schnittpunkt der Ausgleichsgeraden mit der y-Achse bezeichnet.  Der Wert `a` skaliert die Variable `x`. In der Mathematik haben sie solche Gleichungen als  Funktionen in umgekehrter Schreibweise kennengelernt. 

$$
y = f(x) = ax + c_i
$$

Die Idee ist hier, dass eine *abhängige Variable* `y` von einer *unabhängigen Variablen* `x` beeinflusst wird. Diese Abhängigkeit wird in DAGs als Pfeil zwischen zwei Knoten dargestellt. 

```R
dagify(
     y ~ x
) %>%
    ggdag() +
          theme_dag_blank()
```

![DAG](https://github.com/dxiai/statistik/raw/main/bilder/xy_dag.png)

Der Korrelationskoeffizient hat uns gezeigt, dass diese Beziehung nicht immer exakt sein wird. Daher müssen wir die lineare Beziehung zwischen zwei Variablen durch eine Streuung bzw. einen Fehlerfaktor ergänzen. Entsprechend wird unsere Gleichung etwas komplizierter. 

$$
y = c_i + ax  + e
$$

Der Wert \\( e \\) ist dabei eine zufällige Abweichung vom eigentlichen Wert. Dieser Wert wird als Fehler bezeichnet und ist eine zufällige Abweichung nach oben oder unten von der idealen linearen Beziehung. Aus diesem Wert ergeben sich die sog. **Residuen**. 

<p class="alert alert-primary" markdown="1">
**Definition:** Als **Residuen** oder **Restwerte** werden in der Regressionsanalyse die Werte bezeichnet, die nicht über andere (unabhängige) Variablen erklärt werden können. 
</p>

<p class="alert alert-success" markdown="1">
**Merke:** Der Korrelationskoeffizient ist ein Kennwert für die Streuung der Residuen von einer Ausgleichsgeraden. Ein Korrelationskoeffizient nahe `1` bzw. `-1` steht für eine kleine Streuung. Ein Korrelationskoeffizient nahe 0 steht für eine grosse Streuung.
</p>

In der Statistik messen wir die Variablen `x` und `y`. Uns interessieren bei der Analyse die unbekannten Werte \\( a \\) und \\( c_i \\), weil diese Werte die Wirkung der unabhängigen Variablen auf die abhängige Variable beschreiben. 

<p class="alert alert-primary" markdown="1">
**Definition:** Die **einfache lineare Regression** versucht den Zusammenhang zwischen Variablen durch eine lineare Funktion (d.h. durch eine Gerade) zu erklären.
</p>

<p class="alert alert-primary" markdown="1">
**Definition:** Ein Erklärung zwischen Variablen wird als **Modell** bezeichnet. 
</p>

In R haben wir dieses Modell bereits in unserem DAG festgehalten. Dieses Modell können wir mit der Funktion `lm()` untersuchen.

```R
tibble(
    x = runif(100, 1, 10) ,
    y = runif(100, 1, 10),
) -> data

data %>%
    lm(y ~ x, .) %>%
    summary() 
```

```
Call:
lm(formula = y ~ x, data = .)

Residuals:
    Min      1Q  Median      3Q     Max 
-5.0584 -2.0215 -0.0142  2.9748  4.0595 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)  6.07308    0.64267   9.450 1.89e-15 ***
x           -0.01473    0.10136  -0.145    0.885    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 2.936 on 98 degrees of freedom
Multiple R-squared:  0.0002155,	Adjusted R-squared:  -0.009986 
F-statistic: 0.02112 on 1 and 98 DF,  p-value: 0.8847
```

Dieses Ergebnis zeigt uns im Abschnitt `Residuals` die Kennwerte für die Streuung von `e`. Hier sehen wir eine recht breite Streuung von -5  bis 4.5, was fast den gesamten Wertebereich von 1 und 10 abdeckt. 

Unter `Coefficients` sehen wir die Werte für \\(c_i\\) unter `(Intercept)` und die Variable `x`. Hier interessieren uns vor Allem zwei Werte. Der `Estimate`-Wert bezeichnet den gesuchten Wert für die Ausgleichsgerade. Der `Pr()`-Wert zeigt an, ob dieser Wert wahrscheinlich richtig ist. Ein `Pr()` grösser als 0.05 ist ein Zeichen dafür, dass der unter `Estimate` angezeigte Wert eher nicht die richtige Einflussgrösse abbildet. 

Der Wert für \\( c_i \\) hat für uns meistens keine Bedeutung und so untersuchen wir nur die Werte für `x`. Hier zeigt uns der `Pr()`-Wert an, dass das Modell mit der Variable `x` die Variable `y` sehr wahrscheinlich nicht erklärt. Das ist in diesem Fall nicht verwunderlich, weil wir die beiden Variablen aus unabhängigen Stichproben erzeugt haben. Weil der p-Wert gross ist, erwarten wir hier keine Korrelation. 

```
data %>% 
    summarise(
        cor = cor( y, x) 
    )
```

| cor |
| --- |
| -0.01467917 | 

Dieser Korrelationskoeffizient liegt nahe 0 und zeigt wie erwartete **keine** Korrelation an. 

*Simulieren* wir  nun eine Stichprobe mit einem echten Zusammenhang zwischen den beiden Variablen. Wir legen hier die Werte für `c_i` auf `-1.5` und `a` auf `4` fest. 

Ausserdem legen wir die Streuung für unseren zufälligen Fehler auf 1 fest. 

Den Stichprobenumfang `n` soll `100` sein. 

```R
c_i = -1.5
a = 4
sd_e = 1

n = 100

tibble(
    x = runif(n, 1, 10),
    y = c_i + a * x + rnorm(n, 0, sd_e),
) -> data2 
```

Analysieren wir nun diese *simulierten* Ergebnisse. 

```
data2 %>%
    lm(y ~ x, .) %>%
    summary()
```

```
Call:
lm(formula = y ~ x, data = .)

Residuals:
     Min       1Q   Median       3Q      Max 
-2.36833 -0.72776 -0.06364  0.58568  2.66608 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept) -1.75913    0.27210  -6.465 3.97e-09 ***
x            4.06030    0.04428  91.706  < 2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 1.087 on 98 degrees of freedom
Multiple R-squared:  0.9885,	Adjusted R-squared:  0.9884 
F-statistic:  8410 on 1 and 98 DF,  p-value: < 2.2e-16
```

Bestimmen wir noch schnell den Korrelationskoeffizienten. 

```
data2 %>%
    summarise(
        cor = cor(y,x)
    )
```

| cor |
| --- |
| 0.994224 | 

Wir erkennen, dass unsere simulierte Stichprobe mit 100 Datensätzen eine starke Korrelation zwischen den beiden Variablen aufzeigt. Das erkennen wir ebenfalls aus dem linearen Modell, weil

1. für `x` der Schätzwert (`Estimate`) fast unserem Wert `a` entspricht, und 
2. für `x` der Wert in der Spalte `Pr()` sehr klein ist (`< 2e-16` bzw. < `0.0000000000000002)`. 

Diese Zusammenhang zwischen den beiden Variablen können Sie auch in einem Punktdiagramm darstellen. 

```
data2 %>% 
    ggplot(aes(x, y)) +
        geom_point()
```

![Punktdiagramm für die simulierten Daten](https://github.com/dxiai/statistik/raw/main/bilder/corrr/xy_cor_sim.png)

### Zusammenhang zwischen Korrelation und Streuung

Der Korrelationskoeffizient ist ein Kennwert für die Abweichung vom idealen Zusammenhang. Leider können wir den Korrelationskoeffizienten nicht direkt in den Term für den Fehlerwert eintragen. Gleichzeitig wollen wir den Korrelationskoeffizienten nicht durch umständliches Ausprobieren erzeugen. Für eine Simulation ergibt sich diese Abweichung des Fehlerterms aus der Streuung bzw. Standardabweichung `sd_e` des Fehlerterms. 

Diese Streuung wollen wir aus dem angestrebten Korrelationskoeffizienten erzeugen. Die folgende Formel hilft uns bei der Berechnung der Streuung. 

$$
sd_e \approx 2 \cdot r \cdot a \cdot \mu_x 
$$ 

Damit können wir kontrolliert Stichproben mit den gewünschten Korrelationskoeffizienten erzeugen. 

```R
n = 100

c_i = -1.5 

# Die Vorzeichen von a und des Korrelationskoeffizienten müssen übereinstimmen!
a = 4 
r_cor = .45

min_x = 1
max_x = 10

sd_e = 2 *  r_cor * a * mean(c(min_x, max_x))

tibble(
    x = runif(n, min_x, max_x),
    y = c_i + a * x + rnorm(n, 0, sd_e),
) -> data2c 
```
