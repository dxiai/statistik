# Mittlere absolute Abweichung vom Median

Der Interquartilsabstand (IQR) ist ein Standardmass, dass sich aus den Quartilsgrenzen ergibt. Der IQR gibt aber nur über einen Teil der Stichprobe Auskunft, nämlich genau über die Hälfte der Stichprobe. Das liegt daran, dass der IQR in den Grenzen des 2. und 3. Quartils definiert ist. Weil jedes Quartil genau ein Viertel einer Stichprobe umfasst, decken 2 Quartile die Hälfte der Stichprobe ab. 

Damit wir die gesamte Stichprobe beschreiben können, wünschen wir uns ein Mass für die Streuung über die gesamte Stichprobe. Dieses Mass ist die sog. **Mittlere absolute Abweichung vom Median** (MAD).  Wie der IQR oder die Standardabweichung ist dieses Mass ein Kennwert für die Varianz einer Stichprobe. 

Dazu berechnen wir die Abstände der jeweiligen Merkmalsausprägung von Median. Weil sich aus der Definition des Medians ergibt, dass 50% unserer Werte unterhalb und 50% oberhalb des Medians liegen müssen wir darauf achten, dass sich die Differenzen nicht aufheben. Deshalb berechnen wir den Absolutbetrag der Differenzen. Damit erhalten für jede gemessene Merkmalsausprägung einen Wert für den Abstand zum Median. Für diese Werte bilden wir das arithmetische Mittel.

Das Ergebnis zeigt uns die zentrale Tendenz für alle Werte unserer Stichprobe in Bezug auf den Median. Ist der Wert klein, dann weist das auf insgesamt dicht zusammenliegende Werte hin. Ist dieser Wert gross, dann weist das auf eine breit gestreute Werte hin. 

Das folgende Beispiel stellt das Mittlere absolute Abweichung vom Median (`mad`) und den IQR für das Variablepaar `q10_1`  gegenüber.


```R
stichprobe %>%
    pivot_longer(starts_with("q10_1"), names_to = "variable", values_to = "werte") %>% 
    select(variable, werte) %>%
    drop_na() %>%
    group_by(variable) %>%
    summarise(
        n = n(),
        md = median(werte), 
        iqr = IQR(werte),

        mad_haendisch = (werte - md) %>% abs() %>% median(),
        mad = mad(werte)
    )
```

<table border="1">
<thead>
	<tr><th scope=col>variable</th><th scope=col>n</th><th scope=col>md</th><th scope=col>iqr</th><th scope=col>mad_haendisch</th><th scope=col>mad</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>q10_1_0</td><td>27</td><td>7</td><td>3.5</td><td>1.8888889</td><td>1.8888889</td></tr>
	<tr><td>q10_1_1</td><td>27</td><td>9</td><td>1.5</td><td>0.8888889</td><td>0.8888889</td></tr>
</tbody>
</table>

<div class="col-md-12 text-center h4">
    <a href="https://moodle.zhaw.ch/course/view.php?id=7569&section=4"><i class="fa fa-lg fa-arrow-up"></i></a>
</div>