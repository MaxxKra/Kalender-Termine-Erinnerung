# Kalender-Termine-Erinnerung
Mit dieser Zusammenstellung von Home Assistant Trigger-Templates und Notify-Automatisierungen besteht die Möglichkeit, Termine aus dem Kalender übersichtlich für heute oder morgen zu erfassen. Zusätzlich kann der nächste anstehende Termin aus dem Kalender auf dem Dashboard angezeigt oder durch eine Notify-Benachrichtigung hervorgehoben werden.


<br>


### <p align="center">Wenn du Interesse daran hast, mich, meinen Kanal oder meine kreative Arbeit zu unterstützen,<br>freue ich mich über jeglichen Support:
</p>
<p align="center">
  <a href="https://www.buymeacoffee.com/bastler">
    <img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Allgemein/buy_me_a_coffee_logo.png" alt="Buy Me A Coffee" width="150">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://www.paypal.me/kramlmaxx">
    <img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Allgemein/paypal_donate_logo.png" alt="PayPal Donate" width="150">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://www.amazon.de/hz/wishlist/ls/3FT7MNGRVOTM3?ref_=wl_share">
    <img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Allgemein/amazon_wishlist_logo.png" alt="Amazon Wishlist" width="150">
  </a>
</p>

### <p align="center">Danke</p>

<br>


# :one: ÜBER TRIGGER SENSOREN/TEMPLATES


Ein Home Assistant Trigger-Sensor ist ein Komponententyp in der Home Assistant-Plattform, der dazu dient, auf bestimmte Ereignisse oder Zustände zu reagieren und automatisierte Aktionen auszulösen. Dieser Sensor überwacht kontinuierlich den Status von bestimmten Entitäten oder Plattformen und wird aktiviert, wenn vordefinierte Bedingungen erfüllt sind.
<br>
Die Hauptfunktionen eines Trigger-Sensors sind:


1. **Überwachung von Zuständen:** Der Sensor beobachtet den aktuellen Zustand von ausgewählten Entitäten, wie beispielsweise Sensoren, Schaltern oder anderen Geräten.


2. **Ereignisbasierte Auslösung:** Der Sensor reagiert auf vordefinierte Ereignisse, die durch Änderungen in den überwachten Zuständen oder durch spezifische Aktionen ausgelöst werden.


3. **Automatisierte Aktionen:** Bei Auslösung führt der Sensor vorher festgelegte Automatisierungen oder Aktionen aus, wie das Einschalten von Lichtern, das Senden von Benachrichtigungen oder das Starten anderer Geräte.


4. **Anpassbare Konfiguration:** Der Trigger-Sensor ermöglicht eine flexible Konfiguration, einschließlich der Auswahl der zu überwachenden Entitäten, der Definition von Auslösebedingungen und der Festlegung von Reaktionen.


Beispiele für Trigger-Sensoren in Home Assistant sind State-Trigger, Time-Trigger oder Event-Trigger. Diese Sensoren spielen eine entscheidende Rolle bei der Automatisierung von Smart-Home-Funktionen, indem sie eine nahtlose Integration verschiedener Geräte und Plattformen ermöglichen.


## HELFER BUTTON

Die nachfolgenden Trigger-Templates werden alle 15 Minuten sowie bei einem Neustart von Home Assistant ausgelöst. Um unerwünschte LOG-Einträge zu vermeiden, habe ich einen Input-Button als Auslöser verwendet, anstelle den Zeitinterval `time-pattern` direkt im Template einzusetzen. Die Betätigung dieses Buttons erfolgt durch eine entsprechende Automatisierung.

Zu diesem Zweck habe ich einen Helfer `input_button` mit dem Namen `trigger_update` konfiguriert.


<img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Termine/Button_Trigger_Update.png" alt="Example" width="600"/>


Dieser wird über die Automatisierung `Trigger Update`, für welche ich ein Blueprint erstellt habe, gedrückt.

Um die Blaupause zu installieren, kann folgender Link verwendet werden:

https://gist.github.com/MaxxKra/4b8e2fbc5555a85669288ae4573b8815


Für den Fall, dass jemand lieber die Automatisierung selber anlegen möchte, ist hier ein Vorlage:
<details>


### TRIGGER AUTOMATISIERUNG

<summary>:arrow_down_small: TRIGGER AUTOMATISIERUNG :arrow_down_small:</summary>


