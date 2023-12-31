# Request Talks informations from pretlx API

[Open Project TI Task 51](https://project.geolibres.org.ar/wp/51)

Lendo a documentação da API do pretalx  para termos acesso à informação do evento, podemos acessar por API.

Para isso é importante que o request seja feito por um usuário cadastrado e vinculado ao projeto talks.osgeo para pode acessar aos dados do eveneto.

Uma vez cadastrado e vinculado, basta acessar a pagina https://talks.osgeo.org/orga/me para poder identificar o token de autenticação a ser usado no request.

Tendo isso já seria possível acessar vários endpoint, mas imagino que os de maior interesse seriam:

events/{event}/talks/ para ter a lista de palestras já aceitas e organizadas no cronograma do evento;

The talk resource is the same as the submission resource, but will limit the returned submissions to talks that already have a slot on the current schedule.

Cada palestra terá as seguintes informações:

| Field | Type | Description |
|---|---|---|
| code | string | A unique, alphanumeric identifier, also used in URLs |
| speakers | list | A list of speaker objects, e.g. [{"name": "Jane", "code": "ABCDEF", "biography": ""}] |
| created | string | The time of submission creation as an ISO-8601 formatted datetime. Available if the requesting user has organiser permission. |
| title | string | The submission’s title |
| submission_type | string | The submission type (e.g. “talk”, “workshop”) |
| submission_type_id | number | ID of the submission type |
| track | string | The track this talk belongs to (e.g. “security”, “design”, or null) |
| track_id | number | ID of the track this talk belongs to (e.g. “security”, “design”, or null) |
| state | string | The submission’s state, one of “submitted”, “accepted”, “rejected”, “confirmed” |
| abstract | string | The abstract, a short note of the submission’s content |
| description | string | The description, a more expansive description of the submission’s content |
| duration | number | The talk’s duration in minutes, or null |
| do_not_record | boolean | Indicates if the speaker consent to recordings of their talk |
| is_featured | boolean | Indicates if the talk is show in the schedule preview / sneak peek |
| content_locale | string | The language the submission is in, e.g. “en” or “de” |
| slot | object | An object with the scheduling details, e.g. {"start": …, "end": …, "room": "R101", "room_id": 12} if they exist. This will not be present til after the schedule is released. |
| slot_count | number | How often this submission may be scheduled. |
| answers | list | The question answers given by the speakers. Available if the requesting user has organiser permissions, and if the questions query parameter is passed. |
| notes | string | Notes the speaker left for the organisers. Available if the requesting user has organiser permissions. |
| internal_notes | string | Notes the organisers left on the submission. Available if the requesting user has organiser permissions. |
| tags | list | The tags attached to the current submission, as a list of strings. Available if the requesting user has organiser or reviewer permissions. |
| tag_ids | list | The tags attached to the current submission, as a list of IDs. Available if the requesting user has organiser or reviewer permissions. |

Para mais informações: <https://docs.pretalx.org/api/resources/talks.html>



Exemplor de código:

```python

import requests
from urllib import parse
from decouple import config
pretalx_token = config('PRETALX_TOKEN')

BASE_URL = 'https://talks.osgeo.org/api/'

# Getting event information
EVENT = 'events/'
event_response = requests.get(parse.urljoin(BASE_URL, EVENT), headers={'Authorization': pretalx_token})
event_response = event_response.json()

#getting talks
TALKS = parse.urljoin(EVENT, f"{event_response[0].get('slug')}/talks/")
response = requests.get(parse.urljoin(BASE_URL,
                                      TALKS), headers={'Authorization': pretalx_token})
response = response.json()
```
