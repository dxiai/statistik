# Rezept: Mehrere Unterschiedstests auf einmal Durchführen

Aus unserem Kausalitätsdiagramm können wir die notwendigen Tests direkt ableiten: 

Wir wollen Unterschiede für alle Variablen bestimmen, für welche wir eine kausale Beziehung identifiziert haben.

Aus unserer Fragestellung ergeben sich die Hypothesen, die wir überprüfen müssen.

Wir erfassen die Hypothesen in einer Tabelle für *gleichwertige* Tests. D.h. wir müssen jeweils t-Tests, Wilcoxon-Tests und \\(\chi^2\\)-Tests separat vorbereiten und durchführen. 

### Lösung

```R
# 1. Daten einlesen
sp = read_csv2("notenspiegel.csv")

# 2. Daten bereinigen und gleichwertige Variablen auswählen
sp %>%
    select(Luct, Lkct,  Lgct) %>%
    filter(Lkct > 1) %>%               # und ungültige Werte entfernen
    pivot_longer(everything()) ->
        sp_ct_alle    # R-Variable mit den Daten

# 3.  Tests in einer eigenen Stichprobe/Tabelle festlegen
sp_t_tests = tibble(
    x = c("Lkct", "Lgct", "Lgct"), 
    y = c("Luct", "Luct", "Lkct"),
    richtung = c("less", "less", "two.sided"),
    abhaengige_stichprobe = c(FALSE, TRUE, FALSE)
) 

# 4. Tests durchführen
sp_t_tests %>% 
    group_by(x, y) %>%      # 4.a jeden Test isolieren
    mutate(
        tt = sp_ct_alle %>% 
                filter(name %in% c(x, y)) %>%  # 4.b Variablen aus der Langform extrahieren
                # 4.c der eigentliche Test
                t_test(
                    value ~ name,                    # Die Langform ist immer gleich 
                    order = c(x, y),                 # Die Reihenfolge für gerichtete Hypothesen
                    alternative = richtung,           # Gerichtet oder ungerichtet?
                    paired = abhaengige_stichprobe   # Art des Tests.
                ) %>% 
                list()              # Hilfsliste 
    ) %>% 
    unnest(tt)                 # 4.d Hilfsliste auflösen.
```

### Erklärung

Schritt 1 und 2 sind selbsterklärend. 

In Schritt 3 wird eine Hilfstabelle erstellt. Diese Tabelle hat vier Vektoren. `x` und `y` bezeichnen die zu untersuchenden Variablen. Der Vektor `alternative` zeigt an, ob ein gerichteter oder ein ungerichteter Test durchgeführt werden soll und fall ein gerichteter Tests vorgesehen ist, welche Richtung geprüft werden soll. Der Vektor `abhaengige_stichprobe` zeigt an, ob der jeweilige Test für eine abhängige oder eine unabhängige Stichprobe durchgeführt werden soll. Für die Interpretation ist es hilft zusätzlich, wenn Sie einen Vektor erstellen, der festhält, ob Sie einen Unterschied erwarten oder nicht.

In Schritt 4 erfolgt der eigentliche Test. Dieser Schritt erfolgt in 3 Teilschritten.

Schritt 4.a legt fest, dass jeder Test separat durchgeführt werden soll. Im Beispiel wird nur nach den Variablen gruppiert. Falls mehrere Fälle (gerichtet/ungerichtet) untersucht werden sollen, dann muss zusätzlich nach der `alternative` gruppiert werden. ***Achtung*** Sie dürfen ***niemals*** die Art der Stichprobe variieren!

Schritt 4.b extrahiert aus den Daten in der Langform die relevanten Variablen, die in der Hilfstabelle festgelegt wurden. 

Schritt 4.c führt den eigentlichen Test durch. Hier müssen wir die Funktionen `t_test()`, `chisq_test()` oder `wicolx.test()` wählen. Bei der Funktion `wilcox.test()` müssen wir zusätzlich die `tidy()`-Funktion in die Funktionskette einfügen. Dieser Schritt ist für die anderen beiden Funktionen nicht notwendig. Anschliessend wird das Ergebnis in einer Hilfsliste abgelegt. 

Schritt 4.d löst die Hilfsliste wieder auf, so dass wir die Ergebnisse tabellarisch betrachten können. 

#### Variante für den Wilcoxon-Test

```R
sp_t_tests %>% 
    group_by(x, y) %>%      # 4.a jeden Test isolieren
    mutate(
        tt = sp_ct_alle %>% 
                filter(name %in% c(x, y)) %>%  # 4.b Variablen aus der Langform extrahieren
                # 4.c der eigentliche Test
                wilcox.test(
                    value ~ name,                    # Die Langform ist immer gleich 
                    data = .,
                    alternative = richtung,       # Gerichtet oder ungerichtet?
                    paired = abhaengige_stichprobe   # Art des Tests.
                ) %>% 
                tidy() %>%
                list()              # Hilfsliste 
    ) %>% 
    unnest(tt)                 # 4.d Hilfsliste auflösen.
```


#### Sonderfall nominalskalierte Gruppierungsvariable

Gelegentlich erhalten unsere Daten bereits eine oder mehrere Gruppierungsvariablen (z.B. Geschlecht), die wir überprüfen müssen. In diesem Fall bringen wir nur die zu untersuchenden Variablen in die Langform, jedoch nicht die Gruppierungsvariablen.


```R
digitalesUmfeld = read_csv2("digitales_umfeld.csv")

# 2.  Werte auswählen und in die Langform bringen
sp %>%
    select(digitlisiert, technik_neuste_geräte, geschlecht) %>%
    filter(mob_typ %in% c("Android Smartphone", "iPhone")) %>%
    pivot_longer(c(digitalisiert, starts_with("technik")) ->
        sp_alle

# 3.  Tests in einer eigenen Stichprobe/Tabelle festlegen
sp_wx_tests = tibble(
    x = c("digitalisiert", "technik_neuste_geräte", "digitalisiert"), 
    y = c("geschlecht", "geschlecht", "mob_typ"),
    richtung = c("less", "less", "two.sided"),
    abhaengige_stichprobe = c(FALSE, FALSE, FALSE)
) 

# 4. Tests durchführen
sp_wx_tests %>% 
    group_by(x, y) %>%      # 4.a jeden Test isolieren
    mutate(
        tt = sp_ct_alle %>% 
                filter(name == x) %>%  # 4.b Variablen aus der Langform extrahieren
                select(gruppierer = any_of(y), value) %>% # Gruppierungsvariable vereinheitlichen und Werte auswählen
                # 4.c der eigentliche Test
                Wilcox.test(
                    value ~ gruppierer,    
                    data = .,       
                    alternative = richtung,            # Gerichtet oder ungerichtet?
                    paired = abhaengige_stichprobe   # Art des Tests.
                ) %>% 
               tidy() %>% 
                list()              # Hilfsliste 
    ) %>% 
    unnest(tt)                 # 4.d Hilfsliste auflösen.
```



$$ $$
