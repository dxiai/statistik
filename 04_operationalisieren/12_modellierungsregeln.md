# Modellierungsregeln für DAGs

Kausale DAG verfügen über mehrere Regeln, mit denen sich  Effekte  in empirischen Studien vorhersagen lassen. Diese ergeben sich aus den Arrangements der Knoten und Beziehungen. Drei Basismuster sind für die Operationalisierung von Studien wichtig: 

1. Die Kette (orig. *pipe*)
2. Die Verzweigung (orig. *fork*)
3. Der Zusammenstoss (orig. *collider*)

Dieses Basismuster helfen bei der Analyse von Studien. 

## Kette 

Bei der Kette sind die Variablen paarweise von einer anderen Variable abhängig. 

![DAG Kette](https://github.com/dxiai/statistik/raw/main/bilder/dag_pipe.png)

Bei einer Kette wirkt eine unabhängige Variable *indirekt* durch eine andere Variable auf eine Zielvariable. Diese indirekte Wirkung kann sowohl verstärkend als auch abschwächend wirken. 

Die Kette verursacht Wechselwirkungen durch die Hintertür (das sog. Backdoor-Kriterium). 

## Verzweigung

Bei der Verzweigung sind zwei Variablen von einer unabhängigen Variable abhängig. 

![DAG Kette](https://github.com/dxiai/statistik/raw/main/bilder/dag_fork.png)

Bei einer Verzweigung wird durch die gemeinsame Einflussgrösse eine Beziehung zwischen den beiden abhängigen Variablen geschaffen, obwohl diese Beziehung zwischen diesen Variablen nicht existieren muss. 

## Zusammenstoss

Beim Zusammenstoss ist eine Variable von zwei unabhängigen Variablen abhängig. 

![DAG Kette](https://github.com/dxiai/statistik/raw/main/bilder/dag_collider.png)

Ein Zusammenstoss wirkt entgegengesetzt zur Verzweigung:  Durch den gemeinsamen Einfluss kann der Eindruck entstehen, als ob eine Beziehung zwischen den unabhängigen Variablen besteht, wenn nicht alle Merkmalsausprägungen der abhängigen Variable berücksichtigt werden.

## Referenzen

* Barrett, M. (2021). An Introduction to directed graph analysis. [CRAN](https://cran.r-project.org/web/packages/ggdag/vignettes/intro-to-dags.html)
* Pearl, J., & Mackenzie, D. (2018). The book of why: the new science of cause and effect.  Penguin Press. 


