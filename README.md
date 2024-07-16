

# Get information about next departures

A sensor platform which allows you to get information about departures from a specified OEBB stop.






## Example configuration.yaml

```yaml
sensor:
  platform: oebb
  evaId: 491116
  dirInput: 491123

template:
  - sensor:
      - name: "oebb0"
        state: >
          {{ strptime(state_attr('sensor.oebb_journey_0', 'startDate') + " " + state_attr('sensor.oebb_journey_0', 'startTime'), '%d/%m/%Y %H:%M') }}
        attributes:
          actualDeparture: > 
            {% if state_attr('sensor.oebb_journey_0', 'rt') == false %}
            {{ strptime(state_attr('sensor.oebb_journey_0', 'startDate') + " " + state_attr('sensor.oebb_journey_0', 'startTime'), '%d/%m/%Y %H:%M') }}
            {% elif state_attr('sensor.oebb_journey_0', 'rt')['status'] == 'Ausfall' %}
            {{ strptime(state_attr('sensor.oebb_journey_0', 'startDate') + " " + state_attr('sensor.oebb_journey_0', 'startTime'), '%d/%m/%Y %H:%M') }}
            {% elif state_attr('sensor.oebb_journey_0', 'rt')['status'] is none %}
            {{ strptime(state_attr('sensor.oebb_journey_0', 'startDate') + " " + state_attr('sensor.oebb_journey_0', 'rt')['dlt'], '%d/%m/%Y %H:%M') }}
            {% endif %}
          scheduledTime: >
            {{ state_attr('sensor.oebb_journey_0', 'startTime') }}
          actualTime: >
            {% if state_attr('sensor.oebb_journey_0', 'rt') == false %}
            {{ state_attr('sensor.oebb_journey_0', 'startTime') }}
            {% elif state_attr('sensor.oebb_journey_0', 'rt')['status'] is none %}
            {{ state_attr('sensor.oebb_journey_0', 'rt')['dlt'] }}
            {% elif state_attr('sensor.oebb_journey_0', 'rt')['status'] == 'Ausfall' %}
            {{ state_attr('sensor.oebb_journey_0', 'startTime') }}
            {% endif %}
          lastStop: >
            {{ ((state_attr('sensor.oebb_journey_0', 'lastStop'))) | replace('Bahnhof', '') | replace('Hbf', '') | replace('.', '. ') | replace('&#246;', 'ö') }}
          line: >
            {{ ((state_attr('sensor.oebb_journey_0', 'line'))) | replace(' ', '') }}
          status: >
            {% if state_attr('sensor.oebb_journey_0', 'rt') == false %}
            Pünktlich
            {% elif state_attr('sensor.oebb_journey_0', 'rt')['status'] is none %}
            Verspätet
            {% elif state_attr('sensor.oebb_journey_0', 'rt')['status'] == 'Ausfall' %}
            Ausfall
            {% endif %}
            
      - name: "oebb1"
        state: >
          {{ strptime(state_attr('sensor.oebb_journey_1', 'startDate') + " " + state_attr('sensor.oebb_journey_1', 'startTime'), '%d/%m/%Y %H:%M') }}
        attributes:
          actualDeparture: > 
            {% if state_attr('sensor.oebb_journey_1', 'rt') == false %}
            {{ strptime(state_attr('sensor.oebb_journey_1', 'startDate') + " " + state_attr('sensor.oebb_journey_1', 'startTime'), '%d/%m/%Y %H:%M') }}
            {% elif state_attr('sensor.oebb_journey_1', 'rt')['status'] == 'Ausfall' %}
            {{ strptime(state_attr('sensor.oebb_journey_1', 'startDate') + " " + state_attr('sensor.oebb_journey_1', 'startTime'), '%d/%m/%Y %H:%M') }}
            {% elif state_attr('sensor.oebb_journey_1', 'rt')['status'] is none %}
            {{ strptime(state_attr('sensor.oebb_journey_1', 'startDate') + " " + state_attr('sensor.oebb_journey_1', 'rt')['dlt'], '%d/%m/%Y %H:%M') }}
            {% endif %}
          scheduledTime: >
            {{ state_attr('sensor.oebb_journey_1', 'startTime') }}
          actualTime: >
            {% if state_attr('sensor.oebb_journey_1', 'rt') == false %}
            {{ state_attr('sensor.oebb_journey_1', 'startTime') }}
            {% elif state_attr('sensor.oebb_journey_1', 'rt')['status'] is none %}
            {{ state_attr('sensor.oebb_journey_1', 'rt')['dlt'] }}
            {% elif state_attr('sensor.oebb_journey_1', 'rt')['status'] == 'Ausfall' %}
            {{ state_attr('sensor.oebb_journey_1', 'startTime') }}
            {% endif %}
          lastStop: >
            {{ ((state_attr('sensor.oebb_journey_1', 'lastStop'))) | replace('Bahnhof', '') | replace('Hbf', '') | replace('.', '. ') | replace('&#246;', 'ö') }}
          line: >
            {{ ((state_attr('sensor.oebb_journey_1', 'line'))) | replace(' ', '') }}
          status: >
            {% if state_attr('sensor.oebb_journey_1', 'rt') == false %}
            Pünktlich
            {% elif state_attr('sensor.oebb_journey_1', 'rt')['status'] is none %}
            Verspätet
            {% elif state_attr('sensor.oebb_journey_1', 'rt')['status'] == 'Ausfall' %}
            Ausfall
            {% endif %}

```

## Configuration variables

key | description
-- | --
**platform (Required)** | The platform name.
**evaID (Required)** | OEBB Stop ID
**dirInput (Optional)** | OEBB stop ID - filter journey stopping there
**boardType (Optional)** | ? 
**tickerID (Optional)** | ? 
**start (Optional)** | ? 
**eqstop (Optional)** | ? 
**showJourneys (Optional)** | ? number of journeys to retrieve
**additionalTime (Optional)** | ? offset to query next journeys

## Sample overview

![Sample overview](overview.png)

## Notes

To retrieve the parameters please vitit the website, choose your stop and check the iframe url:
https://fahrplan.oebb.at/bin/stboard.exe/
Additional info on the parameters can also be found on the getStationBoardDataOptions() docu in the mymro repo - Thanks!

Sources: 
https://github.com/mymro/oebb-api

This platform is using the [OEBB API]([https://fahrplan.oebb.at/bin/stboard.exe/]) API to get the information.

