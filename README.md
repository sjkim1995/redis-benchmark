# StackExchange.Redis Benchmark Tool
Console app for benchmarking StackExchange.Redis client performance. Outputs latency, throughput, memory, and CPU statistics to a CSV file.

The app takes in a number of ops ```X``` to execute in parallel and a duration ```Y``` for execution. It calls ```db.StringGetAsync``` ```X``` times in parallel on a dummy key inserted into the cache at the start of the trial  for ```Y``` seconds. When this finished, the app calculates and writes the average latency, median latency, average throughput, and CPU and memory usage through time to a CSV file.

# Usage :
Build (most recent build already pushed) and navigate to `\bin\Release` from the root directory. Run: 
```
./SE.Redis-Benchmark.exe <hostname> <password> <parallelOps> <timePerTrialInSeconds> <numberOfTrials> <outputFileName>
```
So, for example, if I wanted to run three 15-min trials on one of my caches hosted on Azure cloud at 50k ops, I'd run:

```
./SE.Redis-Benchmark.exe someAzureCacheName.redis.cache.windows.net ########### 50000 900 3 TestResults50k.csv
```

The output of this command is wrriten to TestResults50k.csv, the ```<outputFileName>``` supplied as the final argument. Note that you don't need to create this file before running the benchmark beforehand, but if you do, be aware that the contents of the file will be overwritten by running Redis-Benchmark.exe.

Note that there aren't default values for any of these parameters, so you faiking to supply a param will result in an Exception being thrown.

# Best Practices :
If you are benchmarking an Azure Redis cache, I recommend running this on a client VM in the same region as the one your cache is in. This reduces network noise and other biases that would be introduced by running Redis-Benchmark on your local machine. 
You can read more about best practices and optimal settings/parameters for benchmarking Azure caches [here](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#performance-testing).