<img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Termine/Automatisierung_Trigger_Update.png" alt="Example" width="800"/>


```yaml
description: ""
mode: single
trigger:
  - trigger: homeassistant
    event: start
  - trigger: time_pattern
    minutes: /15
condition: []
action:
  - action: input_button.press
    metadata: {}
    data: {}
    target:
      entity_id: input_button.trigger_update
```

</details>

<br>



# :two: TEMPLATE TERMINE HEUTE


Alle Templates sind mit meinem Kalender `calendar.meine_termine` angelegt.
Dieser Kalender muss bei den Templates auf den eigenen angepasst werden.
Insgesamt ist der Kalender 3 x im Template eingetragen.

<img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Termine/Template_Korrekturen.png" alt="Example" width="600"/>


In meinem Google-Kalender habe ich für die Tests folgende Termine eingetragen:


<img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Termine/Kalender_Termine_Heute.png" alt="Example" width="600"/>


Diese werden in Home Assistant so angezeigt:


<img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Termine/HA_Termine_Heute.png" alt="Example" width="800"/>


Dazu muss lediglich das folgende Template in Home Assistant angelegt werden.
> [!IMPORTANT]
> Nicht vergessen den Kalendernamen anzupassen und den Trigger (Button und Automatisierung) einzurichten!

<details>


### TRIGGER-TEMPLATE-SENSOR TERMINE HEUTE

<summary>:arrow_down_small: TRIGGER-TEMPLATE-SENSOR TERMINE HEUTE :arrow_down_small:</summary>

```yaml
#            ######## ########  ####  ######    ######   ######## ########              #
#               ##    ##     ##  ##  ##    ##  ##    ##  ##       ##     ##             #
#               ##    ##     ##  ##  ##        ##        ##       ##     ##             #
#               ##    ########   ##  ##   #### ##   #### ######   ########              #
#               ##    ##   ##    ##  ##    ##  ##    ##  ##       ##   ##               #
#               ##    ##    ##   ##  ##    ##  ##    ##  ##       ##    ##              #
#               ##    ##     ## ####  ######    ######   ######## ##     ##             #
#                                                                                       #
#             ######  ######## ##    ##  ######   #######  ########                     #
#            ##    ## ##       ###   ## ##    ## ##     ## ##     ##                    #
#            ##       ##       ####  ## ##       ##     ## ##     ##                    #
#             ######  ######   ## ## ##  ######  ##     ## ########                     #
#                  ## ##       ##  ####       ## ##     ## ##   ##                      #
#            ##    ## ##       ##   ### ##    ## ##     ## ##    ##                     #
#             ######  ######## ##    ##  ######   #######  ##     ##                    #

#########################################################################################
#---------------------------------------------------------------------------------------#
##-----------------  Liste von Terminen aus dem Kalender - Für Heute ------------------##
#---------------------------------------------------------------------------------------#
#########################################################################################


#----------------------------------------------------------------------------------------
# Auslöser - Aktion starten (Alle 15 Min. und HA Neustart)
#----------------------------------------------------------------------------------------
trigger:
  - trigger: state
    entity_id:
      - input_button.trigger_update
      
#----------------------------------------------------------------------------------------
# Aktion - Events abrufen (60 Tage)
#----------------------------------------------------------------------------------------
action:
  - action: calendar.get_events
    target:
      entity_id: calendar.meine_termine
    data:
      start_date_time: "{{ as_timestamp(now()) | timestamp_custom('%Y-%m-%d 00:00:01', true) }}"
      end_date_time: "{{ as_timestamp(now()) | timestamp_custom('%Y-%m-%d 23:59:59', true) }}"
    response_variable: today_events

#----------------------------------------------------------------------------------------
# Sensor mit Attributen anlegen
#----------------------------------------------------------------------------------------
sensor:
  - name: Termine Heute
    unique_id: termine_heute
    state: "{{ today_events['calendar.meine_termine'].events | count() }}"
    attributes:
      termine_heute: >
        {%- set data = namespace(result=[]) %}
        {%- for event in today_events['calendar.meine_termine'].events %}
          {%- set START = strptime(event.start, '%Y-%m-%dT%H:%M:%S%z') %}
          {%- set ENDE = strptime(event.end, '%Y-%m-%dT%H:%M:%S%z') %}
          {%- set UHRZEIT = START.strftime("%H:%M") %}
          {%- set DAUER_MIN = ((ENDE - START).seconds) // 60 %}
          
          {%- set DAUER_STUNDEN = DAUER_MIN // 60 %}
          {%- set DAUER_REST_MINUTEN = DAUER_MIN % 60 %}
          
          {%- set DAUER_STRING = "Dauer: " %}
          {%- if DAUER_STUNDEN > 0 %}
            {%- set DAUER_STRING = DAUER_STRING + DAUER_STUNDEN|string + " Stunde" + ("n" if DAUER_STUNDEN > 1 else "") + " " %}
          {%- endif %}
          {%- if DAUER_REST_MINUTEN > 0 %}
            {%- set DAUER_STRING = DAUER_STRING + DAUER_REST_MINUTEN|string + " Minute" + ("n" if DAUER_REST_MINUTEN > 1 else "") %}
          {%- endif %}
          
          {%- set TERMIN = event.summary %}
          {%- set HINWEIS = event.description | default('') %}
          
          {%- set event_dict = {
              'Uhrzeit': UHRZEIT,
              'Dauer': DAUER_STRING,
              'Termin': TERMIN,
              'Hinweis': HINWEIS
            }
          %}
          
          {%- set data.result = data.result + [(event_dict)] %}
        {%- endfor %}
        {{ data.result }}
    icon: mdi:calendar
```

