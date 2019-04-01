# Machine Data Workshop <img src="https://i.imgur.com/lqo3uaJ.png" width=30 height=30 alt="CrateDB">
Data and instructions for the Machine Data Workshop at [CukenFest London 2019](http://cukenfest.cucumber.io/). Don't forget to check out our [blog](https://crate.io/blog/)!

## Installations
- [CrateDB](https://crate.io/docs/crate/reference/en/0.57/installation.html)
- [Grafana](http://docs.grafana.org/installation/)

## Datasets

### Exercise 1 - Geospatial Machine Data
Create a table in CrateDB for the machine data dataset.

```
CREATE TABLE IF NOT EXISTS "doc"."machine_data" (
   "location" GEO_POINT,
   "model" STRING,
   "objectid" STRING,
   "sensor_type" LONG,
   "sensor_value" LONG,
   "time_stamp" TIMESTAMP
);
```
You can find the dataset in JSON format in `data/machine_data.json`. Add the dataset to CrateDB.
```
cr> COPY machine_data FROM '/Users/tanaypant/Desktop/MachineData-Workshop/data/machine_data.json';
```
Create a table for the dataset on countries and their shape.

```
CREATE TABLE IF NOT EXISTS "doc"."world" (
   "country" STRING,
   "country_shape" GEO_SHAPE INDEX USING GEOHASH
);
```
You can find the dataset in JSON format in `data/world.json`. Add the dataset to CrateDB. Some queries for the dataset.
```
SELECT country, COUNT(country) FROM machine_data, world WHERE WITHIN(location, country_shape) GROUP BY country;
```


### Exercise 2 - 2017 NYC Cabs Data

You can find the dataset [here](https://gist.github.com/kovrus/328ba1b041dfbd89e55967291ba6e074/raw/7818724cb64a5d283db7f815737c9e198a22bee4/nyc-yellow-taxi-2017.tar.gz
). Extract the dataset and add it to CrateDB. Some queries for the dataset.

```
cr> SELECT COUNT(*) FROM TAXI_RIDES WHERE pickup_location_id ='230';
cr> SELECT * FROM taxi_rides a INNER JOIN taxi_rides b ON a._id != b._id AND a.pickup_location_id = b.dropoff_location_id AND a.dropoff_location_id = b.pickup_location_id;
```

## Resources
- [GeoJSON Editor](http://geojson.io/)
- [CrateDB and Grafana](https://crate.io/a/pair-cratedb-with-grafana-an-open-platform-for-time-series-data-visualization/)
- [CrateDB Newsletter](https://crate.io/newsletter/)