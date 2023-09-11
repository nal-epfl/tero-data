The data is json-formatted in utf8 encoding.

Files:
----------
- locations.json          : single json file containing all user locations. 
- raw/latency             : contains the latency digits as extracted from gaming thumbnails after comparing the values from 3 OCR engines, separated by game and month.
- raw/alternative_values  : contains all values extracted from gaming thumbnails if there are at least two different values; i.e. not all points from `latency` have an entry in `alternative values`.
- processed/latency       : contains latency values after cleaning, removing `glitches` and replacing suspect values using `alternative values` if possible.
- processed/glitches      : contains latency values marked as glitches (latency decreases due to image processing errors).
- processed/spikes        : contains latency values marked as spikes (latency increases).
- processed/clusters      : contains json files with the latency clusters per country and game.


locations.json:
----------
-  The json file contains:
    * data          : list of user locations, formatted as follows: {user_id, location: {country, alpha-2 country_code, (region), (city), latitude, longitude}, (temporal index)}. Parameters in brackets are optional.
    * last updated  : unix timestamp indicating the last time the data was modified.
- Region refers to the largest administrative subdivision of the country; i.e. state in the US, province in Canada, and region in France. 
- Locations include the coordinates of their geographical center: these coordinates are not the user coordinates, but a generic point common to all users in the same location. 
- Each user id can be associated with one or more locations. Users with more than one location appear several times on the list, each time with a different location. In these cases, the parameter `temporal index` is used to give partial order to the different locations.
- All location data except the country code is in local language.


raw/latency:
----------
- Each zip file contains a list of json files, one per day of the month (if data is available). 
- Each daily json file contains:
    * data          : list of latency entries per user, formatted as follows: {user_id, game_id, values: [{date as unix timestamp, latency}]}. The user_id corresponds to locations as listed in the `locations.json` file.
    * last updated  : unix timestamp indicating the last time the data was modified.


raw/alternative_values:
----------
- Each zip file contains a list of json files, one per day of the month (if data is available). 
- Each daily json file contains:
    * data          : list of alternative latency values per user, formatted as follows: {user_id, game_id, values: [{date as unix timestamp, list of alternatives}]}. The user_id corresponds to locations as listed in the `locations.json` file.
    * last updated  : unix timestamp indicating the last time the data was modified.


processed/latency:
----------
- Each zip file contains a list of json files, one per day of the month (if data is available). 
- Each daily json file contains:
    * data          : list of latency values per user after running a custom cleaning procedure that removes `glitches` (latency decreases due to image processing errors). The data field is formatted as follows: {user_id, game_id, values: [{date as unix timestamp, latency}]}. The user_id correspond to locations as listed in the `locations.json` file. 
    * last updated  : unix timestamp indicating the last time the data was modified.


processed/glitches:
----------
- Each zip file contains a list of json files, one per day of the month (if data is available). 
- Each daily json file contains:
    * data          : list of latency values discarded from the pool as possible glitches, divided per user, formatted as follows: {user_id, game_id, values: [{date as unix timestamp, latency}]}. 
    * last updated  : unix timestamp indicating the last time the data was modified.
    

processed/spikes:
----------
- Each zip file contains a list of json files, one per day of the month (if data is available). 
- Each daily json file contains:
    * data          : list of latency values marked as spikes, divided per user, formatted as follows: {user_id, game_id, values: [{date as unix timestamp, latency}]}. 
    * last updated  : unix timestamp indicating the last time the data was modified.


processed/clusters:
----------
- Each json contains the clusters for a given game in a country.
- Each daily json file contains:
    * game_id       : code of the game from which latency was extracted.
    * country       : alpha-2 country code.
    * data          : list of clusters per geographical region (administrative subdivision), formatted as follows: {region, min: minimum latency value of the cluster, max: maximum latency value of the cluster, coverage: percentage of users in the region contained in the cluster, n_users: number of users in the cluster}. 
    * last updated  : unix timestamp indicating the last time the data was modified.