</details>

<br>



# :three: TEMPLATE TERMINE MORGEN


Für die Termine Morgen habe ich in meinem Google-Kalender für die Tests folgende Termine eingetragen:


<img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Termine/Kalender_Termine_Morgen.png" alt="Example" width="600"/>


Diese werden in Home Assistant so angezeigt:


<img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Termine/HA_Termine_Morgen.png" alt="Example" width="800"/>


Dazu muss lediglich das folgende Template in Home Assistant angelegt werden.
> [!IMPORTANT]
> Nicht vergessen den Kalendernamen anzupassen und den Trigger (Button und Automatisierung) einzurichten!


<details>


### TRIGGER-TEMPLATE-SENSOR TERMINE MORGEN

<summary>:arrow_down_small: TRIGGER-TEMPLATE-SENSOR TERMINE MORGEN :arrow_down_small:</summary>

```yaml
#            ######## ########  ####  ######    ######   ######## ########              #
#               ##    ##     ##  ##  ##    ##  ##    ##  ##       ##     ##             #
#               ##    ##     ##  ##  ##        ##        ##       ##     ##             #
#               ##    ########   ##  ##   #### ##   #### ######   ########              #
#               ##    ##   ##    ##  ##    ##  ##    ##  ##       ##   ##               #
#               ##    ##    ##   ##  ##    ##  ##    ##  ##       ##    ##              #
#               ##    ##     ## ####  ######    ######   ######## ##     ##             #
#                                                                                       #
#             ######  ######## ##    ##  ######   #######  ########                     #
#            ##    ## ##       ###   ## ##    ## ##     ## ##     ##                    #
#            ##       ##       ####  ## ##       ##     ## ##     ##                    #
#             ######  ######   ## ## ##  ######  ##     ## ########                     #
#                  ## ##       ##  ####       ## ##     ## ##   ##                      #
#            ##    ## ##       ##   ### ##    ## ##     ## ##    ##                     #
#             ######  ######## ##    ##  ######   #######  ##     ##                    #

#########################################################################################
#---------------------------------------------------------------------------------------#
##-----------------  Liste von Terminen aus dem Kalender - Für Morgen -----------------##
#---------------------------------------------------------------------------------------#
#########################################################################################


#----------------------------------------------------------------------------------------
# Auslöser - Aktion starten (Alle 15 Min. und HA Neustart)
#----------------------------------------------------------------------------------------
trigger:
  - trigger: state
    entity_id:
      - input_button.trigger_update
      
#----------------------------------------------------------------------------------------
# Aktion - Events abrufen (60 Tage)
#----------------------------------------------------------------------------------------
action:
  - action: calendar.get_events
    target:
      entity_id: calendar.meine_termine
    data:
      start_date_time: "{{ (as_timestamp(now()) + 86400) | timestamp_custom('%Y-%m-%d 00:00:01', true) }}"
      end_date_time: "{{ (as_timestamp(now()) + 86400) | timestamp_custom('%Y-%m-%d 23:59:59', true) }}"
    response_variable: tomorrow_events

#----------------------------------------------------------------------------------------
# Sensor mit Attributen anlegen
#----------------------------------------------------------------------------------------
sensor:
  - name: Termine Morgen
    unique_id: termine_morgen
    state: "{{ tomorrow_events['calendar.meine_termine'].events | count() }}"
    attributes:
      termine_morgen: >
        {%- set data = namespace(result=[]) %}
        {%- for event in tomorrow_events['calendar.meine_termine'].events %}
          {%- set START = strptime(event.start, '%Y-%m-%dT%H:%M:%S%z') %}
          {%- set ENDE = strptime(event.end, '%Y-%m-%dT%H:%M:%S%z') %}
          {%- set UHRZEIT = START.strftime("%H:%M") %}
          {%- set DAUER_MIN = ((ENDE - START).seconds) // 60 %}
          
          {%- set DAUER_STUNDEN = DAUER_MIN // 60 %}
          {%- set DAUER_REST_MINUTEN = DAUER_MIN % 60 %}
          
          {%- set DAUER_STRING = "Dauer: " %}
          {%- if DAUER_STUNDEN > 0 %}
            {%- set DAUER_STRING = DAUER_STRING + DAUER_STUNDEN|string + " Stunde" + ("n" if DAUER_STUNDEN > 1 else "") + " " %}
          {%- endif %}
          {%- if DAUER_REST_MINUTEN > 0 %}
            {%- set DAUER_STRING = DAUER_STRING + DAUER_REST_MINUTEN|string + " Minute" + ("n" if DAUER_REST_MINUTEN > 1 else "") %}
          {%- endif %}
          
          {%- set TERMIN = event.summary %}
          {%- set HINWEIS = event.description | default('') %}
          
          {%- set event_dict = {
              'Uhrzeit': UHRZEIT,
              'Dauer': DAUER_STRING,
              'Termin': TERMIN,
              'Hinweis': HINWEIS
            }
          %}
          
          {%- set data.result = data.result + [(event_dict)] %}
        {%- endfor %}
        {{ data.result }}
    icon: mdi:calendar
```

