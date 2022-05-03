# Data Model and write path

## Data model and terminology

Influxdb database stores *points*. A point has four components: a measurement, a tagset, a fieldset, a timestamp.
- measurement provide a way to associate related points that might have different tagsets or fieldsets.
- The tagset is a dictionary or key-value pairs to store metadata
- The fieldset is a set of typed scalar values - the data being recorded by the point.

The format for points is defiend by the line protocol. Eg:\
```temperature,machine=unit42,type=assembly internal=32,external=100 1434055562000000035```

- **measurement** is *temperature*
- **tagset** is *machine=unit42,type=assembly*
- **fieldset** is *internal=32,external=100*

Each point is store in 1 *retention policy* within 1 *database*. *Database* is container for user, rp, and points. A RP configures how long Influxdb keeps points (duration), how many copies of points are stores in cluster (replication factor), and the time range covered by shard groups (shard group duration).

## Receiving points from clients

Clients POST points (in line protocol format) to Influxdb's HTTP */write* endpoint. Points can be sent individually; however, for efficiency, most applications send points in batches, around hundreds to thousands points.

The POST request specifics database, optional RP via query parameters. If the RP is not specificed, the default RP is used.

When the database receives new points, it must make those points durable so that they can be recovered is case of a db or server crash and make the points queryable. This note focuses on the first half, making those points durable.

## Persistent Points to Storage

To make points durable, each batch is written and **fsynced** to write ahead log (WAL). The WAL is append only file that is only read during a database recovery.

WAL 

## Ref

- https://www.influxdata.com/blog/influxdb-internals-101-part-one/