# Rezept: Eine Co-occurence Matrix erstellen

<p class="alert alert-primary" markdown="1">
**Definition:** Eine **Co-Occurence-Matrix** ist eine spezielle Matrix, in der festgehalten wird, wie häufig zwei Worte in einem Corpus gemeinsam im gleichen Kontext auftreten.
</p>

Eine Co-Occurence-Matrix hat also für jedes Wort im Dokument eine Zeile und eine Spalte. An den Kreuzungspunkte steht jeweils die Häufigkeit, mit der die beiden Worte im gleichen Kontext auftreten. Aus diesem Auftreten können wir inhaltlich zusammengehörende Worte auch über Dokumente hinweg finden. 

Wir können Co-Occurence-Matrizen für verschiedene Szenarien bestimmen. Die Szenarien unterscheiden sich durch den Kontext, in denen das gemeinsame Auftreten bestimmt wird. Solche Kontexte können z.B. der markierte Text einer Kodierung sein oder kurze Freitextantworten in Fragebögen.

### Schritt 1: Daten einlesen

In diesem Beispiel verwenden wir einen Blog-Eintrag des ZHAW Instituts für Marketing. Anstelle des Word-Dokuments verwende ich hier die reine Text-Version des gleichen Inhalts. Diese Version wurde aus dem Word-Dokument mit Hilfe der "Speichern unter"-Option und dem Datenformat `Nur Text (.txt)` erzeugt. Falls wir Absätze berücksichtigen wollen, müssen wir für dieses Format sicherstellen, dass zwischen den einzelnen Absätzen (und Überschriften) jeweils eine zusätzliche Leerzeile (d.h. eine Zeile ohne Symbole - auch keine Leerschläge) ist. 

```
raw_text = tibble(
    docid = 1,
    text = read_file("text//marketing_4.txt")
)
```

Gewöhnen Sie sich an, dass Sie jeden eingelesenen Text mit einer eindeutigen Kennung markieren. Hier ist das der Vektor `docid`. 