</details>

<br>


# :four: TEMPLATE NÄCHSTER TERMIN HEUTE


Um eine Terminerinnerung 30 Minuten vorher zu erhalten, habe ich dieses Template angelegt.


Für den nächsten Termin Heute, habe ich in meinem Google-Kalender für die Tests folgende Termine eingetragen:


<img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Termine/Kalender_Termine_Heute.png" alt="Example" width="600"/>


Diese werden in Home Assistant um 05:30 Uhr so angezeigt:


<img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Termine/HA_Termin_Heute_Nächster.png" alt="Example" width="800"/>


Dazu muss lediglich das folgende Template in Home Assistant angelegt werden.
> [!IMPORTANT]
> Nicht vergessen den Kalendernamen anzupassen und den Trigger (Button und Automatisierung) einzurichten!



<details>


### TRIGGER-TEMPLATE-SENSOR NÄCHSTER TERMINE HEUTE

<summary>:arrow_down_small: TRIGGER-TEMPLATE-SENSOR NÄCHSTER TERMINE HEUTE :arrow_down_small:</summary>

```yaml
#            ######## ########  ####  ######    ######   ######## ########              #
#               ##    ##     ##  ##  ##    ##  ##    ##  ##       ##     ##             #
#               ##    ##     ##  ##  ##        ##        ##       ##     ##             #
#               ##    ########   ##  ##   #### ##   #### ######   ########              #
#               ##    ##   ##    ##  ##    ##  ##    ##  ##       ##   ##               #
#               ##    ##    ##   ##  ##    ##  ##    ##  ##       ##    ##              #
#               ##    ##     ## ####  ######    ######   ######## ##     ##             #
#                                                                                       #
#             ######  ######## ##    ##  ######   #######  ########                     #
#            ##    ## ##       ###   ## ##    ## ##     ## ##     ##                    #
#            ##       ##       ####  ## ##       ##     ## ##     ##                    #
#             ######  ######   ## ## ##  ######  ##     ## ########                     #
#                  ## ##       ##  ####       ## ##     ## ##   ##                      #
#            ##    ## ##       ##   ### ##    ## ##     ## ##    ##                     #
#             ######  ######## ##    ##  ######   #######  ##     ##                    #

#########################################################################################
#---------------------------------------------------------------------------------------#
##----------------------  Nächster Termin Heute aus dem Kalender ----------------------##
#---------------------------------------------------------------------------------------#
#########################################################################################


#----------------------------------------------------------------------------------------
# Auslöser - Aktion starten (Alle 15 Min. und HA Neustart) über Automatisierung
#----------------------------------------------------------------------------------------
trigger:
  - trigger: state
    entity_id:
      - input_button.trigger_update
      
#----------------------------------------------------------------------------------------
# Aktion - Event abrufen in 30 Minuten für 15 Minuten
#----------------------------------------------------------------------------------------
action:
  - action: calendar.get_events
    target:
      entity_id: calendar.meine_termine
    data:
      start_date_time: "{{ (as_timestamp(now()) + 1800) | timestamp_custom('%Y-%m-%d %H:%M:%S', true) }}"
      end_date_time: "{{ (as_timestamp(now()) + 2700) | timestamp_custom('%Y-%m-%d %H:%M:%S', true) }}"
    response_variable: today_events_next

#----------------------------------------------------------------------------------------
# Sensor mit Attributen anlegen
#----------------------------------------------------------------------------------------
sensor:
  - name: Termine Heute Nächster
    unique_id: termine_heute_nachster
    state: "{{ today_events_next['calendar.meine_termine'].events | count() }}"
    attributes:
      termine_heute: >
        {%- set data = namespace(result=[]) %}
        {%- for event in today_events_next['calendar.meine_termine'].events %}
          {%- set START = strptime(event.start, '%Y-%m-%dT%H:%M:%S%z') %}
          {%- set ENDE = strptime(event.end, '%Y-%m-%dT%H:%M:%S%z') %}
          {%- set UHRZEIT = START.strftime("%H:%M") %}
          {%- set DAUER_MIN = ((ENDE - START).seconds) // 60 %}
          
          {%- set DAUER_STUNDEN = DAUER_MIN // 60 %}
          {%- set DAUER_REST_MINUTEN = DAUER_MIN % 60 %}
          
          {%- set DAUER_STRING = "Dauer: " %}
          {%- if DAUER_STUNDEN > 0 %}
            {%- set DAUER_STRING = DAUER_STRING + DAUER_STUNDEN|string + " Stunde" + ("n" if DAUER_STUNDEN > 1 else "") + " " %}
          {%- endif %}
          {%- if DAUER_REST_MINUTEN > 0 %}
            {%- set DAUER_STRING = DAUER_STRING + DAUER_REST_MINUTEN|string + " Minute" + ("n" if DAUER_REST_MINUTEN > 1 else "") %}
          {%- endif %}
          
          {%- set TERMIN = event.summary %}
          {%- set HINWEIS = event.description | default('') %}
          
          {%- set event_dict = {
              'Uhrzeit': UHRZEIT,
              'Dauer': DAUER_STRING,
              'Termin': TERMIN,
              'Hinweis': HINWEIS
            }
          %}
          
          {%- set data.result = data.result + [(event_dict)] %}
        {%- endfor %}
        {{ data.result }}
    icon: mdi:calendar
```

