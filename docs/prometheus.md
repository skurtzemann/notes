# Prometheus

## General

A metric name is a label (`__name__`):

    ```
    my_metric{
        job="scrape-job",
        env="dev",
    }

    {
        __name__="my_metric",
        job="scrape-job",
        env="dev",
    }
    ```

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