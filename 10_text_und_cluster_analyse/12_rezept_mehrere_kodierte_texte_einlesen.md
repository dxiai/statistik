# Mehrere Dateien in eine Stichprobe einlesen

Kodierte Texte sind keine Tabellen, sondern liegen als mehrere Dateien auf unserem Rechner vor. Diese Dateien möchten wir möglichst in einem Schritt einlesen und auswerten. 

Die Code-Beispiele basieren auf Dateien aus dem [Beispielpacket](https://moodle.zhaw.ch/mod/resource/view.php?id=485346)

### Schritt 1: Alle Dateien einlesen 

Wenn wir unsere Texte mit Word kodiert haben, können wir sie mit Hilfe der `docxtratr` Bibliothek einlesen. 

```
library(tidyverse)
library(docxtractr)
```

Dazu erstellen wir uns ein Stichprobenobjekt zur Unterstützung, in das wir die Namen der Dateien einlesen.

```
datenordner = "kodiert"

tibble(
    dateien = list.files(
                    path = datenordner, 
                    pattern = "^[^~]+.docx"  # nur richtige Word Dokumente suchen
               )
) %>% 
    mutate(
        textid = row_number()
    ) -> dateinamen
```

Nun können wir die kodierten Dokumente einzeln einlesen. 

```
dateinamen %>% 
    group_by(datei) %>% 
    mutate(
        pfad = str_c(datenordner, "/", datei),
        codes = read_docx(pfad) %>% 
                    docx_extract_all_cmnts(include_text = TRUE) %>% list()
    ) %>% 
    ungroup() %>% 
    unnest(codes) -> alleCodes
```

Mit dieser Operation lesen wir jede einzelne Datei ein. In der Variablen `alleCodes` liegen nun alle vorgenommenen Kodierungen mit den relevanten Zusatzinformationen. 

Beachten Sie, dass Sie mit dem Parameter ``include_text = TRUE`` nicht nur die Kodierung einlesen, sondern auch den Text, der beim Kodieren markiert wurde. 

Mit Hilfe der Vektoren `textid` und `id` können wir jeden Code einer Datei eindeutig zuordnen. 

Wenn Sie im Team kodieren, sollten Sie unbedingt auch die Kodierenden im Vektor ``author`` bzw. ``initials`` behalten. So können Sie überprüfen, ob die gleichen Textstellen mit den gleichen Codes versehen wurden. Dazu müssen Sie für jeden Text mindestens zwei Kodierungen vorliegen haben. Das sog. **Interrater-Agreement** ist dabei ein Kennwert für die Qualität der gemeinschaftlichen Kodierungen. Dabei prüfen Sie, wie sehr sich die  Kodierungen eines Texts unterscheiden.