</details>

<br>


# :five: TEMPLATE TERMIN MORGEN UM DIESE ZEIT

Um eine Terminerinnerung 24 Stunden vorher zu erhalten, habe ich dieses Template angelegt.


Für den Termin Morgen um diese Zeit, habe ich in meinem Google-Kalender für die Tests folgende Termine eingetragen:


<img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Termine/Kalender_Termine_Morgen.png" alt="Example" width="600"/>


Diese werden in Home Assistant heute um 05:30 Uhr so angezeigt:


<img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Termine/HA_Termin_Morgen_Nächster.png" alt="Example" width="800"/>


Dazu muss lediglich das folgende Template in Home Assistant angelegt werden.
> [!IMPORTANT]
> Nicht vergessen den Kalendernamen anzupassen und den Trigger (Button und Automatisierung) einzurichten!



<details>


### TRIGGER-TEMPLATE-SENSOR TERMIN MORGEN NÄCHSTER

<summary>:arrow_down_small: TRIGGER-TEMPLATE-SENSOR TERMIN MORGEN NÄCHSTER :arrow_down_small:</summary>

```yaml
#            ######## ########  ####  ######    ######   ######## ########              #
#               ##    ##     ##  ##  ##    ##  ##    ##  ##       ##     ##             #
#               ##    ##     ##  ##  ##        ##        ##       ##     ##             #
#               ##    ########   ##  ##   #### ##   #### ######   ########              #
#               ##    ##   ##    ##  ##    ##  ##    ##  ##       ##   ##               #
#               ##    ##    ##   ##  ##    ##  ##    ##  ##       ##    ##              #
#               ##    ##     ## ####  ######    ######   ######## ##     ##             #
#                                                                                       #
#             ######  ######## ##    ##  ######   #######  ########                     #
#            ##    ## ##       ###   ## ##    ## ##     ## ##     ##                    #
#            ##       ##       ####  ## ##       ##     ## ##     ##                    #
#             ######  ######   ## ## ##  ######  ##     ## ########                     #
#                  ## ##       ##  ####       ## ##     ## ##   ##                      #
#            ##    ## ##       ##   ### ##    ## ##     ## ##    ##                     #
#             ######  ######## ##    ##  ######   #######  ##     ##                    #

#########################################################################################
#---------------------------------------------------------------------------------------#
##------------  Termin Morgen um diese Zeit von Terminen aus dem Kalender -------------##
#---------------------------------------------------------------------------------------#
#########################################################################################


#----------------------------------------------------------------------------------------
# Auslöser - Aktion starten (Alle 15 Min. und HA Neustart) durch Automatisierung
#----------------------------------------------------------------------------------------
trigger:
  - trigger: state
    entity_id:
      - input_button.trigger_update
      
#----------------------------------------------------------------------------------------
# Aktion - Events abrufen in 24 Stunden für 15 Minuten
#----------------------------------------------------------------------------------------
action:
  - action: calendar.get_events
    target:
      entity_id: calendar.meine_termine
    data:
      start_date_time: "{{ (as_timestamp(now()) + 86400) | timestamp_custom('%Y-%m-%d %H:%M:%S', true) }}"
      end_date_time: "{{ (as_timestamp(now()) + 87300) | timestamp_custom('%Y-%m-%d %H:%M:%S', true) }}"
    response_variable: tomorrow_events_next

#----------------------------------------------------------------------------------------
# Sensor mit Attributen anlegen
#----------------------------------------------------------------------------------------
sensor:
  - name: Termine Morgen Nächster
    unique_id: termine_morgen_nachster
    state: "{{ tomorrow_events_next['calendar.meine_termine'].events | count() }}"
    attributes:
      termine_morgen: >
        {%- set data = namespace(result=[]) %}
        {%- for event in tomorrow_events_next['calendar.meine_termine'].events %}
          {%- set START = strptime(event.start, '%Y-%m-%dT%H:%M:%S%z') %}
          {%- set ENDE = strptime(event.end, '%Y-%m-%dT%H:%M:%S%z') %}
          {%- set UHRZEIT = START.strftime("%H:%M") %}
          {%- set DAUER_MIN = ((ENDE - START).seconds) // 60 %}
          
          {%- set DAUER_STUNDEN = DAUER_MIN // 60 %}
          {%- set DAUER_REST_MINUTEN = DAUER_MIN % 60 %}
          
          {%- set DAUER_STRING = "Dauer: " %}
          {%- if DAUER_STUNDEN > 0 %}
            {%- set DAUER_STRING = DAUER_STRING + DAUER_STUNDEN|string + " Stunde" + ("n" if DAUER_STUNDEN > 1 else "") + " " %}
          {%- endif %}
          {%- if DAUER_REST_MINUTEN > 0 %}
            {%- set DAUER_STRING = DAUER_STRING + DAUER_REST_MINUTEN|string + " Minute" + ("n" if DAUER_REST_MINUTEN > 1 else "") %}
          {%- endif %}
          
          {%- set TERMIN = event.summary %}
          {%- set HINWEIS = event.description | default('') %}
          
          {%- set event_dict = {
              'Uhrzeit': UHRZEIT,
              'Dauer': DAUER_STRING,
              'Termin': TERMIN,
              'Hinweis': HINWEIS
            }
          %}
          
          {%- set data.result = data.result + [(event_dict)] %}
        {%- endfor %}
        {{ data.result }}
    icon: mdi:calendar
```

