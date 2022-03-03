# Prometheus

## General

A metric name is a label (`__name__`)
<details><summary>Example:</summary>
<p>

```
my_metric{
    job="scrape-job",
    env="dev",
}
```
is the same thing like:
```
{
    __name__="my_metric",
    job="scrape-job",
    env="dev",
}
```
</p>
</details>



## Labels

Labels comes from 2 sources:
* **instrumentation** labels (from the application)
* **target** labels (during scraping)


### Hidden labels

Start with `__` is for internal use:
* `__meta`: added by Service Discovery
* `__tmp`: can be used by users



## PromQL 

Show all series
```
{__name__!=""}
```

Getting all metric names
```
group by(__name__) ({__name__!=""})
```

Number of series per metric name
```
sort_desc(count by(__name__) ({__name__!=""}))
```

Total number of series
```
count({__name__!=""})
```

Number of series per target/instance
```
sort_desc(count by(instance) ({__name__!=""}))
```

Number of series per job
```
sort_desc(count by(job) ({__name__!=""}))
```