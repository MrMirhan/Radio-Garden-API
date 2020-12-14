# Radio Garden API

My attempt at trying to reverse-engineer and understand the Radio Garden API.

- [Radio Garden API](#radio-garden-api)
	- [Base URLs](#base-urls)
	- [Endpoints](#endpoints)
		- [Get list of places](#get-list-of-places)
			- [Request](#request)
			- [Response](#response)
		- [Get place or country details](#get-place-or-country-details)
			- [Request](#request-1)
			- [Response](#response-1)
		- [Get station details](#get-station-details)
			- [Request](#request-2)
			- [Response](#response-2)
		- [Get station stream URL](#get-station-stream-url)
			- [Request](#request-3)
			- [Response](#response-3)
		- [Search for places, countries and stations](#search-for-places-countries-and-stations)
			- [Request](#request-4)
			- [Response](#response-4)
		- [Get station staff picks](#get-station-staff-picks)
			- [Request](#request-5)
			- [Response](#response-5)
		- [Get client's location via IP](#get-clients-location-via-ip)
			- [Request](#request-6)
			- [Response](#response-6)
	- [Notes](#notes)

## Base URLs

- https://radio.garden/api
- http://radio.garden/api

## Endpoints

### Get list of places

#### Request

- Method: GET
- Endpoint: /ara/content/places

#### Response

- Format: JSON

| Name                 | Type    | Example value          | Description                                                                   |
| -------------------- | ------- | ---------------------- | ----------------------------------------------------------------------------- |
| apiVersion           | Number  | 1                      | API version, 1 is the only one currently known                                |
| version              | String  | 8fc46fcc               | Possibly current Git commit hash of the website                               |
| data                 | Object  |                        |                                                                               |
| data.list            | Array   |                        | List of all areas where radio stations are available                          |
| data.list[n]         | Object  |                        | A place object                                                                |
| data.list[n].id      | String  | PbqG2Mmi               | Unique ID of this place, can be used for subsequent API requests              |
| data.list[n].title   | String  | Ghazni                 | Name of this place                                                            |
| data.list[n].country | String  | Afghanistan            | Country this place is in                                                      |
| data.list[n].url     | String  | /visit/ghazni/PbqG2Mmi | Path that can be appended to the hostname, that results in the front-end link |
| data.list[n].size    | Number  | 1                      | Number of available radio stations from this area                             |
| data.list[n].boost   | Boolean | true                   |                                                                               |
| data.list[n].geo     | Array   |                        | Coordinates of this place                                                     |
| data.list[n].geo[0]  | Number  | 68.417                 | Longitude of this place                                                       |
| data.list[n].geo[1]  | Number  | 33.545                 | Latitude of this place                                                        |
| data.version         | String  | 8fc46fcc               | Same value and description as version                                         |

### Get place or country details

#### Request

- Method: GET
- Endpoint: /ara/content/page/`id`

#### Response

- Format: JSON

| Name                                        | Type   | Example value                 | Description                                                                                                |
| ------------------------------------------- | ------ | ----------------------------- | ---------------------------------------------------------------------------------------------------------- |
| apiVersion                                  | Number | 1                             | API version, 1 is the only one currently known                                                             |
| version                                     | String | 8fc46fcc                      | Possibly current Git commit hash of the website                                                            |
| data                                        | Object |                               |                                                                                                            |
| data.map                                    | String | PbqG2Mmi                      | Unique ID of this place, can be used for subsequent API requests, can also be an array                     |
| data.map                                    | Array  |                               | Coordinates of the country this item represents, can also be a string                                      |
| data.map[0]                                 | Number | 13.404954                     | Longitude of the country this item represents                                                              |
| data.map[1]                                 | Number | 52.5200066                    | Latitude of the country this item represents                                                               |
| data.title                                  | String | Ghazni                        | Name of this place                                                                                         |
| data.url                                    | String | /visit/ghazni/PbqG2Mmi        | Path that can be appended to the hostname, that results in the front-end link                              |
| data.utcOffset                              | Number | 4.5                           | Timezone offset from UTC in hours                                                                          |
| data.subtitle                               | String | Afghanistan                   | Subtitle displayed in the front-end, currently always the country this place is in                         |
| data.count                                  | Number | 1                             | Number of available radio stations from this area                                                          |
| data.content                                | Array  |                               | Lists related to this place, displayed in the front-end                                                    |
| data.content[n]                             | Object |                               | A list object                                                                                              |
| data.content[n].type                        | String | list                          | Currently always list                                                                                      |
| data.content[n].title                       | String | Stations in Ghazni            | Title of this list                                                                                         |
| data.content[n].rightAccessory              | String | chevron-right                 | Content to display at the right side of all list items in the front-end, not available if itemsType is set |
| data.content[n].itemsType                   | String | channel                       | Type of this item, not available if rightAccessory is set, currently always channel                        |
| data.content[n].items                       | Array  |                               | All items in this list                                                                                     |
| data.content[n].items[n]                    | Object |                               | An item object                                                                                             |
| data.content[n].items[n].title              | String | Kilid Ghazni                  | Title of this list item, for example the name of a radio station                                           |
| data.content[n].items[n].subtitle           | String | Islamabad                     | Subtitle of this list item, displayed in the front-end, not always available                               |
| data.content[n].items[n].rightDetail        | String | 134 km                        | Additional details about this item, displayed in grey to the right of the item the front-end               |
| data.content[n].items[n].href               | String | /listen/kilid-ghazni/eP-8QMgM | URL the front-end should redirect to when clicking on this item                                            |
| data.content[n].items[n].map                | String | aqYp0wTm                      | If list item is a place, ID of this place, not always set (items are not always places)                    |
| data.content[n].items[n].page               | Object |                               | If list item redirects to another page, contains details of that page, currently only set on "Go to" items |
| data.content[n].items[n].page.url           | String | /visit/afghanistan/Q6CDd3Y4   | URL the front-end should redirect to when clicking on this item                                            |
| data.content[n].items[n].page.map           | String | bJHG3R4J                      | Unique ID of the place this item represents, can be used for subsequent API requests, can also be an array |
| data.content[n].items[n].page.map           | Array  |                               | Coordinates of the place this item represents, can also be a string                                        |
| data.content[n].items[n].page.map[0]        | Number | 69.207486                     | Longitude of the place this item represents                                                                |
| data.content[n].items[n].page.map[1]        | Number | 34.5553494                    | Latitude of the place this item represents                                                                 |
| data.content[n].items[n].page.title         | String | Kabul                         | The name of the place this item represents, currently always either the area or country                    |
| data.content[n].items[n].page.subtitle      | String | Afghanistan                   | Additional details about this item, not always set, currently always the country if title is the area      |
| data.content[n].items[n].page.count         | Number | 5                             | Number of available radio stations from the area this item represents, not always set                      |
| data.content[n].items[n].type               | String | more                          | Type of this list item, currently always either more or not set                                            |
| data.content[n].items[n].leftAccessory      | String | count                         | Content to display at the left side of this list item in the front-end, not always available               |
| data.content[n].items[n].leftAccessoryCount | Number | 5                             | Only set if leftAccessory is count, number of available radio stations from the area this item represents  |
| data.content[n].items[n].rightAccessory     | String | chevron-right                 | Content to display at the right side of this list item in the front-end, not always available              |
| error                                       | String | Not found                     | Error message, only set if request was unsuccessful                                                        |

### Get station details

#### Request

- Method: GET
- Endpoint: /ara/content/channel/`id`

#### Response

- Format: JSON

| Name               | Type    | Example value                              | Description                                                                          |
| ------------------ | ------- | ------------------------------------------ | ------------------------------------------------------------------------------------ |
| apiVersion         | Number  | 1                                          | API version, 1 is the only one currently known                                       |
| version            | String  | 8fc46fcc                                   | Possibly current Git commit hash of the website                                      |
| data               | Object  |                                            |                                                                                      |
| data.id            | String  | eP-8QMgM                                   | Unique ID of this station, can be used for subsequent API requests                   |
| data.title         | String  | Kilid Ghazni                               | Name of this station                                                                 |
| data.website       | String  | https://tkg.af/radios/radio-killid-ghazni/ | Official website of this station                                                     |
| data.url           | String  | /listen/kilid-ghazni/eP-8QMgM              | Path that can be appended to the hostname, that results in the front-end link        |
| data.secure        | Boolean | false                                      | Whether the radio stream is served via HTTPS                                         |
| data.place         | Object  |                                            | Details about the place this station is in                                           |
| data.place.id      | String  | PbqG2Mmi                                   | Unique ID of the place this station is in, can be used for subsequent API requests   |
| data.place.title   | String  | Ghazni                                     | Name of the place this station is in                                                 |
| data.country       | Object  |                                            | Details about the country this station is in                                         |
| data.country.id    | String  | Q6CDd3Y4                                   | Unique ID of the country this station is in, can be used for subsequent API requests |
| data.country.title | String  | Afghanistan                                | Name of the country this station is in                                               |
| error              | String  | Not found                                  | Error message, only set if request was unsuccessful                                  |

### Get station stream URL

#### Request

- Method: HEAD or GET
- Endpoint: /ara/content/listen/`id`/channel.mp3

#### Response

- Format: HTTP Redirect

### Search for places, countries and stations

#### Request

- Method: GET
- Endpoint: /search?q=`query`

#### Response

- Format: JSON

| Name                           | Type   | Example value          | Description                                                                   |
| ------------------------------ | ------ | ---------------------- | ----------------------------------------------------------------------------- |
| took                           | Number | 1                      |                                                                               |
| hits                           | Object |                        | Array of results                                                              |
| hits.hits                      | Array  |                        | Array of results                                                              |
| hits.hits[n]                   | Object |                        | A result object                                                               |
| hits.hits[n]._id               | String | 4fDdWHYB33znsq_AiAv8   | ID of result, possibly used by search indexer                                 |
| hits.hits[n]._score            | Number | 197.44696              | Relevance of result, possibly used by search indexer                          |
| hits.hits[n]._source           | Object |                        | Search result itself                                                          |
| hits.hits[n]._source.code      | String | AF                     | Country code of place, country or station                                     |
| hits.hits[n]._source.subtitle  | String | Afghanistan            | Country if result is place, place if result is station                        |
| hits.hits[n]._source.placeId   | String | PbqG2Mmi               | Unique ID of place, country or place station is in                            |
| hits.hits[n]._source.title     | String | Ghazni                 | Name of this place, country or station                                        |
| hits.hits[n]._source.type      | String | place                  | Type of this result, either place, country or channel                         |
| hits.hits[0]._source.channelId | String | TJJTy9Oh               | Unique ID of this station, set only if result is a station                    |
| hits.hits[n]._source.url       | String | /visit/ghazni/PbqG2Mmi | Path that can be appended to the hostname, that results in the front-end link |
| query                          | String | Query                  | Supplied search query                                                         |
| version                        | String | 8fc46fcc               | Possibly current Git commit hash of the website                               |
| apiVersion                     | Number | 1                      | API version, 1 is the only one currently known                                |

### Get station staff picks

#### Request

- Method: GET
- Endpoint: /ara/content/static-pages/our-favorites

#### Response

- Format: JSON

| Name                              | Type   | Example value                           | Description                                                                        |
| --------------------------------- | ------ | --------------------------------------- | ---------------------------------------------------------------------------------- |
| apiVersion                        | Number | 1                                       | API version, 1 is the only one currently known                                     |
| version                           | String | 8fc46fcc                                | Possibly current Git commit hash of the website                                    |
| data                              | Object |                                         |                                                                                    |
| data.title                        | String | Search                                  | Title of this static page                                                          |
| data.url                          | String | /search                                 | Front-end link of this static page                                                 |
| data.content                      | Array  |                                         | Array of lists                                                                     |
| data.content[n]                   | Object |                                         | A list object                                                                      |
| data.content[n].type              | String | list                                    | Currently always list                                                              |
| data.content[n].title             | String | Independent Sounds                      | Title of this list                                                                 |
| data.content[n].subtitle          | String | Clashing genres and independent sounds. | Description of this list                                                           |
| data.content[n].itemsType         | String | channel                                 | Type of all items in this list                                                     |
| data.content[n].items             | Array  |                                         | Array of elements in this list                                                     |
| data.content[n].items[n]          | Object |                                         | List element, currently always a station                                           |
| data.content[n].items[n].title    | String | NTS 1                                   | Name of this station                                                               |
| data.content[n].items[n].subtitle | String | London, United Kingdom                  | Subtitle displayed in the front-end, currently always the place this station is in |
| data.content[n].items[n].href     | String | /listen/nts/wT9JJD4j                    | Path that can be appended to the hostname, that results in the front-end link      |

### Get client's location via IP

#### Request

- Method: GET
- Endpoint: /geo

#### Response

- Format: JSON

| Name         | Type   | Example value    | Description                                                         |
| ------------ | ------ | ---------------- | ------------------------------------------------------------------- |
| ip           | String | 185.240.246.172  | Client's IP address                                                 |
| country_code | String | US               | ISO 3166-1 alpha-2 code of country the client's IP is from          |
| country_name | String | United States    | Name of the country the client's IP is from                         |
| region_code  | String | NY               | ISO 3166-2 subdivision code of the area the client's IP is from     |
| region_name  | String | New York         | Name of the area the client's IP is from                            |
| city         | String | New York         | Name of the city the client's IP is from                            |
| zip_code     | String | 10012            | ZIP code of the city the client's IP is from                        |
| time_zone    | String | America/New_York | Time zone of the client, determined via their IP address            |
| latitude     | String | 40.7279          | Latitude of the client's location, determined via their IP address  |
| longitude    | String | 73.9966          | Longitude of the client's location, determined via their IP address |
| metro_code   | String | 501              | Area code if in the United States, 0 otherwise                      |

## Notes

- GET /identify is another endpoint, though it doesn't return any response. Possibly used for analytics.
