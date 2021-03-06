# k-metar-taf

[![Latest Release](https://img.shields.io/github/v/tag/kalisio/k-metar-taf?sort=semver&label=latest)](https://github.com/kalisio/k-metar-taf/releases)
[![Build Status](https://app.travis-ci.com/kalisio/k-metar-taf.svg?branch=master)](https://app.travis-ci.com/kalisio/k-metar-taf)

A [Krawler](https://kalisio.github.io/krawler/) based service to download [METAR](https://en.wikipedia.org/wiki/METAR) and [TAF](https://en.wikipedia.org/wiki/Terminal_aerodrome_forecast) data from the [Aviation Weather Center](https://www.aviationweather.gov/).

## Description

The **k-metar-taf** job allow to scrape **METAR** and **TAF** data using the [web services](https://www.aviationweather.gov/help/webservice) provided by the **Avioation Weather Center**. 

The downloaded data are stored within a [MongoDB](https://www.mongodb.com/) database and more precisely in 3 collections:
* `metar-taf-observations` that stores the **METAR** data
* `metar-taf-forecasts` that stores the **TAF** data
* `metar-taf-stations` that stores the stations data

All records are stored in [GeoJson](https://fr.wikipedia.org/wiki/GeoJSON) format.

The job is executed according a specific cron expression. By default, every hours.

## Implementation

Because webservices return a limited amount of data based on the query bounding box, the job consists of a set of tasks which query the webservice according a given bounding box.
The following illustrates the different bounding boxes used to defined the set of tasks to be executed:

![bboxes](./bboxes.jpg)

The subdivision is irregular in order to define smaller bounding boxes on areas of large amount of aerodromes (.e.g. north america and western europe).

## Configuration

| Variable | Description |
|--- | --- |
| `DB_URL` | The mongoDB database URL. The default value is `mongodb://127.0.0.1:27017/metar-taf` |
| `TTL` | The data time to live. It must be expressed in seconds and the default value is `604 800` (7 days) |
| `DATA` | The data to be scrapped. It must be either `metar` or `taf`. The default value is `metar` |
| `FOOTPRINT` | Writes the footprint of the different queries as a **GeoJSON** file. The default value is `false`. |
| `DEBUG` | Enables debug output. Set it to `krawler*` to enable full output. By default it is undefined. |

## Deployment

We personally use [Kargo](https://kalisio.github.io/kargo/) to deploy the service.

## Contributing

Please refer to [contribution section](./CONTRIBUTING.md) for more details.

## Authors

This project is sponsored by 

![Kalisio](https://s3.eu-central-1.amazonaws.com/kalisioscope/kalisio/kalisio-logo-black-256x84.png)

## License

This project is licensed under the MIT License - see the [license file](./LICENSE) for details
