# Gundlagen

Diese Grundlagen befassen sich mit den Begrifflichkeiten, stellen die konkrete Motivation und Anwendungsgebiete vor. Auf Basis dieser Grundlagen können im folgenden Kapitel konkrete Techniken und Vorgehensweisen erläutert werden.

## Event

Ein grundlegender Begriff stellt das **Event** oder auch Ereignis an sich dar. David Luckham definiert ein Event als eine Aufzeichnung einer Änderung in einem System. Die Events sind über drei Faktoren miteinander verbunden: Zeit, Kausalität und Aggregation. Darüberhinaus können die Events in einer Hierarchie zueinander stehen, dass bedeutet das ein Event aus mehreren anderen Events hervorgehen kann. Als Beispiel für ein sehr niedriges Event kann ein Netzwerkevent in Form eines Ping eines Routers dienen. Mehrere langsame Pings können auf das höhere Event eines Ausfalls aggregiert werden. [1]

Events können auch als Veränderung eines Zustands eines reallen oder virutellen Objekts beschrieben werden. Dabei kann zwischen technischen Ereignissen (bspw. Veränderung einer gemessenen Temperatur) und Geschäftsereignissen (Kündigung eines Liefervertrags) unterschieden werden. [2]

Wichtig für die Abgrenzung eines Events besteht in dem Verständnis, dass ein Event einen konkretem Zeitpunkt zugeordnet wird. Damit besitzt ein Event keine Dauer und grenzt sich damit von einer Aktivität ab. Eine Aktivität besteht aus einem Start- und einem Endeereignis. Eine Aktivität ändert den Zustand des Systems nicht. Als Beispiel für eine Aktivität kann der Transport einer Palette genommen werden. Das Anfangsereignis ist die Abfahrt des Gabelstaplers und das Endereignis die Ankunft im Lager. [3]

## Event-Stream

Ein Ereignisstrom oder **Event-Stream** besteht aus einer linear geordneten Sequenz kontinuierlich eintreffenden Ereignissen. Der typischer zu verarbeitender Datenstrom weist die folgenden Kennzeichen auf: 


- Er enhält aktuelle live Daten. Die enthaltenen Daten sind von geringer Komplexität und lassen sich mit wenigen Daten beschreiben. 
- Der Datenstrom ist hochfrequent, es handelt sich um Ereignisse mit einer hohen Eingangsrate. 
- Der Datenstrom ist unbegrenzt, neue Ereignisse geschehen fortlaufend. 
- Es können nicht alle Daten gespeichert werden, der Datenstrom ist volatil. 
- Die enthaltenen Datensätze stehen in einer impliziten Beziehung. [2]

## Event-Stream-Processing

In den Quellen finden sich zwei teilweise synonym verwendete Begriffe für die Technologie der Verarbeitung von Ereignisdatenströmen. Sowohl der Begriff **Event-Stream-Processing** als auch der Begriff **Complex-Event-Processing (CEP)** beschreiben Softwaretechnologie zur dynamischen Analyse von massiven Ereigniströmen in Echtzeit. Ziel ist dabei ein Informationsgewinn aus dem Erkennen der Beziehung zwischen den Ereignissen auf Basis von Mustern. Die Abgrenzung zwischen den beiden Begriffen erfolgt häufig über die Komplexität der Verarbeitung der Ereignisse. Als Event-Stream-Processing kann eine einfache Verarbeitung bspw. in Form einer Aggregation gesehen werden. Das Complex-Event-Processing versucht auf Basis von Regeln Muster zu erkennen und daraus höherwertige abstraktere Ereignisse zu generieren. In dieser Arbeit werden die Begriffe synonym verwendet. [2] [4] [5]

## Konkrete Anwendungsfälle

Mit der geschaffenen grundlegenden Übersicht sollen an diese Stelle möglichst konkrete Einsatzgebiete für das Event-Stream-Processing als Motivation präsentiert werden.

- Leistungsvoraussage für Windenergieanlagen. Die Daten mehrere Windenergieanlagen werden ausgewertet um eine Voraussage über die Stromproduktion zu gewinnen. [6]
- Business Acitivity Monitoring. Kontrolle der Einhaltung von Service-Level-Agreements im IT-Umfeld.
- Echtzeitüberwachung der Produktion im Industrie 4.0 Umfeld. Unter anderem zur Ablaufoptimierung, Erkennung von Wartungsnotwendigkeit und Qualitätsanalyse.
- Überwachung von Supply-Chains mit der Erkennung von Verzögerungen.
- Kundenbeziehung. Erfassung von Kunden-Feedback über Verhalten in sozialen Netzwerken.
- Finanzmärkte. Analyse von Aktienkursen und Einflussgrößen. Erkennung von Betrug im Zahlungsverkehr. [7]


***

```

Quellen:

[1] Luckham, David (2002): The Power of Events: An Introduction to Complex Event Processing in Distributed Enterprise Systems. Pearson Education, Inc, Seite 88ff.

[2] Bruns, Ralf; Dunkel, Jürgen (o.J.): Complex Event Processing im Überblick, Seite 9f.

[3] Hedtstück, Ulrich (2017): Complex Event Processing. Berlin, Heidelberg: Springer Berlin Heidelberg, Seite 12f.

[4] Bade, Dirk (2009): Event Stream Processing & Complex Event Processing, online unter http://docplayer.org/3361260-Event-stream-processing-complex-event-processing-dirk-bade.html

[5] David B. Robins (2010): Complex Event Processing, University of Washington, Seite 1.

[6] von Gallera, Diana; Trujillo, Juan José; Nicklas, Daniela (o.J.): Leistungskennlinienberechnung von
Windenergieanlagen unter Einsatz eines
Datenstrommanagementsystems, online unter http://odysseus.informatik.uni-oldenburg.de/fileadmin/files/WEAUsingOdysseus.pdf.  

[7] Projekt Odysseus, Uni Oldenburg: online unter http://odysseus.informatik.uni-oldenburg.de.


```

***



