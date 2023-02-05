# Tokens: Worte, Sätze und mehr

Nachdem wir Texte eingelesen haben, können wir diese in Einheiten gliedern, die wir untersuchen können. Dieser Schritt wird als ***Tokenisierung*** bezeichnet. 

<p class="alert alert-primary" markdown="1">
**Definition:** Die **Tokenisierung** bezeichnet die systematische Strukturierung eines Texts in kleinere Einheiten. Die Tokenisierung ist ein notwendiger Schritt für die quantitative Textanalyse. 
</p>

Bei der quantitativen Textanalyse entfällt gegenüber der qualitativen Textanalyse das manuelle Kodieren. Häufig werden bei der Textanalyse quantitative und qualitative Methoden *kombiniert*, um Annahmen zu überprüfen. 

Die Textanalyse arbeitet immer im *Kontext* eines Korpus.

<p class="alert alert-primary" markdown="1">
**Definition:** Ein **Korpus** bezeichnet die Texte, die gemeinsam in eine Analyse einfliessen. Ein *Korpus* definiert den **Kontext** der Textanalyse.
</p>

Für die Textanalyse erzeugen wir aus langen Texten mehrere kurze Teiltexte für die weitere Analyse. Diese Teiltexte werden als *Tokens* bezeichnet. 

<p class="alert alert-primary" markdown="1">
**Definition:** Ein **Token** bezeichnet einen Teil eines Texts. 
</p>

Tokens können  *Absätze* (`paragraph`), *Sätze* (`sentence`), *Worte* (`words`), Zeilen (`line`), Seiten (`page`), *Wortfolgen* (`ngram`) oder auch *Buchstabenfolgen* (`character_shingles`) sein. 

> Wenn wir kodieren, dann erzeugen wir ebenfalls Tokens: Der für die Kodierung markierte Text ist ebenfalls ein Token. Beim Kodieren können Tokens mit unterschiedlicher Wortanzahl entstehen. 

### Sätze und Absätze

Sätze und Absätze sind besondere Tokens in Texten. 

<p class="alert alert-primary" markdown="1">
**Definition:** Ein **Satz** bezeichnet einen *inhaltlich zusammenhängenden Teiltext*, der durch einen Punkt (`.`), ein Fragezeichen (`?`) oder ein Ausrufezeichen (`!`) beendet wird. 
</p>

<p class="alert alert-primary" markdown="1">
**Definition:** Ein **Absatz** bezeichnet einen *inhaltlich zusammenhängenden Teiltext*, der mindestens einem Satz besteht. 
</p>

Die Tokenisierung in Absätze und Sätze wird oft dazu verwendet, um grössere Korpora aus inhaltlich zusammenhängenden Texten zu erzeugen. 

### n-Gramme 

Die Gliederung von Texten in n-Gramme hat in der Textanalyse eine besondere Bedeutung. 

<p class="alert alert-primary" markdown="1">
**Definition:** Ein **n-Gram** bezeichnet eine Wortfolge, die aus `n` aufeinanderfolgenden Worten besteht. 
</p>

Im Gegensatz zur Tokenisierung in Worte, werden n-Gramme **nicht durch Stoppworte verzerrt**. Mit Hilfe von n-Grammen lassen sich inhaltlich-relevante Phrasen identifizieren, die bei einer Tokenisierung auf Wortebene verloren gehen würden. 

## Tokens extrahieren