</details>

<br>


# :six: NOTIFY AUTOMATISIERUNGEN


Die folgenden Automatisierungen werden durch eine Veränderung des Sensors (Nächster Termin Heute/Morgen) ausgelöst.
Eine Bedingung ist, dass mindestens 1 Termin ansteht (also nicht 0)


<img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Termine/HA_Termin_Heute_Nächster_Anzahl.png" alt="Example" width="800"/>


Als Ergebnis werden die Erinnerungen als Notify an das Smartphone gesendet:


<img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Termine/Notifys.png" alt="Example" width="500"/>


Die Automatisierung ist hier als YAML-Code zum kopieren bereit gestellt.
> [!IMPORTANT]
> Nicht vergessen `notify.<DEIN-SMARTPHONE>` auf deine Entität zu Ändern!


```yaml
alias: Termin Heute Notify
description: ""
trigger:
  - trigger: state
    entity_id:
      - sensor.termine_heute_nachster
    attribute: termine_heute
condition:
  - condition: not
    conditions:
      - condition: state
        entity_id: sensor.termine_morgen_nachster
        state: "0"
action:
  - action: notify.<DEIN-SMARTPHONE>
    metadata: {}
    data:
      title: Nächster Termin in 30 Minuten!
      message: >-
        Uhrzeit:
        {{states.sensor.termine_heute_nachster.attributes.termine_heute.0.Uhrzeit}}
        Uhr

        Termin:
        {{states.sensor.termine_heute_nachster.attributes.termine_heute.0.Termin
        }}

        {{
        states.sensor.termine_heute_nachster.attributes.termine_heute.0.Dauer}}

        Hinweis:
        {{states.sensor.termine_heute_nachster.attributes.termine_heute.0.Hinweis
        }}
mode: single
```


