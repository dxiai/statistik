## Kursorganisation 

Dieser Kurs hat 12 inhaltliche Teile zur den wichtigsten Themen der Statistik. Die Themen wurden so ausgewählt, dass der Schwerpunkt auf der Bedeutung der Techniken für die Anwendung liegt.

In diesem Kurs erhalten Sie zu jedem Thema eine kompakte Einführung. Die Einführung zeigt Ihnen nur die zentralen Konzepte. Für die Erfahrungsnote müssen Sie sich mit der Literatur im Modul auseinandersetzen. Die Erfahrungsnote besteht aus begleitenden Übungen und kurzen Tests, wobei **alle** Übungen und Tests Prüfungsrelevant sind. Zusätzlich werden _zwei Testate_ in die Erfahrungsnote einbezogen.

### Einbettung im Modul

Die Statistik ist Teil des Moduls Daten und Information 2. Das Modul besteht aus dem Kurs _"Empirische Forschungsmethoden"_ und _"Statistik"_. Beide Module sind im organisatorisch und inhaltlich aufeinander abgestimmt. Es gilt im gesamten Modul die gleiche Gruppeneinteilung. Stellen Sie deshalb sicher, dass Sie in Moodle in beiden Modulen in den gleichen Arbeitsgruppen und Präsenzgruppen zugeteilt sind.

Grundsätzlich stehen die Aufgaben im Kurs Statistik für sich und müssen jeweils _einzeln_ abgegeben werden. 

Die Testate beziehen sich auf Ihre Arbeit im anderen Kurs. Diese Testate geben Sie als _Gruppe_ ab und erhalten eine gemeinsame Gruppennote. Die Termine für die Testate hängen von Ihrer Zuteilung in der jeweiligen Präsenzgruppe ab. 

### Arbeitsumgebung

Für alle Übungen werden R-Notebooks auf Basis der JupyterLab Plattform verwendet. Die Arbeitsumgebung wird durch den Dozierenden als Cloud-Dinest bereitgestellt. Allen Studierenden steht damit eine persönliche Arbeitsumgebung online zur Verfügung.

### Lokale Arbeitsumgebung 

Optional kann die Arbeitsumgebung auch lokal auf dem persönlichen Laptop installiert werden. Dazu wird die Software [Docker Desktop](https://www.docker.com/products/docker-desktop) benötigt. 

Die Arbeitsumgebung benötigt auf Ihrem Laptop ca. 5GB Speicherplatz auf Ihrer Festplatte.

Auf der Kommandozeile kann die Arbeitsumgebung mit dem folgenden Befehl gestartet werden: 

Windows
```
docker run -it -p 8888:8888 -v Dokumente:/home/jovyan/ phish108/r-notebook:latest
```

MacOS
```
docker run -it -p 8888:8888 -v ~/Documents:/home/jovyan/ phish108/r-notebook:latest
```

Linux
```
docker run -it -p 8888:8888 -v ~/daten:/home/jovyan/ phish108/r-notebook:latest
```

Dieser Befehl installiert automatisch alle Funktionen und Bibliotheken, die Ihnen auch online zur Verfügung stehen.

Sobald die Arbeitsumgebung gestartet ist, sehen Sie auf der Kommandozeile eine URL, die so ähnlich wie die folgende URL aussieht. 

```
http://127.0.0.1:8888/?token=3d6b...a125
```

Mit dieser URL können Sie auf Ihre lokale Arbeitsumgebung zugreifen. 

Beachten Sie, dass Ihre Dateien nicht automatisch zwischen Ihrer Lokalen und Ihrer Cloud-Arbeitsumgebung synchronisiert werden.