<p class="alert alert-success" markdown="1">
Wenn wir mehrere Dokumente einlesen wollen, verwenden wir den gleichen Trick wie zum [Einlesen mehrere kodierter Dokumente](https://moodle.zhaw.ch/mod/page/view.php?id=480899)
</p>

### Schritt 2: Text in Tokens gliedern

Wir zerlegen unseren Text nun zuerst so, dass wir die *Kontext-Tokens* erhalten. Wir wollen hier untersuchen, welche Worte gemeinsam in den gleichen Sätzen auftreten. Dazu wählen für die Funktion `unnest_tokens()` das Token-Format `sentences`. 

```
raw_text %>% 
    unnest_tokens(sentence, text, token = "sentences") %>% 
    mutate(
        satzid = row_number()
    ) -> kontextTokens
```

Damit wir später die Worte den ursprünglichen Sätzen wieder zuordnen können, müssen wir die Sätze ebenfalls mit einer eindeutigen Kennung versehen. Das macht der Anpassung mit `mutate`, in der wir die Zeilennummer (der Stichprobe) als Nummerierung der Sätze verwenden. Zusammen ergeben die beiden Variablen `docid` und `satzid` eine eindeutige Kennung, selbst wenn wir mit vielen Dokumenten arbeiten.

Nun können wir die eigentlichen inhaltlichen Tokens extrahieren. In diesem Fall sind das die Worte. Damit wir nur Worte mit einer inhaltlichen Relevanz in unserer Analyse behalten, entfernen wir die [Stoppworte](https://moodle.zhaw.ch/mod/page/view.php?id=418486) mit `anti_join()`. 

Betrachten wir das Zwischenergebnis nach dem Aufruf von `anti_join()` erkennen wir, dass im Text auch Zahlen vorkommen. Diese haben in diesem Fall keine Bedeutung für uns. Deshalb entfernen wir diese "Worte" mit einem einfachen Filter.  

```
stopworte_DE = tibble(word = stopwords::stopwords("de", source = "stopwords-iso"))

kontextTokens %>%
    unnest_tokens(word, sentence, token = "words") %>% 
    anti_join(stopworte_DE)) %>%
    filter(!(word %>% str_detect("^\\d+$"))) -> inhaltlicheTokens
```

### Schritt 3: Die Co-Occurance-Matrix erstellen

Eine Matrix ist bekanntlich eine Datenstruktur, die nur Werte des gleichen Datentyps enthält. Die Matrixoperationen sind aber nur für Matrizen definiert, die nur Zahlen haben. Wir haben in unserer Stichprobe, bereits die Sätze nummeriert. Nun wollen wir auch die vorkommenden Worte so nummerieren, dass jedes eindeutige Wort im Text eine eindeutige Nummer erhält. 

Um die Worte eindeutig zu nummerieren, extrahieren wir alle Worte und erstellen eine Liste, in der jedes Wort genau einmal vorkommt. 

```
# Eine Liste mit den vorkommenden Worten erstellen
Wortliste = inhaltlicheTokens %>% pull(word) %>% unique()

# Diese Liste wird nummeriert
tibble(word = Wortliste) %>% 
    mutate(wortid = row_number()) -> WortlisteNummeriert
```

Mit dieser Liste weisen wir die erzeugten Nummern unseren Worten in der gesamten Stichprobe zu. Dazu nutzen wir die Funktion `full_join()`. 

```
inhaltlicheTokens %>% 
    full_join(WortlisteNummeriert) -> inhaltlicheTokensNummeriert
```

Nun können wir die Co-Occurence-Matrix erstellen. Dazu benötigen wir zwei Hilfsmatrizen, die das Zählen für uns übernehmen. 

Die erste Hilfsmatrix zeigt an, welche Worte in den gleichen Sätzen vorkommen. Diese  Matrix bezeichnen wir als *Kontextmatrix*. Wir erhalten eine solche Matrix, in der alle Worte als Zeilen und alle Sätze als Spalten abgebildet sind. Eine `1` in dieser Matrix bedeutete, dass das zwei Worte im gleichen Satz vorkommen. 

Damit wir eine solche Matrix erhalten, müssen wir das *äussere Produkt* über die Gleichheit zweier Vektoren bilden. In unserem Fall sind beide Vektoren identisch: nämlich der Vektor `satzids`. 

> Bei Stichproben mit mehreren Dokumenten müssen Sie die Sätze zuerst über *alle* Dokumente nummerieren. Nummerieren sie nicht jeden Satz durch erhalten Sie falsche Zuordnungen für Worte die in unterschiedlichen Dokumenten in Sätzen mit der gleichen `satzid` vorkommen.  

```
# Kontext Matrix erstellen
satzids = inhaltlicheTokensNummeriert %>% pull(satzid)
mKontext = outer(satzids, satzids, "==") * 1
```

Die zweite Matrix die Vorkommnisse in jedem Satz (bzw. an jeder Satzposition) an. Diese Matrix bezeichnen wir als *Inhaltsmatrix*. Das Prinzip für die Inhaltsmatrix ist identisch für die Erstellung der Kontextmatrix: Die Inhaltsmatrix hat so viele Zeilen, wie Worte im Dokument und so viele Spalten wie unterschiedliche Worte vorkommen. Auf diese Weise stellen wir sicher, dass wir am Ende keine Worte doppelt in unserer Co-Occurence-Matrix erfassen. 

```
# Inhaltsmatrix erstellen
wortids = inhaltlicheTokensNummeriert %>% pull(wortid) 
mInhalt = outer(wortids %>% unique(), wortids, "==") * 1
```

Nun können wir die Co-Occurence-Matrix erstellen. Dazu führen wir zwei Matrixmultiplikationen durch. Die erste Multiplikation zählt wie häufig ein Wort mit anderen Worten im gleichen Satz vorkommt. Die zweite Multiplikation zählt die Vorkommisse für die einzelnen eideutigen Wortpaare zusammen. 

```
# Co-Occurance Matrix erstellen
mInhalt %*% mkontext %*% t(mInhalt)  -> mCooc

# Die Matrix spaltenweise beschriften. 

rownames(mCooc) <- Wortliste

# Optional können wir auch die Spalten beschriften
colnames(mCooc) <- Wortliste
```

Das Ergebnis ist eine symmetrische Matrix mit einer Zeile und einer Spalte für jedes eindeutige Wort in unserem Dokument. 

### Schritt 4: Die Co-Occurence-Matrix visualisieren

Die Co-Occurence-Matrix können wir als sog. *Heatmap* darstellen. Damit wir die relevante Information besser erkennen können, entfernen wir aus unserer Matrix alle Einträge mit dem Wert `0`. In diesem Beispiel  entferne ich *für die Visualisierung* auch sog. Einmalereignisse. Das sind Worte, die nur einmal gemeinsam auftreten.

```
mCooc %>% 
    # In ein Stichprobenobjekt für ggplot2 umwandeln
    as_tibble() %>% 
    # Worte für die x-Achse einfügen
    mutate(
        X = Wortliste
    ) %>% 
    # Stichprobe in die Langform bringen. 
    pivot_longer(-X, names_to = "Y") %>%
    # Optional: Die Häufigkeiten als Faktor organiseren
    mutate(
        value =  factor(value)
    ) %>% 
    # Worte alphabetisch sortieren
    arrange(X, Y) %>%
    # Optional: Nicht auftretende Kombinationen und Einmalereignisse entfernen UND 
    # nur die unteren Hälfte  der Matrix behalten (die obere Hälfte kann wegen der
    # Symmetrie vernachlässigt werden).
    filter(value > 1 & X > Y) %>%
    ggplot(aes(X, Y, fill = value)) + 
          # geom_raster() ist wie geom_point() malt aber Quadrate als Raster (s. Heatmap)
          geom_raster() + 
          theme_bw() +
          # Die nächsten beiden Zeilen sorgen dafür, dass ggplot2 
          # keine Worte aus der Liste löscht.
          scale_x_discrete(drop = FALSE) +
          scale_y_discrete(drop = FALSE) +
 
          theme(
            # Achsbeschriftungen der x-Achse so rotieren, dass sie lesbar werden
            axis.text.x = element_text(angle = -90, hjust = 0),
            # Quadratisches Raster erzwingen
            aspect.ratio = 1,
            # Optional: Legende mit den Häufigkeiten verbergen.
            legend.position = "none"
          )
```

![Visualisierung einer Co-Occurence-Matrix](https://github.com/dxiai/statistik/raw/main/bilder/text_analyse/co-occurence-map.png)

In dieser Visualisierung erkennen wir nun gut, welche Wortpaare gemeinsam auftreten, weil nur diese Kreuzungspunkte eingefärbt sind. Damit haben wir sozusagen die "DNA" unseres Dokuments extrahiert und visualisiert. Diese Daten können wir nun mit anderen Dokumenten vergleichen und auf Ähnlichkeiten untersuchen. 