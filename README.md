# Kalender-Termine-Erinnerung
Mit dieser Zusammenstellung von Home Assistant Trigger-Templates und Notify-Automatisierungen besteht die Möglichkeit, Termine aus dem Kalender übersichtlich für heute oder morgen zu erfassen. Zusätzlich kann der nächste anstehende Termin aus dem Kalender auf dem Dashboard angezeigt oder durch eine Notify-Benachrichtigung hervorgehoben werden.


## ÜBER TRIGGER SENDOREN/TEMPLATES


Ein Home Assistant Trigger-Sensor ist ein Komponententyp in der Home Assistant-Plattform, der dazu dient, auf bestimmte Ereignisse oder Zustände zu reagieren und automatisierte Aktionen auszulösen. Dieser Sensor überwacht kontinuierlich den Status von bestimmten Entitäten oder Plattformen und wird aktiviert, wenn vordefinierte Bedingungen erfüllt sind.
<br>
Die Hauptfunktionen eines Trigger-Sensors sind:


1. **Überwachung von Zuständen:** Der Sensor beobachtet den aktuellen Zustand von ausgewählten Entitäten, wie beispielsweise Sensoren, Schaltern oder anderen Geräten.


2. **Ereignisbasierte Auslösung:** Der Sensor reagiert auf vordefinierte Ereignisse, die durch Änderungen in den überwachten Zuständen oder durch spezifische Aktionen ausgelöst werden.


3. **Automatisierte Aktionen:** Bei Auslösung führt der Sensor vorher festgelegte Automatisierungen oder Aktionen aus, wie das Einschalten von Lichtern, das Senden von Benachrichtigungen oder das Starten anderer Geräte.


4. **Anpassbare Konfiguration:** Der Trigger-Sensor ermöglicht eine flexible Konfiguration, einschließlich der Auswahl der zu überwachenden Entitäten, der Definition von Auslösebedingungen und der Festlegung von Reaktionen.


Beispiele für Trigger-Sensoren in Home Assistant sind State-Trigger, Time-Trigger oder Event-Trigger. Diese Sensoren spielen eine entscheidende Rolle bei der Automatisierung von Smart-Home-Funktionen, indem sie eine nahtlose Integration verschiedener Geräte und Plattformen ermöglichen.


# HELFER BUTTON

Die nachfolgenden Trigger-Templates werden alle 15 Minuten sowie bei einem Neustart von Home Assistant ausgelöst. Um unerwünschte LOG-Einträge zu vermeiden, habe ich einen Input-Button als Auslöser verwendet, anstelle den Zeitinterval `time-pattern` direkt im Template einzusetzen. Die Betätigung dieses Buttons erfolgt durch eine entsprechende Automatisierung.

Zu diesem Zweck habe ich einen Helfer `input_button` mit dem Namen `trigger_update` konfiguriert.

Dieser wird über die Automatisierung `Trigger Update`, für welche ich ein Blueprint erstellt habe, gedrückt.

Um die Blaupause zu installieren, kann folgender Link verwendet werden:

https://gist.github.com/MaxxKra/4b8e2fbc5555a85669288ae4573b8815


