# Kalender-Termine-Erinnerung
Mit dieser Zusammenstellung von Home Assistant Trigger-Templates und Notify-Automatisierungen besteht die Möglichkeit, Termine aus dem Kalender übersichtlich für heute oder morgen zu erfassen. Zusätzlich kann der nächste anstehende Termin aus dem Kalender auf dem Dashboard angezeigt oder durch eine Notify-Benachrichtigung hervorgehoben werden.


<br>

### <p align="center">Wenn du Interesse daran hast, mich, meinen Kanal oder meine kreative Arbeit zu unterstützen,<br>freue ich mich über jeglichen Support:
</p>
<p align="center">
  <a href="https://www.buymeacoffee.com/">
    <img src="https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png" alt="Buy Me A Coffee">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://www.paypal.me/kramlmaxx">
    <img src="https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif" alt="PayPal Donate">
  </a>
</p>

### <p align="center">Danke</p>

<br>


## :one: ÜBER TRIGGER SENDOREN/TEMPLATES


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


<img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Termine/Button_Trigger_Update.png" alt="Example" width="600"/>


Dieser wird über die Automatisierung `Trigger Update`, für welche ich ein Blueprint erstellt habe, gedrückt.

Um die Blaupause zu installieren, kann folgender Link verwendet werden:

https://gist.github.com/MaxxKra/4b8e2fbc5555a85669288ae4573b8815


## :two: TEMPLATE TERMINE HEUTE


Alle Templates sind mit meinem Kalender `calendar.meine_termine` angelegt.
Dieser Kalender muss bei den Templates auf den eigenen angepasst werden.
Insgesamt ist der Kalender 3 x im Template eingetragen.

<img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Termine/Template_Korrekturen.png" alt="Example" width="600"/>


In meinem Google-Kalender habe ich für die Tests folgende Termine eingetragen:


<img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Termine/Kalender_Termine_Heute.png" alt="Example" width="600"/>


Diese werden in Home Assistant so angezeigt:


<img src="https://raw.githubusercontent.com/MaxxKra/README_images/master/Termine/HA_Termine_Heute.png" alt="Example" width="600"/>
<details>


Dazu muss lediglich das folgende Template in Home Assistant angelegt werden.


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
  - platform: state
    entity_id:
      - input_button.trigger_update
      
#----------------------------------------------------------------------------------------
# Aktion - Events abrufen (60 Tage)
#----------------------------------------------------------------------------------------
action:
  - service: calendar.get_events
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



## :three: TEMPLATE TERMINE MORGEN



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
  - platform: state
    entity_id:
      - input_button.trigger_update
      
#----------------------------------------------------------------------------------------
# Aktion - Events abrufen (60 Tage)
#----------------------------------------------------------------------------------------
action:
  - service: calendar.get_events
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


## :four: TEMPLATE NÄCHSTER TERMIN HEUTE



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
  - platform: state
    entity_id:
      - input_button.trigger_update
      
#----------------------------------------------------------------------------------------
# Aktion - Event abrufen in 30 Minuten für 15 Minuten
#----------------------------------------------------------------------------------------
action:
  - service: calendar.get_events
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


## :five: TEMPLATE TERMIN MORGEN UM DIESE ZEIT



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
  - platform: state
    entity_id:
      - input_button.trigger_update
      
#----------------------------------------------------------------------------------------
# Aktion - Events abrufen in 24 Stunden für 15 Minuten
#----------------------------------------------------------------------------------------
action:
  - service: calendar.get_events
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
