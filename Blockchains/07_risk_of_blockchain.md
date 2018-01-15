***

[<< zurück](02_toc.md)

***

# Gefahren der Blockchain

Wie jede Technik hat auch die Blockchain ihre Nachteile und Schattenseiten. Auf einige davon wird in diesem Kapitel näher eingegangen.

## Effizienz
Ein Problem der Blockchain-Technologie ist ihre geringe Effizienz, insbesondere bei großen Datenmengen. Als anschauliches Beispiel mögen hier die Kryptowährungen dienen. Da jeder Teilnehmer über denselben Datenbestand verfügt, muss ein neuer Teilnehmer zunächst die bereits vorhandenen Daten herunterladen und verarbeiten, bevor er überhaupt mit der Blockchain arbeiten kann. Die Bitcoin-Blockchain weist bereits eine Größe von mehr als 100 GB auf. Die Ethereum-Blockchain bringt es sogar bereits auf mehr als 200 GB. Der Nutzung dieser Kryptowährungen sind also durch die vorhandene Internetanbindung und den freien Festplattenspeicher für manch einen bereits Grenzen gesetzt. Für die reine Nutzung einer Wallet ist das Herunterladen der gesamten Blockchain zwar nicht erforderlich, allerdings muss man in diesem Fall Vertrauen in den genutzten Wallet-Server besitzen. [01]

Ein weiteres Problem der Blockchain ist ihre mangelnde Skalierbarkeit. Das Bitcoin-Netzwerk kann maximal 7 Transaktionen pro Sekunde durchführen [01]. In der Praxis sind 3-4 Transaktionen pro Sekunde die Regel und damit ist das Bitcoin-Netzwerk bereits heute ausgelastet [02]. Zum Vergleich: Paypal hat im Jahr 2016 durchschnittlich 193 Transaktionen pro Sekunde durchgeführt mit Spitzenwerten von 450 Transaktionen pro Sekunde an Tagen wie dem "Cyber Monday" [03]. Die Kreditkartengesellschaft VISA hat im Jahr 2016 durchschnittlich 1667 Transaktionen pro Sekunde verarbeitet und geht davon aus, dass ihr Netzwerk bis zu 56.000 Transaktionen pro Sekunde verarbeiten kann [03].

Die geringe Effizienz des Bitcoin-Netzwerks ergibt sich aus der Blockgröße. Diese ist auf 1 MB festgelegt. Eine Transaktion benötigt im Durchschnitt 226 Byte, womit nur wenige Tausend Transaktionen in einem Block erfasst werden können. Da für jeden neuen Block ein Mining erforderlich ist, ist die Wachstumsgeschwindigkeit der Blockchain begrenzt. Ein Vergrößern der Blockgröße würde zwar mehr Transaktionen pro Block erlauben, jedoch das Mining erschweren und die dezentrale Struktur der Blockchain gefährden, wenn das Mining nur noch mit großen Mining-Farmen in vertretbarer Zeit durchführbar wäre. Auch wenn es durchaus weitere Überlegungen gibt, um die Effizienz der Bitcoin-Blockchain zu steigern, sind Transaktionsraten wie bei Paypal oder gar VISA damit nach derzeitigem Stand nicht einmal ansatzweise realisierbar. [02]

Gerade die Effiziens ist einer der größten Problem des Bitcion, umso verwunderlicher ist die Absage des Segwit Updates[06] für Bitcoin, wo es die Blockgröße auf 2 MB angehoben werden sollte. Die Entwickler versprechen eine Lösung zu finden, doch wie die Antwort auf dieses Problem aussehen wird, ist bisher nicht klar. Ein Blockbeitrag vom Dezember 2017 verweist allerdings wieder darauf, dass das umstrittende Segwit Update doch 2018 kommen wird.[07]

