# gofence

Tool for geofencing with different algorithms for profiling

```
NAME:
   fence - Fence geojson features from stdin

USAGE:
   fence [global options] command [command options] fence_file
   
VERSION:
   0.0.0
   
COMMANDS:
   help, h	Shows a list of commands or help for one command
   
GLOBAL OPTIONS:
   --fence "rtree"		Type of fence to use rtree|brute|qtree|qrtree|city|city-bbox|bbox
   --concurrency, -c "4"	Concurrency factor, defaults to number of cores
   --zoom, -z "18"		Some fences require a zoom level
   --help, -h			show help
   --version, -v		print the version
   --debug			generate profiling data
```

The city algorithms are special cases and both require NYC_BOROS_PATH envvar to be set to a geojson file

The boros and tracts data can be found on [NYC Open Data Maps](http://www1.nyc.gov/site/planning/data-maps/open-data/districts-download-metadata.page)

## Benchmarks

| Benchmark           | Operations | Time (ns/op) | Bytes (b/op) | Mallocs (allocs/op) | 
|---------------------|-----------:|-------------:|-------------:|--------------------:| 
| BenchmarkBrute-4    |       5000 |       251641 |           19 |                   1 | 
| BenchmarkCity-4     |      30000 |        39913 |           40 |                   1 | 
| BenchmarkBbox-4     |      50000 |        37085 |           11 |                   1 | 
| BenchmarkCityBbox-4 |     200000 |         9484 |           13 |                   1 | 
| BenchmarkQfence-4   |     300000 |         3959 |          399 |                  18 | 
| BenchmarkRfence-4   |    1000000 |         2290 |          174 |                   9 | 

![chart link broken](https://docs.google.com/spreadsheets/d/1PYoxb7nhPA_zrh9oPFnUH0mvo8geYvEkjfe8Jtc0vvY/pubchart?oid=1486005290&format=image)

Benchmarking requires NYC_TRACTS_PATH envvar to be set. Benchmarks are ran by checking which census tract a point is in [code here](lib/fence_test.go)