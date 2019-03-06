project_path: /web/_project.yaml book_path: /web/fundamentals/_book.yaml description: Nach der Entfernung aller unnötigen Ressourcen besteht der nächste Schritt in der Minimierung der Gesamtgröße der verbleibenden Ressourcen, die der Browser herunterladen muss, d. h. in deren Komprimierung über die Anwendung inhaltstypspezifischer und allgemeiner Komprimierungsalgorithmen (GZip).

{# wf_updated_on: 2014-09-11 #} {# wf_published_on: 2014-03-31 #}

# Codierung und Übertragungsgröße textbasierter Ressourcen optimieren {: .page-title }

{% include "web/_shared/contributors/ilyagrigorik.html" %}

Die Webanwendungen werden immer umfangreicher, ehrgeiziger und funktionsreicher - das ist eine gute Sache. Allerdings bewirkt die ungebremste Entwicklung hin zu einem üppigerem Web einen weiteren Trend: die Datenmengen, die von den einzelnen Anwendungen heruntergeladen werden, nehmen kontinuierlich zu. Im Sinne einer hohen Leistungsfähigkeit muss die Bereitstellung eines jeden Datenbytes optimiert werden!

## Datenkomprimierung 101

Nachdem alle unnötigen Ressourcen entfernt wurden, besteht der nächste Schritt in der Minimierung der verbleibenden Ressourcen, die der Browser herunterladen muss, d. h. in deren Komprimierung. Je nach Ressourcentyp - Text, Bilder, Schriften usw. - steht uns eine Anzahl unterschiedlicher Methoden zur Verfügung: allgemeine Tools, die auf dem Server aktiviert werden können, vorbereitende Optimierungen für bestimmte Inhaltstypen und ressourcenspezifische Optimierungen, die Eingaben des Entwicklers erfordern.

Für die bestmögliche Leistung ist eine Kombination all dieser Verfahren erforderlich.

### TL;DR {: .hide-from-toc }

* Komprimierung bezeichnet die Codierung von Informationen unter Verwendung von weniger Bits.
* Mit der Entfernung unnötiger Daten werden stets die besten Resultate erzielt.
* Es gibt viele verschiedene Komprimierungsmethoden und -algorithmen.
* Sie benötigen eine Auswahl an Techniken, um die beste Komprimierung zu erreichen.

Die Reduzierung der Datenmenge wird als `Datenkomprimierung` bezeichnet und stellt einen eigenen Forschungsbereich dar: Viele Menschen verbrachten ihre komplette Berufslaufbahn mit der Arbeit an Algorithmen, Verfahren und Optimierungen zur Verbesserung des Komprimierungsverhältnisses, der Geschwindigkeit und der Speicheranforderungen diverser Komprimierungsprogramme. Im Rahmen dieser Ausführungen kann dieses Thema selbstverständlich nicht erschöpfend behandelt werden, aber ein grundsätzliches Verständnis der Funktionsweise von Komprimierungen und der verfügbaren Methoden zur Reduzierung der Größe der verschiedenen Ressourcen, die von unseren Seiten benötigt werden, ist dennoch wichtig.

Zur Veranschaulichung der Grundprinzipien dieser Methoden im Einsatz wollen wir überlegen, wie wir zur Optimierung eines einfachen Textnachrichtenformats vorgehen können, das wir nur für dieses Beispiel erfinden:

    # Nachstehend sehen Sie eine geheime Nachricht, die aus einer Reihe von Headern im
    # Schlüssel-Wert-Format gefolgt von einer neuen Zeile und der verschlüsselten Nachricht besteht.
    format: secret-cipher
    date: 04/04/14
    AAAZZBBBBEEEMMM EEETTTAAA
    

1. Die Nachrichten können beliebige Anmerkungen beinhalten, die durch das Präfix `#` gekennzeichnet sind. Die Anmerkungen wirken sich nicht auf die Bedeutung oder eine andere Verhaltensweise der Nachricht aus.
2. Die Nachrichten können `Header` enthalten, bei denen es sich um Schlüssel-Wert-Paare getrennt durch `:` handelt und die am Anfang der Nachricht erscheinen müssen.
3. Nachrichten beinhalten Nutzdaten in Form von Text.

Wie könnten wir die Größe der obigen Nachricht reduzieren, die derzeit 200 Zeichen lang ist?

1. Nun, die Anmerkung ist interessant, aber sie ändert die Bedeutung der Nachricht nicht, deshalb entfernen wir sie, wenn wir die Nachricht übertragen.
2. Es gibt wahrscheinlich einige raffinierte Methoden, mit denen wir die Header effizient codieren könnten. Falls zum Beispiel alle Nachrichten stets ein `Format` und ein `Datum` aufwiesen, könnten wir diese Informationen in kurze ganzzahlige IDs konvertieren und dann nur diese senden! Allerdings sind wir uns hier nicht sicher, deshalb lassen wir das vorerst bleiben.
3. The payload is text only, and while we don’t know what the contents of it really are (apparently, it’s using a "secret-message"), just looking at the text shows that there's a lot of redundancy in it. Perhaps instead of sending repeated letters, you can just count the number of repeated letters and encode them more efficiently. For example, "AAA" becomes "3A", which represents a sequence of three A’s.

Über die Kombination unserer Methoden erzielen wir das folgende Ergebnis:

    format: secret-cipher
    date: 04/04/14
    3A2Z4B3E3M 3E3T3A
    

Die neue Nachricht ist 56 Zeichen lang, d. h., wir schafften es, die ursprüngliche Nachricht um beeindruckende 72 % zu reduzieren - nicht schlecht und wir haben gerade erst begonnen!

Sie werden sich fragen: Das ist ja alles prima, aber wie hilft es uns dabei, unsere Webseiten zu optimieren? Wir werden doch wohl nicht versuchen, eigene Komprimierungsalgorithmen zu erfinden, oder? Die Antwort ist nein, aber wie Sie feststellen werden, nutzen wir genau dieselben Verfahren und Strategien bei der Optimierung der diversen Ressourcen auf unseren Seiten: Vorverarbeitung, kontextspezifische Optimierungen und unterschiedliche Algorithmen für unterschiedliche Inhalte.

## Minimierung: Vorverarbeitung und kontextspezifische Optimierungen

### TL;DR {: .hide-from-toc }

* Mit inhaltsspezifischen Optimierungen kann die Größe der übertragenen Ressourcen erheblich reduziert werden.
* Inhaltsspezifische Optimierungen werden am besten im Rahmen des Build-/Release-Zyklus durchgeführt.

Die beste Vorgehensweise bei der Komprimierung redundanter und unnötiger Daten besteht darin, diese ganz zu entfernen. Natürlich können wir nicht einfach willkürlich Daten löschen, aber in manchen Szenarien, wo wir über inhaltsspezifische Kenntnisse des Datenformats und dessen Eigenschaften verfügen, ist es häufig möglich, die Menge der Nutzdaten ohne Auswirkung auf den Sinn erheblich zu reduzieren.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/optimizing-content-efficiency/_code/minify.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Betrachten Sie die einfache HTML-Seite oben und ihre drei unterschiedlichen Inhaltstypen: HTML-Markup, CSS-Styles und JavaScript. Für jeden dieser Inhaltstypen gibt es unterschiedliche Regeln darüber, welches Markup, welche Styles bzw. welche Inhalte zulässig sind, wie Kommentare zu kennzeichnen sind und so weiter. Wie können wir die Größe dieser Seite verringern?

Mit der Anwendung der obigen Schritte können wir unsere Seite von 406 auf 150 Zeichen reduzieren - das ist eine Einsparung von 63 %! Zugegeben, man kann den Code nicht gut lesen, aber das ist auch nicht nötig: Wir können die ursprüngliche Seite als `Entwicklungsversion` behalten und immer dann die obigen Schritte anwenden, wenn wir die Seite auf unserer Website freigeben wollen.

* Codekommentare sind zwar für den Entwickler sehr hilfreich, werden vom Browser aber nicht benötigt! Durch einfache Entfernung der CSS- (`/* ... */`), HTML- (`<!-- ... -->`) und JavaScript- (`// ...`) Kommentare lässt sich die Gesamtgröße der Seite erheblich reduzieren.
* Ein `intelligentes` CSS-Komprimierungsprogramm könnte feststellen, dass wir die Regeln für `.awesome-container` ineffizient verwenden und es könnte die beiden Deklarationen ohne Auswirkung auf andere Styles zu einer zusammenfassen. Auf diese Weise ließen sich noch mehr Bytes einsparen.
* Leerraum (Leerzeichen und Tabulatoren) ist nur bei der Entwicklung von HTML-, CSS- und JavaScript-Code hilfreich. Mit einem zusätzlichen Komprimierungsprogramm können alle diese Tabulatoren und Leerzeichen entfernt werden.

<pre class="prettyprint">
{% includecode content_path="web/fundamentals/performance/optimizing-content-efficiency/_code/minified.html" region_tag="full" adjust_indentation="auto" %}
</pre>

Wenn wir einen Schritt zurückgehen, zeigt das obige Beispiel einen wichtigen Punkt auf: Ein Allzweck-Komprimierungsprogramm, z. B. eines für die Komprimierung von beliebigem Text, könnte die obige Seite wahrscheinlich recht gut komprimieren, aber es wäre damit niemals möglich, Kommentare zu entfernen, die CSS-Regeln zusammenzufassen oder Dutzende andere inhaltsspezifischer Optimierungen durchzuführen. Aus diesem Grund ist die Kombination aus Vorverabeitung, Minimierung und kontextspezifischer Optimierung so leistungsstark.

Note: Fallbeispiel: Die unkomprimierte Entwicklungsversion der JQuery-Bibliothek nähert sich mittlerweile der Größe von 300 KB an. Dieselbe Bibliothek in minimierter Form, d. h. ohne Kommentare usw., ist ungefähr 3 mal kleiner: ca. 100 KB.

Auf ähnliche Weise können die obigen Verfahren über rein textbasierte Ressourcen hinaus angewendet werden. Bilder, Videos und andere Inhaltstypen beinhalten alle eigene Arten von Metadaten und verschiedene Nutzdaten. Wenn Sie zum Beispiel ein Bild mit einer Kamera aufnehmen, enthält das Foto üblicherweise eine Menge zusätzlicher Informationen: Kameraeinstellungen, Ort und so weiter. Je nach Anwendung können diese Daten kritisch, z. B. bei einer Foto-Sharing-Website, oder komplett nutzlos sein und Sie sollten die Entfernung in Betracht ziehen. In der Praxis können diese Metadaten bei jedem Bild eine zweistellige Zahl an Kilobytes ausmachen!

Kurze Zusammenfassung: Machen Sie als ersten Schritt bei der Optimierung der Effizienz Ihrer Ressourcen eine Bestandsaufnahme der verschiedenen Inhaltstypen und überlegen Sie, welche inhaltsspezifischen Optimierungen Sie anwenden können, um deren Größe zu reduzieren - auf diese Weise können Sie erhebliche Einsparungen erzielen! Sobald Sie die entsprechenden Optimierungen identifiziert haben, automatisieren Sie diese, indem Sie sie Ihren Erstellungs- und Freigabeprozessen hinzufügen. Das ist die einzige Möglichkeit, um sicherzustellen, dass die Optimierungen beibehalten werden.

[GZIP](http://de.wikipedia.org/wiki/Gzip) ist ein allgemeines Komprimierungsprogramm, das auf beliebige Bytestreams angewendet werden kann. Es erkennt zuvor festgestellte Inhalte wieder und versucht, doppelte Datenfragmente auf effiziente Weise zu ersetzen. Wenn Sie nähere Informationen wünschen, sehen Sie sich diese [großartige, leicht verständliche Erläuterung zu GZIP an](https://www.youtube.com/watch?v=whGwm0Lky2s&feature=youtu.be&t=14m11s). Allerdings erzielt GZIP in der Praxis die beste Leistung bei textbasierten Inhalten, wo es bei größeren Dateien häufig Komprimierungsraten von bis zu 70 bis 90 % erreicht, wohingegen die Ausführung von GZIP auf Ressourcen, die bereits mit alternativen Algorithmen komprimiert wurden, wie zum Beispiel bei den meisten Bildformaten, wenig bis gar keine Verbesserungen mit sich bringt.

Alle modernen Browser unterstützen und handeln die GZIP-Komprimierung bei sämtlichen HTTP-Anfragen aus. Unsere Aufgabe besteht darin, den Server ordnungsgemäß zu konfigurieren, damit er die komprimierte Ressource bereitstellt, wenn diese vom Client angefordert wird.

## Textkomprimierung mit GZIP

### TL;DR {: .hide-from-toc }

* GZIP ist am leistungsstärksten bei textbasierten Ressourcen: CSS, JavaScript, HTML.
* Alle modernen Browser unterstützen die GZIP-Komprimierung und fordern sie automatisch an.
* Ihr Server muss für die Aktivierung der GZIP-Komprimierung konfiguriert sein.
* Bei manchen CDNs sind besondere Maßnahmen für die Aktivierung von GZIP erforderlich.

In der obigen Tabelle sind die Einsparungen durch die GZIP-Komprimierung für einige der beliebtesten Java-Script-Bibliotheken und CSS-Frameworks aufgeführt. Die Einsparungen reichen von 60 bis 88 %. Beachten Sie, dass die Kombination aus minimierten Dateien, die im Dateinamen durch `.min` gekennzeichnet sind, mit GZIP zu noch größeren Reduktionen führt.

Das Beste dabei ist, dass die Aktivierung von GZIP eine der einfachsten und wirkungsvollsten Optimierungsformen darstellt - leider wird dies vielfach dennoch vergessen. Die meisten Webserver komprimieren die Inhalte für Sie. Es muss nur noch überprüft werden, ob der Server für die Komprimierung aller Inhaltstypen korrekt konfiguriert ist, die von der GZIP-Komprimierung profitieren.

<table>
  
<thead>
  <tr>
    <th>Bibliothek</th>
    <th>Größe</th>
    <th>Komprimierte Größe</th>
    <th>Komprimierungsverhältnis</th>
  </tr>
</thead>

<tr>
  <td data-th="library">jquery-1.11.0.js</td>
  <td data-th="size">276 KB</td>
  <td data-th="compressed">82 KB</td>
  <td data-th="savings">70 %</td>
</tr>
<tr>
  <td data-th="library">jquery-1.11.0.min.js</td>
  <td data-th="size">94 KB</td>
  <td data-th="compressed">33 KB</td>
  <td data-th="savings">65 %</td>
</tr>
<tr>
  <td data-th="library">angular-1.2.15.js</td>
  <td data-th="size">729 KB</td>
  <td data-th="compressed">182 KB</td>
  <td data-th="savings">75 %</td>
</tr>
<tr>
  <td data-th="library">angular-1.2.15.min.js</td>
  <td data-th="size">101 KB</td>
  <td data-th="compressed">37 KB</td>
  <td data-th="savings">63 %</td>
</tr>
<tr>
  <td data-th="library">bootstrap-3.1.1.css</td>
  <td data-th="size">118 KB</td>
  <td data-th="compressed">18 KB</td>
  <td data-th="savings">85 %</td>
</tr>
<tr>
  <td data-th="library">bootstrap-3.1.1.min.css</td>
  <td data-th="size">98 KB</td>
  <td data-th="compressed">17 KB</td>
  <td data-th="savings">83 %</td>
</tr>
<tr>
  <td data-th="library">foundation-5.css</td>
  <td data-th="size">186 KB</td>
  <td data-th="compressed">22 KB</td>
  <td data-th="savings">88 %</td>
</tr>
<tr>
  <td data-th="library">foundation-5.min.css</td>
  <td data-th="size">146 KB</td>
  <td data-th="compressed">18 KB</td>
  <td data-th="savings">88 %</td>
</tr>
</table>

Welches ist die beste Konfiguration für Ihren Server? Das Projekt `HTML5 Boilerplate` enthält [Beispiel-Konfigurationsdateien](https://github.com/h5bp/server-configs) für alle populären Server mit detaillierten Kommentaren zu jedem Konfigurationsmerker und jeder Einstellung. Suchen Sie Ihren bevorzugten Server in der Liste, achten Sie auf den GZIP-Bereich und überprüfen Sie, ob Ihr Server mit den empfohlenen Einstellungen konfiguriert ist.

1. **Wenden Sie zuerst die inhaltsspezifischen Optimierungen an: CSS-, JS- und HTML-Minimierungen.**
2. **Komprimieren Sie die minimierten Resultate mit GZIP.**

Enabling GZIP is one of the simplest and highest-payoff optimizations to implement, and yet, many people don't implement it. Most web servers compress content on your behalf, and you just need to verify that the server is correctly configured to compress all the content types that benefit from GZIP compression.

Eine schnelle und einfache Möglichkeit, GZIP in Aktion zu erleben, besteht darin, die Chrome DevTools zu öffnen und sich die Spalte `Size/Content` (Größe/Inhalt) im Netzwerk-Steuerfeld anzusehen: `Size` gibt die Übertragungsgröße der Ressource an und `Content` die unkomprimierte Größe der Ressource. Bei der HTML-Ressource im obigen Beispiel wurden mit GZIP während der Übertragung 24,8 KB eingespart!

* Find your favorite server in the list.
* Look for the GZIP section.
* Confirm that your server is configured with the recommended settings.

<img src="images/transfer-vs-actual-size.png"
  alt="DevTools demo of actual vs transfer size" />

Schließlich noch ein Hinweis: Während die meisten Server die Ressourcen im Zuge der Bereitstellung für den Nutzer automatisch komprimieren, erfordern einige CDNs einen zusätzlichen Konfigurationsaufwand, um sicherzustellen, dass GZIP wieeingestellt aktiv wird. Überprüfen Sie Ihre Website, um sicherzustellen, dass Ihre Ressourcen tatsächlich [komprimiert werden](http://www.whatsmyip.org/http-compression-test/)!

Note: Sometimes, GZIP increases the size of the asset. Typically, this happens when the asset is very small and the overhead of the GZIP dictionary is higher than the compression savings, or when the resource is already well compressed. To avoid this problem, some servers allow you to specify a minimum filesize threshold.

Finally, while most servers automatically compress the assets for you when serving them to the user, some CDNs require extra care and manual effort to ensure that the GZIP asset is served. Audit your site and ensure that your assets are, in fact, [being compressed](http://www.whatsmyip.org/http-compression-test/).

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}