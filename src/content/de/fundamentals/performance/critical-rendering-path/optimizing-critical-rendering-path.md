project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Im Sinne der kürzestmöglichen Zeit bis zum ersten Rendern sind drei Variablen zu optimieren. Konkret muss die Anzahl der kritischen Ressourcen, die Zahl der kritischen Bytes und die Länge des kritischen Pfads minimiert werden.

{# wf_updated_on: 2014-04-27 #} {# wf_published_on: 2014-03-31 #}

# Kritischen Rendering-Pfad optimieren {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Im Sinne der kürzestmöglichen Zeit bis zum ersten Rendern sind drei Variablen zu optimieren:

- The number of critical resources.
- The critical path length.
- The number of critical bytes.

Kritische Ressourcen sind Ressourcen, die das erste Rendern der Seite blockieren könnten. Je weniger Ressourcen sich auf der Seite befinden, desto weniger aufwendig ist es für den Browser, Inhalte auf dem Bildschirm darzustellen, und desto weniger Konflikte treten zwischen der CPU und anderen Ressourcen auf.

Außerdem kann der Browser umso schneller zur Verarbeitung der Inhalte und zu ihrer Darstellung auf dem Bildschirm übergehen, je weniger kritische Bytes herunterzuladen sind. Zur Reduzierung der Anzahl der Bytes können wir die Zahl der Ressourcen verringern, d. h., eliminieren oder nicht-kritisch machen, und zudem die Übertragungsmenge durch Komprimieren und Optimieren der einzelnen Ressourcen minimieren.

Die kritische Pfadlänge schließlich ist eine Funktion des Abhängigkeitsgraphen zwischen allen kritischen Ressourcen, die für die Seite und ihre Bytemengen erforderlich sind. Manche Ressourcen können nur heruntergeladen werden, nachdem eine vorherige Ressource verarbeitet wurde, und je größer die Ressource ist, umso mehr Paketumläufe werden zum Herunterladen benötigt.

**The general sequence of steps to optimize the critical rendering path is:**

1. Analysieren und beschreiben Sie den kritischen Pfad: Anzahl der Ressourcen, Bytes, Länge.
2. Minimieren Sie die Anzahl der kritischen Ressourcen: eliminieren, Download zurückstellen, als asynchron kennzeichnen usw.
3. Optimieren Sie die Reihenfolge, in der die übrigen kritischen Ressourcen geladen werden: Alle kritischen Inhalte sollen so frühzeitig wie möglich heruntergeladen werden, um die Länge des kritischen Pfads zu verkürzen.
4. Optimieren Sie die Anzahl der kritischen Bytes, um die Download-Zeit, d. h. die Zahl der Paketumläufe, zu verringern.

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}