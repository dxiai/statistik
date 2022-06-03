# Einfache Korrelationen simulieren

Über den Korrelationskoeffizienten stellen wir fest, ob zwischen zwei Variablen möglicherweise ein Zusammenhang besteht. Wie können wir solche Zusammenhänge simulieren?

## Metrisch-skalierte Variablen

Wir nehmen zwei normalverteilte Variablen mit einem Mittelwert von \\( \mu_1 \\) und \\( \mu_2 \\) einer Standardabweichung \\( \sigma_1 \\) und \\( \sigma_2 \\). Wir nehmen zusätzlich an, dass die beiden Variablen mit dem Korrelationskoeffizienten \\( \rho \\) korrelieren. Der Korrelationskoeffizient muss zwischen 0 und 1 liegen

Mit diesen Kennwerten können wir aus zwei Zufallsstichproben aus der Normalverteilung, Werte für zwei korrelierende Zufallsvariablen erzeugen, 

```R
N = 100 # Stichprobenumfang

mu_1 = 10
sigma_1 = 2

mu_2 = 3
sigma_2 = .5

rho = 0.6

z_1 = rnorm(N)
z_2 = rnorm(N)

simulierteStichprobe = tibble(
    a1 = z_1 * sigma_1 + mu_1,
    a2 = (rho * z1 + sqrt(1-rho^2) * z2) * sigma_2 + mu_2
) 
```

<p class="alert alert-secondary" markdown="1">
**Übung:** Erzeugen Sie eine Zufallsstichprobe mit zwei metrisch-skalierten Variablen und überprüfen mit der Funktion ``cor()`` den erzeugten Korrelationskoeffizienten.
</p>

## Ordinal-skalierte Variablen

Das gleiche Prinzip können wir zum Simulieren der Korrelation von zwei ordinal-skalierten Variablen anwenden. In in diesem Fall sollten wir jedoch die Binomialverteilung verwenden. Wir ersetzen den Mittelwert durch die Anzahl der Merkmalsausprägungen \\( m_1 \\) und \\( m_2 \\). Die Standardabweichung wird durch die Wahrscheinlichkeiten der Binomialverteilung \\( p_1 \\) und \\( p2 \\) ersetzt. Weil die Ausprägungen der Binomialverteilung bei `0` anfangen, müssen wir die Anzahl der Merkmalsausprägungen um `1` reduzieren.

Weil die Multiplikation mit \\( \rho \\) nicht zu einem diskreten Ergebnis führt, müssen wir die Nachkommastellen aus dem Ergebnis entfernen und weil die Werte von `a2` durch die Addition zu gross werden, müssen wir sie wieder in den Wertebereich unserer Skala bringen. 

Daraus folgt der folgende R Code zum Simulieren einer Korrelation mit einem bekannten Korrelationskoeffizienten. 

```R
N = 100 # Stichprobenumfang

m_1 = 11
p1 = .5

m_2 = 7
p2 = .7

rho = .6

l_1 = m_1 - 1 
l_2 = m_2 - 1 

z1 = rbinom(N, l_1, p1)
z2 = rbinom(N, l_2, p2)

simulierteStichprobe = tibble(
    a1 = z1,
    a2 = trunc(rho * z1 + sqrt(1-rho^2) * z2) - 1
)
```

