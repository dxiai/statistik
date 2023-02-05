# Kodierte Texte in R auswerten

Kodierte Texte sind keine Tabellen, sondern liegen als Word-Dokumente auf unserem Rechner. 

Dieses [Beispiel](https://moodle.zhaw.ch/mod/resource/view.php?id=485346) hat vier Beiträge vom offiziellen Blog des Studienschwerpunkts Marketing der ZHAW kopiert und kodiert. Dabei wurden Bilder entfernt. 

Mit diesem Beispiel untersuchen wir, ob diese Beiträge eine genderneutrale Sprache verwenden. Dazu wurden Substantive jeweils Kategorisiert und mit dem jeweiligen Geschlechtlichkeit (`feminin`, `maskulin`, `neutral`) kodiert. 

### Schritt 1: Datei einlesen und bereinigen

Wenn wir unsere Texte mit Word kodiert haben, können wir sie mit Hilfe der `docxtratr` Bibliothek einlesen. 

```
library(tidyverse)
library(docxtractr)
```

Nun können wir ein kodierten Dokumente  einlesen. 

```
read_docx("kodiert/marketing_1.docx") %>% 
    docx_extract_all_cmnts(include_text = TRUE) -> documentCodes
```

Drei Vektoren sind für uns von besonderer Bedeutung: 

- Der Vektor `comment_text` enthält nun unsere Kodierungen. 
- Der Vektor `id` zeigt uns die Position des jeweiligen Kommentars im Dokument.
- Der Vektor `word_src` enthält den von uns beim Kodieren markierten Text.  

Oft müssen wir unsere Codes und Texte noch bereinigen. In diesem Fall, sind die Codes nicht durchgehend einheitlich geschrieben und in jedem Kommentar stehen zwei Codes. Wir wollen deshalb die Codes trennen und vereinheitlichen. In unserem Fall sind enthalten die Kommentartexte *immer* zwei Kodierungen. Daher können wir die Funktion `separate()` verwenden. Diese Funktion dürfen nur dann verwenden, wenn alle unsere Kommentare die gleiche Anzahl von Codes enthalten. 

```
documentCodes %>%
    select(id, comment_text, word_src) %>% 
    separate(comment_text, into =c("kategorie", "gender"), sep =",") %>%
    mutate(
        gender = gender %>% str_trim() %>% str_to_lower(),
        kategorie = kategorie %>% str_trim() %>% str_to_lower()
    ) -> kodierteDaten
```

Nach diesem Schritt sind unsere Codes vereinheitlicht.

Falls wir unterschiedlich viele Kodierungen in einem Kommentar vorliegen haben, müssen wir die Funktion `str_split()`  und anschliessend `unnest()` verwenden. 

```
# falls wir nicht geschickt kodiert haben oder konnten. 

data %>% 
    select(id, comment_text, word_src) %>% 
    mutate(
        code = comment_text %>% str_split(",")
    ) %>% 
    unnest(code) -> allgemeinereExtraktion
```

Zusätzlich  enthalten die markierten Texte in diesem Beispiel Hyperlinks to externen Seiten. Diese Links stören uns bei  der Analyse und wir entfernen sie deshalb aus den Texten. 

```
kodierteDaten %>%
    mutate(
        word_src = word_src %>% str_remove("HY\\s?\\S+ \"[^\"]+\"[^\"]+\"[^\"]+\" "),
    ) -> kodierteDaten2
```

### Schritt 2: Kategorien organisieren

```
kodierteDaten2 %>% 
    count(kategorie)
```

| kategorie | count | 
| --- | --- |
| aktivität | 7 |
| aufgabe |	1 |
| cas |	10 |
| eigenschaft |	4 |
| fähigkeit |	1 |
| funktion |	5 |
| gruppe |	12 |
| information |	1 |
| kontext |	1 |
| konzept |	1 |
| organisation |	1 |
| person |	6 |
| produkt |	3 |
| technik |	1 |
| zeit |	7 | 


Beachten Sie, dass der vorherige Teilschritt in der Regel **nicht** berichtet wird, weil das Ergebnis nur dazu dient, die Codes den richtigen Variablen zuzuordnen. 
Die Codes im Vektor `Kategorie` gehören zu verschiedenen Variablen. Diese Zuordnung muss explizit erfasst  werden.

```
Akteure = c("person", "organisation", "gruppe")

Studiengang = c("cas", "bsc", "mas", "msc")

Funktion = c("aktivität", "eigenschaft", "fähigkeit",
            "möglichkeit", "funktion", "produkt", "anwendung")

Kontext = c("kontext", "konzept", "technik", "zeit", "information")
```

Wir können so die Kodierungen als Merkmalsausprägungen nominalskalierter Variablen verwenden, beschreiben und auswerten. 

### Schritt 3: Deskriptive Statistik 

Nun können wir die deskriptive Statistik für unsere nominalskalierten Variablen durchführen. Wir erstellen dazu die Kontingenztabellen für den `gender`-Vektor und für die anderen Variablen. Damit die Beschreibung etwas schneller geht, erstellen wir eine kleine Hilfsfunktion. 

```
# Hilfsfunktion 
count_wider = . %>%
    count(kategorie, gender) %>% 
    pivot_wider(names_from = kategorie, values_from = n)
```

Nun können wir die Auswertung schnell erledigen: 

```
kodierteDaten2 %>% 
    filter(kategorie %in% Akteure) %>% 
    count_wider()

kodierteDaten2 %>% 
    filter(kategorie %in% Studiengang) %>% 
    count_wider()

kodierteDaten2 %>% 
    filter(kategorie %in% Funktion) %>% 
    count_wider()

kodierteDaten2 %>% 
    filter(kategorie %in% Kontext) %>% 
    count_wider()
```

> Die Ergebnisse werden hier aus Platzgründen ausgelassen. Sie sind angehalten, die Ergebnisse selbst zu reproduzieren.

### Schritt 4: Schliessende Statistik

Prüfen wir ganz allgemein, ob der Text Genderneutral formuliert wurde. 

```
kodierteDaten2 %>% 
    chisq_test(gender ~ kategorie) 
```

| statistic | chisq_df | p_value |
| --- | --- | --- | 
| 32.93597 | 22 | 0.06278023 |

Der p-Wert ist grösser als 0.05. Wir sollten daher die Nullhypothese des Chi-Quadratunterschiedstests *nicht* verwerfen. Das bedeutet, dass zwischen den vorliegenden Merkmalsausprägungen kein statistischer Unterschied zwischen den Kategorien und dem Wortgeschlecht festgestellt werden konnte. In diesem Fall würden wir festhalten, dass der Text Genderneutral verfasst wurde. 

Prüfen wir, ob die benannten Akteure ebenfalls geschlechterneutral bezeichnet wurden.

```
kodierteDaten2 %>% 
    filter(kategorie %in% Akteure) %>%
    chisq_test(gender ~ kategorie) 
```

| statistic | chisq_df | p_value |
| --- | --- | --- | 
| 3.5 | 4 | 0.4778783 |

In diesem Fall können wir ebenfalls von einer geschlechterneutralen Wortwahl ausgehen. Das hat schon die Kontingenztabelle in der deskriptiven Statistik angedeutet. Hier müssen wir zusätzlich bedenken, dass der Stichprobenunfang der untersuchten Teilstichprobe recht klein (n = 6) ist.

#### Weiterführende Textanalyse

Wir können nun weiteranalysieren und besonders häufige Worte für unsere Codes auswerten.

Wir extrahieren die einzelnen Worte aus der jeweiligen Markierung und entfernen Artikel und andere oft benutzte Worte aus unseren Daten ([Stichwort "Stopwords"](https://moodle.zhaw.ch/mod/page/view.php?id=418486)). 

Anschliessend zählen wir die verbleibenden Worte nach Kategorien und Gender.

Damit das Ergebnis einfacher zu lesen ist, wird das Ergebnis mit `arrange()` sortiert. Weil in unserem Fall sehr viele Worte nur einmal vorkommen, tragen diese nicht viel zum Gesamtinhalt bei. Daher werden diese Worte mit dem abschliessenden Filter für die Darstellung entfernt. 

```
library(tidytext)

stopwords_DE = tibble(
    word = stopwords::stopwords("de", source = "stopwords-iso")
)

kodierteDaten2 %>% 
    unnest_tokens(word, word_src) %>%
    anti_join(stopwords_DE) %>% 
    count(word, kategorie, gender) %>% 
    arrange(desc(n)) %>% 
    filter(n > 1)
```

| word | kategorie | gender | n |
| --- |  --- |  --- |  --- | 
| cas | cas | maskulin | 9 |
| marketing | funktion | neutral | 3 |
| mitstudierenden | gruppe | neutral | 3 |
| austausch | aktivität | maskulin | 2 |
| digital | cas | maskulin | 2 |
| marketing | cas | maskulin | 2 |
| netzwerk | gruppe | neutral | 2 |
| npo | cas | maskulin | 2 |
| referenten | gruppe | maskulin | 2 |

Solche Auswertungen geben zusätzliche Einblicke in die Inhalte und helfen bei der Interpretation der Daten. 