Deutlich effizienter ist beispielsweise Ethereum, bei dem man sich zum Ziel gesetzt hat, den Proof-of-work-Mechanismus langfristig durch den Proof-of-Stake-Mechanismus zu ersetzen (vgl. Kapitel [Sicherheit](08_security.md)). Durch einige Designunterschiede zu Bitcoin (vgl. Kapitel [Kryptobörsen](05_cryptocurrencies.md#ethereum)) sind bei Ethereum bereits heute etwa 20 Transaktionen pro Sekunde erreichbar [03]. Mit der Umstellung auf den Proof-of-stake-Mechanismus dürfte dieser Wert noch deutlich steigen, so dass Ethereum insgesamt als erheblich effizienter angesehen werden kann als Bitcoin.

Im Zusammenhang mit der Effizienz sei auch auf den Stromverbrauch im Rahmen des Mining hingewiesen. Der Stromverbrauch des Bitcoin-Netzwerks wird auf 32,4 Terawattstunden pro Jahr geschätzt, was in etwa dem Strombedarf Dänemarks entspricht. Prognosen zufolge könnte der Strombedarf des Bitcoin-Netzwerks bei Anhalten des derzeitigen Booms bereits im Jahr 2019 auf dem Niveau der USA liegen und 2020 dem Strombedarf der gesamten Welt entsprechen. Dass dies nicht nur ein Problem für die Umwelt, sondern auch für die Energieerzeugung und -verteilung darstellt, liegt auf der Hand. Die kurz- und mittelfristige Entwicklung des Bitcoins dürfte daher auch aus umwelt- und energiepolitischer Sicht von großer Bedeutung sein. [04]

## Datenschutz
Eine Blockchain erlaubt ein Höchstmaß an Transparenz. Alle jemals getätigten Transaktionen sind in der Blockchain gespeichert und können durch jeden Interessierten eingesehen werden. Zwar ist eine Zuordnung der Transaktionen zu konkreten Personen nicht ohne weiteres möglich, da lediglich die öffentlichen Schlüssel von Sender und Empfänger einer Zahlung aufgeführt werden. Wird jedoch zu irgendeinem Zeitpunkt bekannt, welche Person sich hinter einem solchen Schlüssel verbirgt, können rückwirkend sämtliche Zahlungen, die diese Person jemals im Bitcoin-Netzwerk getätigt oder empfangen hat, nachvollzogen werden. Hiervor kann man sich jedoch schützen, indem man mehrere Wallets parallel verwendet oder diese in regelmäßigen Abständen wechselt. Dies geht jedoch zu Lasten der Bequemlichkeit. [05]

## Verlust von Wallets
Für die Nutzung einer Wallet ist die Kenntnis des privaten Schlüssels und des zugehörigen Passwortes, mit dem der Schlüssel geschützt wurde, notwendig. Dies birgt die Gefahr des Verlusts. Den privaten Schlüssel bewahrt man in der Regel in Form einer Datei auf. Kommt es zum Datenverlust und besitzt man kein Backup des Schlüssels, ist ein Zugriff auf das Wallet nicht mehr möglich. Gleiches gilt für den Verlust des Passwortes: Ist dieses verloren gegangen, kann der private Schlüssel und damit das Wallet nicht mehr benutzt werden. Sofern sich kein Guthaben im Wallet befindet, ist dies nicht weiter problematisch. Es ist problemlos möglich, sich ein neues Wallet einzurichten und dieses für alle zukünftigen Transaktionen zu verwenden. Befand sich jedoch noch Guthaben im Wallet, ist dieses mit dem Verlust des privaten Schlüssels oder des Passwortes ebenfalls verloren. 

***

[<< Anwendungsgebiete](06_use_cases.md) | [Sicherheit >>](08_security.md)

***

```

Quellenangabe:

[01] - Alexey Malanov: Sechs Mythen über Blockchain und Bitcoin: Wir stellen die Effektivität der Methode auf die Probe. Online-Artikel, 22.08.2017, URL: https://www.kaspersky.de/blog/bitcoin-blockchain-issues/14453/ abgerufen am 02.01.2018
[02] Christian Beckel: Skalieren Blockchains? Sorgen und Lösungsansätze. Online-Artikel, 2017, URL: http://site.blocklab.de/2017/Skalierung/ abgerufen am 02.01.2018
[03] Jan Vermeulen: Bitcoin and Ethereum vs Visa and PayPal – Transactions per second. Online-Artikel, 22.04.2017, URL: https://mybroadband.co.za/news/banking/206742-bitcoin-and-ethereum-vs-visa-and-paypal-transactions-per-second.html angerufen am 02.01.2018
[04] Arvid Kaiser: Warum der Bitcoin-Boom die globale Energiewende bedroht. Online-Artikel, 07.12.2017, URL: http://www.spiegel.de/wirtschaft/unternehmen/bitcoin-stromverbrauch-bedroht-globale-energiewende-a-1182234.html abgerufen am 02.01.2018
[05] Tobias Herrmann: Bitcoin: Gefahr eines Datenschutz-GAU durch die Blockchain? Online-Artikel, 02.08.2017, URL: https://www.datenschutzbeauftragter-info.de/bitcoin-gefahr-eines-datenschutz-gau-durch-die-blockchain/ abgerufen am 02.01.2018
[06] Axel Kannenberg: Segwit 2x: Blockgrößen-Update für Bitcoin abgesagt. Online-Artikel, 10.11.2017, URL: https://www.heise.de/newsticker/meldung/Segwit-2x-Blockgroessen-Update-fuer-Bitcoin-abgesagt-3884820.html abgerufen am 15.01.2018
[07] Dan Romero: Bitcoin SegWit update. Online-Artikel, 15.12.2017, URL: https://blog.coinbase.com/bitcoin-segwit-update-3ab0484e4526 abgerufen am 15.01.2018

```

***