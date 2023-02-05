# Fehlerarten

<div class="alert alert-info" markdown=1>
Dieser Abschnitt ergänzt das Kapitel 7 in Bortz und Schuster (2010). 
</div>

### Fehler 1. und 2. Art

Wir machen bei Messungen Fehler. Wenn wir die Grundgesamtheit nicht kennen (was meistens der Fall ist), können wir diese Fehler nicht zuverlässig bestimmen. 

Im Abschnitt zur [Konfidenz] haben wir schon von Wahrscheinlichkeiten gesprochen mit denen wir eine Nullhypothese akzeptieren oder ablehnen wollen. Weil wir aber die tatsächlichen Werte weder für die Nullhypothese noch für die Alternativhypothese kennen, müssen wir prinzipiell davon ausgehen, dass wir *zufällige*  Messfehler machen. 

Aus den zufälligen Messfehlern ergeben sich zwei mögliche Konsequenzen: 

1. Wir akzeptieren die Nullhypothese, obwohl sie nicht gilt (Fehler 1. Art). 
2. Wir verwerfen die Nullhypothese, obwohl sie gilt (Fehler 2. Art).

Wir können uns die Situation mit der folgenden Kreuztabelle veranschaulichen.

| | \\( H_0 \\) gilt | \\( H_0 \\) gilt nicht | 
| :--- | :---: | :---: |
| \\( H_0 \\) akzeptiert | Kein Fehler | Fehler 2. Art | 
| \\( H_0 \\) verworfen | Fehler 1. Art |  Kein Fehler | 

Der Fehler 1. Art hängt direkt mit unserem Signifikanzniveau \\( \alpha \\) zusammen. 

Beim Fehler 2. Art ist es jedoch etwas komplizierter:  Dieser Fehler hängt mit der Alternativhypothese und *deren* Verteilung zusammen. Betrachten wir nun die Verteilung der Null- und der Alternativhypothese, dann erwarten wir einen Unterschied zwischen den beiden Verteilungen. Diesen Unterschied bezeichnen wir als **Effektgrösse**. 

Wir können uns die Effektgrösse als den Abstand zwischen den Mittelwerten der beiden Verteilungen vorstellen. Häufig tritt die Situation ein, dass die Effektgrösse klein ist und dadurch das Signifikanzniveau für die Alternativhypothese deutlich grösser als das Signifikanzniveau der Nullhypothese ist. Dieses Ungleichgewicht der Signifikanzniveaus begünstigt Fehler 2. Art. 

Die folgende Abbildung stellt dieses Ungleichgewicht grafisch dar. Dabei stellen die Kurven jeweils die Wahrscheinlichkeit dar, mit der ein gemessener Wert vom tatsächlichen aber unbekannten Wert abweicht. Die gestrichelten Linien zeigen die tatsächlichen Werte für die Null- und die Alternativhypothese. Beachten Sie, dass die diese Werte in der Realität **nicht** kennen. Der graue Bereich zeigt die Wahrscheinlichkeit für Fehler 1. Art und der rote Bereich die Wahrscheinlichkeit für Fehler 2. Art. 

<img src="https://github.com/dxiai/statistik/raw/main/bilder/norm_vis/norm_errors_1_2.png">

<div class="alert alert-warning" markdown=1>
Grundsätzlich gilt, dass der *p-Wert* der meisten Teststatistiken Ihnen einen Hinweis auf die Wahrscheinlichkeit Fehler 1. Art gibt. Weil der p-Wert von der Verteilung der Nullhypothese abhängt, sagt dieser nichts über Fehler 2. Art aus. 
</div>

<div class="alert alert-success" markdown=1>
Weil die tatsächliche Effektgrösse unbekannt ist, verlassen wir uns bei der Untersuchung einer  Hypothese nicht auf einen einzigen Test, sondern nähern uns der Hypothese über den Ausschluss mehrerer *unabhängiger* Nullhypothesen. 
</div>

<div class="col-md-12 text-center h4">
    <a href="https://moodle.zhaw.ch/course/view.php?id=7569&section=6"><i class="fa fa-lg fa-arrow-up"></i></a>
</div>

$$ $$