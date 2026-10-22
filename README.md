# GeoFabric public demo data

Versioned, immutable demo data for the [GeoFabric](https://github.com/shadowhunter67/geofabric) static demo
(https://shadowhunter67.github.io/geofabric/). One window of central Ho Chi Minh City ([106.66, 10.74, 106.74, 10.81], OGC:CRS84) cut from
Overture Maps release 2026-09-23.1, as plain GeoParquet.

## Licence

**ODbL 1.0** (see `LICENSE`). The files are a derived database of OpenStreetMap-derived Overture data; they are offered under the ODbL with the
attribution in `ATTRIBUTION.md`. The GeoFabric *code* licence (MIT OR Apache-2.0) does not apply to this data. The places layer keeps its per-record
source licences. If you use or redistribute these files you must give the attribution and offer any derived database under the ODbL.

## Layout

```
v1/                 an immutable data version (never edited; changed data becomes v2/)
  demo_*.parquet.zip   plain GeoParquet; the .zip suffix only keeps CDNs from transfer-compressing the file (which would break HTTP byte ranges)
  SHA256SUMS           sha256sum -c compatible
  MANIFEST.json        layers, rows, CRS, licence, source release, checksums
```

## Using it

Served by jsDelivr at commit-pinned URLs (immutable, CORS, HTTP range reads):
`https://gcore.jsdelivr.net/gh/shadowhunter67/geofabric-demo-data@<commit>/v1/demo_places.parquet.zip`. Use the `gcore.jsdelivr.net` host: the
generic `cdn.jsdelivr.net` name sometimes lands on a backend that answers a ranged HEAD with 200 instead of 206, which DuckDB-Wasm cannot use.
The GeoFabric repository's `data/public-demo/pack.json` is the source of truth for which commit/URLs the demo uses.

## How a version is produced

`data/scripts/publish_demo_data.py` in the GeoFabric repository: generate -> validate (sizes, SHA-256, Parquet, schema/rows read back by the Rust
GeoParquet reader) -> checksums -> one commit + tag per version -> verify range/CORS on the pinned URLs -> write the manifest.