Die Bibliothek `tidytext` stellt viele hilfreiche Funktionen zur quantitativen Textanalyse bereit. Beim Tokenisieren müssen verschiedene Regeln beachtet werden, damit das richtige Ergebnis erzeugt wird. Diese Regeln müssen wir zum Glück nicht im Detail kennen.   Die Bibliothek `tidytext` stellt uns die Funktion [`unnest_tokens()`](https://juliasilge.github.io/tidytext/reference/unnest_tokens.html) sowie ein paar Hilfsfunktionen bereit. Mit diesen Funktionen können wir Texte leicht in Tokens zerlegen. 

Wir laden für die folgenden Beispiele die notwendigen Funktionen und Daten aus der Datei `text/marketing_4.txt` aus den [Beispieltexten.](https://moodle.zhaw.ch/mod/resource/view.php?id=485346) : 

```
library(tidyverse)
library(tidytext)

# 

tibble(
    rohtext = read_file("text/marketing_4.txt")
) -> Rohdaten 
```

Nun können wir die Daten mit Hilfe von `unnest_tokens()` in Tokens zerlegen.

```
Rohdaten %>% 
    unnest_tokens(worte, rohtext)  %>% 
    head()
```

| worte<br>&lt;chr&gt; | 
| --- |
| neues |
| lehrbuch |
| für |
| modernes |
| marketing |
| marketing |

In diesen Beispiel sehen wir die grundsätzliche Arbeitsweise der Funktion. Wir übergeben ein Stichprobenobjekt der Funktion über die Funktionsverkettung. Anschliessend übergeben wir der Funktion als ersten Parameter den Namen des Zielvektors (hier: `worte`) und als zweiten Parameter den Namen des Quellvektors (hier `rohtext`). Alle Texte im Vektor `rohtext` werden durch die Funktion in Worte zerlegt und anschliessend konsequent in die Kleinschreibung überführt. Abschliessend wird der ursprüngliche Vektor `rohtext` aus der Ergebnisstichprobe entfernt. 

### Deutsche Gross- und Kleinschreibung

Während die Gross- und Kleinschreibung im Englischen (mit Ausnahme von Eigennamen) nicht signifikant ist, haben deutsche Worte in Gross- und Kleinschreibung eine andere inhaltliche Bedeutung. Falls diese Bedeutung erhalten bleiben soll, kann der optionale Parameter `to_lower = FALSE` übergeben werden. Der Code ändert sich dann wie folgt: 

```
Rohdaten %>% 
    unnest_tokens(worte, rohtext, to_lower = FALSE)  %>% 
    head()
```

| worte<br>&lt;chr&gt; | 
| --- |
| Neues |
| Lehrbuch |
| für |
| modernes |
| Marketing |
| Marketing |

### Texte in Sätze zerlegen

Wenn wir Texte in Sätze zerlegen wollen, dann verwenden wir die Funktion `unnest_sentences()`.

```
Rohdaten %>% 
    unnest_sentences(saetze, rohtext, to_lower = FALSE)
```

| saetze <br>&lt;chr&gt; |
| --- | 
| Neues Lehrbuch für modernes Marketing. | 
| Marketing wandelt sich im rasanten Tempo. | 
| Auf Kundenwünsche oder neue Technologien muss nicht nur auf der operativen, sondern auch auf der strategischen Ebene in nahezu Echtzeit reagiert werden. | 
| Welche Instrumente und Frameworks dem Marketing dabei zur Verfügung stehen, zeigt das gerade erschienene Lehrbuch des Instituts für Marketing Management: "Marketingmanagement. | 
| Building and Running the Business. | 
| Mit Marketing Unternehmen transformieren" Marketing hat in den vergangenen Jahren einen Paradigmenwechsel durchlaufen. |

Dieser Code ist übrigens identisch mit dem folgenden Code: 

```
Rohdaten %>% 
    unnest_tokens(saetze, rohtext, to_lower = FALSE, token = "sentences")
```

In diesem Beispiel fällt auf, dass `unnest_sentences()` streng entlang den Satztrennzeichen (`. ! ?`) trennt und in Anführungszeichen eingebettete Sätze ebenfalls trennt. Dieses Verhalten kann nur dadurch beeinflusst werden, dass die Texte im Vorfeld entsprechend vorbereitet werden. In solchen Fällen empfiehlt es sich, eingebettete Satzenden durch ein Semikolon (`;`) zu ersetzen. 

In anderen Fällen wollen wir nach dem Trennen die Satzzeichen aus den Sätzen vollständig entfernen. Das erreichen wir mit dem Parameter `strip_punct = TRUE`. Dieser Parameter veranlasst, dass alle Satzzeichen aus den Sätzen entfernt werden. Dazu gehört auch das Zeichen für das Satzende. 

### Absätze trennen

Damit wir Absätze trennen können, müssen wir die Rohtexte entsprechend vorbereitet sein. Absätze werden in Textformaten durch eine zusätzliche Leerzeile markiert. Das weicht von der üblichen Vorgehensweise bei der Arbeit mit Word ab. Dort ist markiert der einfache Zeilenumbruch einen Absatz.

<p class="alert alert-success" markdown="1"> 
Trennen Sie beim Transkribieren mit MS Word Absätze **immer** mit einer zusätzlichen Leerzeile. In dieser Leerzeile dürfen **keine** anderen Symbole stehen (auch keine Leerschläge). Diese Zeile erzeugen Sie durch zwei Zeilenumbrüche mit der Eingabetaste. Sie halten sich so alle Optionen für die nachfolgende Analyse offen.
</p>

Sind die Textdaten entsprechend vorbereitet, dann können wir unsere Texte mit der Funktion `unnest_paragraphs()` in  Absätze gliedern. 

```
Rohdaten %>% 
    unnest_paragraphs(
        saetze, 
        rohtext, 
        to_lower = FALSE, 
        paragraph_break = "\r\n\r\n"
   )  
```

<p class="alert alert-warning" markdown="1"> 
Der zusätzliche Parameter `paragraph_break = "\r\n\r\n"` ist hier notwendig, weil die Daten aus Wort heraus als `Nur Text (.txt)` gespeichert wurden.
</p>

### n-Gramme extrahieren

n-Gramme sind ein wichtiges Werkzeug für einen besseren inhaltlichen Überblick. Worte sind spezielle n-Gramme mit n = 1. Bei dieser Länge haben bestimmte häufig vorkommende Worte (die Stoppworte) die unerwünschte Eigenschaft, das Ergebnis inhaltlich zu verzerren. Durch das Entfernen dieser Worte wird aber immer auch ein Teil der inhaltlichen Bedeutung entfernt. Dieser besondere Effekt von Stoppworten tritt bei n-Grammen mit n > 1 nicht auf. 

n-Gramme extrahieren wir mit der Funktion `unnest_ngrams()`. Dabei wird der Text in Wortsequenzen mit der Länge `n` gegliedert. 

```
Rohdaten %>% 
    unnest_ngrams(ngram, rohtext, to_lower = FALSE) %>% 
    head()
``` 

| ngram<br>&lt;chr&gt; |
| --- |
| Neues Lehrbuch für |
| Lehrbuch für modernes |
| für modernes Marketing |
| modernes Marketing Marketing |
| Marketing Marketing wandelt |
| Marketing wandelt sich |


Dieser Code ist identisch mit dem folgenden Code: 

```
Rohdaten %>% 
    unnest_ngrams(ngram, rohtext, to_lower = FALSE, n = 3) %>% 
    head()
``` 

<div class="alert alert-success" markdown="1"> 
Typische n-Gram-Längen sind

- 3
- 5
- 7

Wobei die n-Gram-Länge von 3 am üblichsten ist. 
</div>


Am Beispiel ist die Arbeitsweise von n-Gram-Tokenisierung erkennbar: Beginnend vom ersten Wort wird eine Sequenz von `n` Worten extrahiert und so lange ein Wort weiter gegangen bis keine Wortsequenz der Länge `n` mehr möglich ist. Dabei ist zu beachten, dass Satz- und Zeilengrenzen nicht automatisch berücksichtigt werden. Um nur inhaltlich zusammenhängende n-Gramme zu erhalten, müssen zwei Tokenisierungen nacheinander vorgenommen werden. Das folgende Beispiel zeigt eine 3-Gram-Zerlegung auf Satzebene. 

```
Rohtext %>% 
    unnest_sentences(saetze, rohtext, to_lower = FALSE)  %>% 
    unnest_ngrams(ngram, saetze, to_lower = FALSE)  %>% 
    head()
```

| ngram<br>&lt;chr&gt; |
| --- |
| Neues Lehrbuch für |
| Lehrbuch für modernes |
| für modernes Marketing |
| Marketing wandelt sich |
| wandelt sich im |
| sich im rasanten |


## Stoppworte

Bei der Analyse von natürlichsprachlichen Texten treten einige Worte besonders häufig auf. Diese Worte lassen für sich allein genommen keine Aussage über den Inhalt eines Texts zu. Gleichzeitig kommen diese Worte so oft vor, dass Sie andere bedeutungsvolle Begriffe überschatten. Sollte Worte behindern die Auswertung von Texten und werden deshalb als *Stoppworte* bezeichnet. 

<p class="alert alert-primary" markdown="1">
**Definition:** Als **Stoppworte** werden Worte bezeichnet, die in einer Sprache so oft verwendet werden, dass sie in praktisch allen Texten vorkommen. Dadurch lassen diese Worte kaum Rückschlüsse auf die Inhalte einzelner Texte zu. 
</p>

Wenn einzelne Worte untersucht werden, dann stören Stoppworte die Auswertung nur. Deshalb werden diese Worte vor der Analyse aus einer Stichprobe entfernt. Für die automatische Textanalyse haben Stoppworte einen so negativen Einfluss, dass es einen internationalen Standard gibt, indem für die einzelnen (Hoch-) Sprachen diese Worte festgehalten sind.  In R greifen wir diese Worte mit der folgenden Operation ab: 

```
stopworte_DE = tibble(
    worte = stopwords::stopwords("de", source = "stopwords-iso")
)
```

Mit dem Parameter `source  = "stopwords-iso"` signalisieren wir, dass wir diese Liste der Worte aus dem offiziellem Standard verwenden möchten. 

<p class="alert alert-warning" markdown="1"> 
Beachten Sie, dass der Vektor `worte` für die Stoppwortliste nicht frei gewählt werden darf. Dieser Vektor **muss** den gleichen Namen haben, wie der Vektor mit den Tokens nach der Tokenisierung mit `unnest_tokens()`.
</p>

Mit dieser Liste und der Funktion ``anti_join()`` können wir nun alle Stoppworte aus einer Stichprobe mit einzelnen Worten entfernen.  

```
tibble( id = 1, text = raw_text) %>% 
    unnest_tokens(worte, text, token = "word") %>%
    anti_join(stopworte_DE) 
```

<p class="alert alert-warning" markdown="1"> 
Die Funktion `anti_join()` arbeitet nur dann korrekt, nachdem die einzelnen Worte durch Tokenisierung aus dem Text extrahiert wurden **und** der Vektor mit den Worten der Stichprobe den identischen Namen hat, wie der Vektor mit der Stoppwortliste. 
</p>

