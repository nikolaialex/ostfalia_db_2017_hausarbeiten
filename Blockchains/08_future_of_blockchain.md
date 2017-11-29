[<< zurück](02_toc.md)

# Zukunft der Blockchain

Es existieren zahlreiche Ideen und Konzepte, wie Blockchains in Zukunft eingesetzt werden können. Dieser Abschnitt soll eine kleine Übersicht über mögliche Einsatzszenarios der Blockchain in der Zukunft geben.

## Smart Grid

Unter einem Smart Grid wird ein intelligentes Stromnetz verstanden. Intelligenz ist vor allem deshalb nötig, weil durch den steigenden Anteil erneuerbarer Energiequellen die Energieerzeugung zunehmend von unkontrollierbaren Faktoren wie Sonnenschein und Wind abhängig ist. Dies macht eine intelligente Steuerung des Stromnetzes erforderlich.

Das in [01] vorgestellte Konzept ist auf kleine, lokale Energiemärkte mit 100 Haushalten ausgelegt. Hintergrund dieses Konzepts ist die mangelnde Fähigkeit bestehender Energiemärkte, in Echtzeit auf die schwankende Produktion erneuerbarer Energiequellen zu reagieren. Das Ziel besteht darin, zwischen den Haushalten einer kleinen Gemeinschaft die Energieerzeugung und den Energiebedarf mittels einer Blockchain zu steuern. Das Konzept sieht vor, innerhalb dieser kleinen, lokalen Gemeinschaft einen Balance zwischen Energieerzeugung und -verbrauch zu erreichen und gleichzeitig ein dynamisches Preissystem zu etablieren.

In dem Konzept wird davon ausgegangen, dass die teilnehmenden Haushalte sich aus Verbrauchern und sogenannten "Prosumern" (zusammengesetzt aus "Producer" und "Consumer"), also Haushalten, die zwar Energie verbrauchen, aber gleichzeitig auch Energie über eine eigene Photovoltaikanlage produzieren, zusammensetzen. Zu jedem Zeitpunkt wird der lokal erzeugte Strom zu einem bestimmten Preis angeboten, der von den anderen Teilnehmern dann gekauft werden kann. Wird mehr Strom benötigt, als die lokale Gemeinschaft bereitstellen kann, wird zusätzlicher Strom aus dem öffentlichen Stromnetz bezogen. Umgekehrt wird auch überschüssige Energie ins öffentliche Netz eingespeist. Der Preis, der für den Strombezug aus dem öffentlichen Netz zu zahlen ist, bildet zugleich die obere Grenze für den Preis des lokal erzeugten Stroms. Ebenso bildet die Einspeisevergütung die untere Grenze.

Für das lokale Smart Grid müssen alle Teilnehmer über intelligente Stromzähler (Smart Meter) verfügen, durch die Energieerzeugung und -bedarf in Echtzeit erfasst werden können. Über die Blockchain wird nun automatisch von jedem Teilnehmer die benötigte Energie zugekauft bzw. überschüssige Energie verkauft. Die Blockchain regelt also einerseits das Gleichgewicht zwischen benötigtem Strom und verfügbarem Strom und enthält andererseits die Preis- und Verbrauchsdaten für die Stromrechnung jedes Teilnehmers.

## Schutz persönlicher Daten

Ein weiteres Konzept befasst sich mit dem Schutz persönlicher Daten. Auch hier kann die Blockchain helfen, allerdings werden in ihr nicht die persönlichen Daten selbst gespeichert, sondern lediglich die Zugriffsrechte auf diese. Die persönlichen Daten werden in einer Key-Value-Datenbank abgelegt. Der Benutzer kann nun über die Blockchain Zugriffsberechtigungen an einzelne Dienste vergeben. Der Zugriff auf die Daten durch einen authorisierten Dienst wird ebenfalls in der Blockchain erfasst. Auf diese Weise ist für den Benutzer jederzeit kontrollierbar, wer Zugriff auf seine Daten hat. Eine einmal erteilte Zugriffsberechtigung kann natürlich auch jederzeit durch einen neuen Eintrag in der Blockchain wieder zurückgenommen werden. [02, Kap. 3.1]

## Digitale Rechteverwaltung

Ein ähnlicher Ansatz kann für die digitale Rechteverwaltung genutzt werden. Die Blockchain dient dabei als Speicher für kryptographische Schlüssel zum Abspielen geschützter Audio- und Videodaten. Zum Abspielen der Medien muss der Benutzer in diesem Konzept allerdings online sein, um die vom Anbieter erteilten Berechtigungen und den benötigten Schlüssel in der Blockchain ermitteln zu können. [02, Kap. 3.2]

## Domain Name Service (DNS)

Da Daten in einer Blockchain weitgehend sicher vor Manipulationen sind, sind Blockchains insbesondere dort von Nutzen, wo viele Teilnehmer die gleichen Informationen benötigen. Dies ist beispielsweise beim Domain Name Service (DNS) der Fall, welcher einem Domainnamen eine IP-Adresse zuordnet. Das vorliegende Konzept soll kein Ersatz für den Domain Name Service sein, sondern diesen ergänzen, um Manipulationen zu erschweren. Die Blockchain enthält dabei den öffentlichen Schlüssel des Besitzers, wodurch dieser eindeutig identifizierbar ist. Zusätzlich kommt eine Hashtabelle zum Einsatz, die die eigentlichen DNS-Records speichert, die vom Domaininhaber digital signiert wurden. Dadurch wird gewährleistet, dass die DNS-Informationen tatsächlich vom Besitzer der Domain stammen und nicht manipuliert wurden. Wechselt eine Domain den Besitzer, wird der öffentliche Schlüssel des neuen Besitzers in der Blockchain hinterlegt. Der alte Besitzer muss die Transaktion mit seinem privaten Schlüssel genehmigen. Fortan ist dann der neue Besitzer für die Pflege des DNS-Records zuständig. [02, Kap. 3.3]

## Überwachung von Lebensmitteln

Für dieses Konzept ist es erforderlich, dass die Lebensmittel mit einem RFID-Chip ausgestattet sind, durch den sie eindeutig identifiziert und automatisch erfasst werden können. Das Konzept sieht nun vor, jeden Produktionsschritt mit allen relevanten Informationen in einer Blockchain zu speichern. So könnte beispielsweise erfasst werden, wo und wann der jeweilige Produktionsschritt stattfand und welche Mitarbeiter beteiligt waren. Da RFIDs bereits heute in der Lebensmittelproduktion verbreitet sind, ist die Umsetzung dieses Konzepts relativ einfach. Da die Daten in der Blockchain nicht nachträglich verändert werden können, lassen sich Lebensmittelskandale nicht mehr so leicht vertuschen und es lässt sich leicht feststellen, welche Produkte von einer möglichen Verunreinigung betroffen sind. [02, Kap. 3.5]


```
Quellenangabe:

[01] - Esther Mengelkamp, Benedikt Notheisen, Carolin Beer, David Dauer, Christof Weinhardt: A blockchain-based smart grid: towards sustainable local energy markets. Erschienen in: Computer Science - Research and Development, 2017, S. 1–8.
[02] - Stephan Wiefling, Luigi Lo Iacono, Frederik Sandbrink: Anwendung der Blockchain außerhalb von Geldwährungen. Erschienen in: Datenschutz und Datensicherheit - DuD, August 2017, Volume 41, Issue 8, S. 482–486.

```

***

[<< Sicherheit](07_security.md) | [Äußere Faktoren >>](09_external_factors.md)