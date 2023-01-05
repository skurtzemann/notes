# Prometheus

## General

### Time series

#### Data

Time series data are: 
  * a sequence of numerical data points 
  * indexed chronologically from the same source

In the scope of Prometheus, these data points are collected at a fixed time interval

An abstract example
```
timestamp=1544978108, company=ACME, location=headquarters, beverage=coffee, value=40172
```

#### Databases

Modern time series databases store the following components:
* a **timestamp**
* a **value**
* some **context about the value**, encoded in a metric name or in associated key/value pairs


### Metric

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
<!-- TODO: example -->
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

Top 10 cardinality of metric
```
topk(
    10,
    count({__name__!=""}) by (__name__)
)
```

Total number of metric
```
count(group by(__name__) ({__name__!=""}))
```

Total number of series
```
count({__name__!=""})
```

Number of series per instance
```
sort_desc(count by(instance) ({__name__!=""}))
```

Number of series per job
```
sort_desc(count by(job) ({__name__!=""}))
```