Die Erinnerung für Morgen verhällt sich genauso wie die für Heute.
Auch hier wird durch eine Veränderung des Sensors die Automatisierung ausgelöst.


Die Automatisierung ist hier als YAML-Code zum kopieren bereit gestellt.
> [!IMPORTANT]
> Nicht vergessen `notify.<DEIN-SMARTPHONE>` auf deine Entität zu Ändern!


```yaml
alias: Termin Morgen Notify
description: ""
trigger:
  - trigger: state
    entity_id:
      - sensor.termine_morgen_nachster
    attribute: termine_morgen
condition:
  - condition: not
    conditions:
      - condition: state
        entity_id: sensor.termine_morgen_nachster
        state: "0"
action:
  - action: notify.<DEIN-SMARTPHONE>
    metadata: {}
    data:
      title: Termin Morgen um diese Zeit!
      message: >-
        Uhrzeit:
        {{states.sensor.termine_morgen_nachster.attributes.termine_morgen.0.Uhrzeit}}
        Uhr

        Termin:
        {{states.sensor.termine_morgen_nachster.attributes.termine_morgen.0.Termin
        }}

        {{
        states.sensor.termine_morgen_nachster.attributes.termine_morgen.0.Dauer}}

        Hinweis:
        {{states.sensor.termine_morgen_nachster.attributes.termine_morgen.0.Hinweis
        }}
mode: single
```

<br>


# :seven: DASHBOARD OPTIONEN


Auf einem Dashboard könnten die Termine beispielsweise so angezeigt werden:


<img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Termine/Kalender_Monat_DB.png" alt="Example" width="600"/>


