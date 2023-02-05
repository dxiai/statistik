# Störfaktoren

<p class="alert alert-primary" markdown="1">
**Definition:** Als **Störfaktor** wird eine unabhängige Variable bezeichnet, von der eine oder mehrere Variablen mit Bezug auf die Forschungsfrage abhängen. 
</p>

Störfaktoren sind immer dann gegeben, wenn zwischen zwei Variablen ein kausaler Zusammenhang vermutet wird und gleichzeitig andere kausale Zusammenhänge zu anderen Variablen zwischen den beiden Variablen bestehen. Mit anderen Worten: Es gibt mehrere (ungerichtete) Pfade, die die beiden Variablen verbinden.

<p class="alert alert-secondary" markdown="1">
Die folgenden Beispiele nehmen die Variable `a` als abhängige Variable der Variablen `b` an, wobei die Beziehung zwischen den Variablen `a` und `b` untersucht werden soll.  Variable `c` wurde ebenfalls als studienrelevant  identifiziert und deshalb  im DAG abgebildet.
</p>

<p class="alert alert-success" markdown="1">
Beachten Sie, dass alle Beispiele auf dieser Seite topologisch gleich sind: In allen Fällen wirkt eine Variable auf zwei andere Variablen. 
</p>

Das folgende Beispiel zeigt drei Variablen `a`, `b` und `c`. Die Variablen `a` und `b` bilden den Untersuchungsgegenstand der Forschungsfrage ab.

![Beispiel eines DAG mit einem Störfaktor](https://github.com/dxiai/statistik/raw/main/bilder/dag_factor.png)

Die Variablen `a` und `b` sind von der Variable `c` abhängig.  Diese Variable ist eine Störvariable, weil sie sich (a) auf die Zielvariable `a` und (b) indirekt über die Variable `b` auf die Zielvariable wirkt. 

In einer Studie müssen wir Störvariablen besonders behandeln, weil sie die Ergebnisse unserer Studien verzerren. Wir müssen in einer Studie Störvariablen **kontrollieren**, um ihren Einfluss auf die Ergebnisse einzuschränken. Beim Kontrollieren einer Störvariable müssen wir die Beziehung der Variablen `a` und `b` für die Merkmalsausprägungen von `c` separat untersuchen. Damit wird der Effekt auf die anderen Variablen *kontrolliert* und wir können uns auf die tatsächliche Beziehung der Variablen konzentrieren. 

<p class="alert alert-warning" markdown="1">
Eine Störvariable kann die Ergebnisse einer Studie so weit verzerren, dass wir falsche Rückschlüsse aus den Daten ziehen, wenn die Variable nicht kontrolliert wird. 
</p>

In diesem Beispiel ist es möglich, dass eine vermutete Beziehung zwischen `a` und `b` nicht mehr festgestellt werden kann, wenn die Variable `c` kontrolliert wird. In diesem Fall müssen wir unsere Vermutung verwerfen. 

Wir können Störvariablen in einem DAG leicht erkennen, wenn ausgehend von der Variable ein Pfeil auf eine der Variablen unseres Untersuchungsgegenstands direkt oder indirekt zeigt. Im ersten Beispiel stört die Variable `c` beide Variablen unseres Forschungsgegenstands. 

Eine andere Form der Störung entsteht durch Verkettung von Variablenbeziehungen. 

![Beispiel eines DAG mit einem Störfaktor durch Verkettung](https://github.com/dxiai/statistik/raw/main/bilder/dag_factor2.png)

Anders als im ersten Beispiel stört die Variable `c` unsere Studie, weil die Variable `b` sowohl direkt als auch indirekt über die Variable `c` auf die Zielvariable `a` wirkt. Auch hier wollen wir den Einfluss der Variable `c` auf die Zielvariable `a` kontrollieren, um die direkte Beziehung zu untersuchen. 

Eine dritte Variante ergibt sich, wenn die Variable `c` von den Variablen `a` und `b` abhängt. In diesem Fall ist Variable `c` keine unabhängige Variable und ist damit auch kein Störfaktor für unsere Studie.  Diese Beziehung wird in der folgenden Abbildung gezeigt. 

![Beispiel eines DAG ohne Störfaktor C](https://github.com/dxiai/statistik/raw/main/bilder/dag_factor3.png)

<p class="alert alert-warning" markdown="1">
Wenn wir eine abhängige Variable wie eine Störvariable kontrollieren, dann wirkt diese kontrollierte Variable wie ein Störfaktor auf unser Ergebnis. Wir erreichen damit genau das Gegenteil unserer Intention.
</p>
