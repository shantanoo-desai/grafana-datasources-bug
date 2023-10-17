# Grafana Datasources Provisioning Bug

> NOTE SOLVED in Grafana v >9.5.1

This repository reproduces a _potential_ bug where upon trying to provision InfluxDBv1 Datasource
the `database` value is not set.

## Reproducing the error

```bash
docker compose up -d
```

### Via Grafana Datasource UI

visit http://localhost:3000/grafana and login with the following credentials:

    admin:komponistTest

Navigate to __Datasources__ and click on _Komponist-InfluxDBv1_ and observe that
`database` value is empty


### Grafana Datasources API

```bash
curl -u admin:komponistTest -XGET http://localhost:3000/grafana/api/datasources | jq "."
```

which should show an empty `database: ""` in the _Komponist-InfluxDBv1_

```json
[
  {
    "id": 1,
    "uid": "P3CF01143EA0AB962",
    "orgId": 1,
    "name": "Komponist-InfluxDBv1",
    "type": "influxdb",
    "typeName": "InfluxDB",
    "typeLogoUrl": "public/app/plugins/datasource/influxdb/img/influxdb_logo.svg",
    "access": "proxy",
    "url": "http://komponist_influxdbv1:8086",
    "user": "rw_user",
    "database": "",
    "basicAuth": false,
    "isDefault": false,
    "jsonData": {
      "httpMode": "GET"
    },
    "readOnly": false
  },
  {
    "id": 2,
    "uid": "P16F52D15435F17DA",
    "orgId": 1,
    "name": "Komponist-InfluxDBv2",
    "type": "influxdb",
    "typeName": "InfluxDB",
    "typeLogoUrl": "public/app/plugins/datasource/influxdb/img/influxdb_logo.svg",
    "access": "proxy",
    "url": "http://komponist_influxdbv2:8087",
    "user": "",
    "database": "",
    "basicAuth": false,
    "isDefault": false,
    "jsonData": {
      "defaultBucket": "komponistdb",
      "organization": "komponistorg",
      "tlsSkipVerify": true,
      "version": "Flux"
    },
    "readOnly": false
  }
]
```

Many times it is set and often the value stays empty.

### Result Video

[231259336-56238a61-65dc-4d40-ba5b-7d5bee956d7a.webm](https://user-images.githubusercontent.com/12070966/231376681-a9d43aa4-3e1b-4d76-a74e-76649f578503.webm)

### Reporting

Potential Bug Report reported at https://github.com/grafana/grafana/issues/66316

## Error Matrix

| Grafana Version | `database` Set |
|:---------------:|:--------------:|
| `9.4.x`         |  __NO__        |
| `9.3.x`         |  __YES__       |
| `9.2.x`         |  __YES__       |           

## Environment


### Docker Version / Docker Compose Version

__Docker Version__: _23.0.1_

__Docker Compose Version__: _2.17.2_

__Grafana Version__: `grafana/grafana-oss:9.4.7`