```yaml
type: custom:atomic-calendar-revive
name: Unsere Kalender
enableModeChange: true
entities:
  - entity: calendar.meine_termine
    color: '#e6bc47'
    type: icon
    icon: mdi:calendar-account
firstDayOfWeek: 1
maxDaysToShow: 60
refreshInterval: 60
showDate: true
showCurrentEventLine: true
sortByStartTime: true
dimFinishedEvents: false
showLastCalendarWeek: true
disableCalEventLink: true
disableCalLink: true
showNoEventsForToday: false
showProgressBar: true
showWeekDay: true
showWeekNumber: true
disableEventLink: true
disableLocationLink: true
showEventIcon: true
showFullDayProgress: true
showHiddenText: true
dateColor: '#f1d7a6'
timeColor: rgb(119,221,119)
eventTitleColor: '#cc5500'
locationLinkColor: rgb(253, 253, 150)
locationIconColor: rgb(253, 253, 150)
dayWrapperLineColor: rgba(253, 253, 150, .35)
descColor: '#f1d7a6'
eventCalNameColor: '#f1d7a6'
calGridColor: rgba(253, 253, 150, 0.15)
calEventBackgroundColor: rgb(194, 59, 0.15)
calEventSatColor: rgba(12, 56, 100, 0.25)
calEventSunColor: rgba(12, 56, 100, 0.25)
calActiveEventBackgroundColor: rgba(128, 206, 225, 0.3)
defaultCalColor: '#f1d7a6'
calDayColor: '#f1d7a6'
nameColor: '#f1d7a6'
calWeekDayColor: '#f1d7a6'
calDateColor: '#f1d7a6'
cardHeight: 100%
calShowDescription: true
defaultMode: Calendar
showDeclined: true
showLocation: true
showLoader: true
hideDuplicates: true
disableCalLocationLink: true
compactMode: false
showMultiDayEventParts: true
showMultiDay: true
allDayBottom: true
offsetHeaderDate: true
hideFinishedEvents: true
showCalendarName: false
showDatePerEvent: false
showTimeRemaining: false
hoursOnSameLine: false
showMonth: true
titleLength: 0
startDaysAhead: 0
sortBy: start
noEventText: Keine Termine
noEventsForNextDaysText: Keine Termine in den nächsten Tagen
hiddenEventText: Termine sind versteckt
untilText: Bis
view_layout:
  grid-area: Kal
card_mod:
  style: |
    ha-card {
      border: none;
      background: transparent;
      font-family: Arial Rounded MT;  
      color: {{ states['sensor.dashboard_primar_farbe'].state }} !important;
    }
    .calDay {
      font-size: 1.2vw !important;
      color: {{ states['sensor.dashboard_primar_farbe'].state }} !important;
      height: 3vw !important;
      max-width: 80px !important;
    }
    .calIcon {
      --mdc-icon-size: 15px !important;
    }
    thead th.cal {
      font-size: 1.2vw !important; !important;
      height: 3vw !important;
    }

```


<img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Termine/Kalender_Tage_DB.png" alt="Example" width="600"/>


```yaml
type: custom:atomic-calendar-revive
name: Ereignisliste
enableModeChange: true
entities:
  - entity: calendar.meine_termine
    type: icon
    icon: mdi:calendar-alert
    color: '#ec6288'
firstDayOfWeek: 1
maxDaysToShow: 2
refreshInterval: 60
showDate: true
showCurrentEventLine: false
sortByStartTime: false
dimFinishedEvents: true
showLastCalendarWeek: false
disableCalEventLink: true
disableCalLink: true
showNoEventsForToday: false
showProgressBar: true
showWeekDay: true
disableEventLink: true
disableLocationLink: true
showEventIcon: false
showFullDayProgress: false
showHiddenText: false
showLoader: false
showLocation: false
showDescription: true
disableCalLocationLink: true
dateColor: rgb(119,158,203)
timeColor: rgb(119,221,119)
locationLinkColor: rgb(253, 253, 150)
locationIconColor: rgb(253, 253, 150)
dayWrapperLineColor: rgba(253, 253, 150, .35)
descColor: '#00FF00'
eventTitleColor: '#cc5500'
eventCalNameColor: '#1E90FF'
calGridColor: rgba(253, 253, 150, .35)
calEventBackgroundColor: rgb(194, 59, 34)
calEventSatColor: rgba(12, 56, 100, .05)
calEventSunColor: rgba(12, 56, 100, .15)
calActiveEventBackgroundColor: rgba(128, 206, 225, .15)
defaultCalColor: '#00FF00'
calDayColor: rgb(119,221,119)
nameColor: rgb(119,221,119)
calWeekDayColor: '#cc5500'
calDateColor: rgb(119,221,119)
noEventsForNextDaysText: Keine Ereignisse in den nächsten Tagen!
noEventText: Keine Ereignisse
hiddenEventText: Ereignisse sind versteckt!
untilText: Bis
view_layout:
  grid-area: Erin
card_mod:
  style: |
    ha-card {
      border: none;
      background: transparent;
      font-family: Arial Rounded MT;
      color: {{ states['sensor.dashboard_primar_farbe'].state }}
    }

```
