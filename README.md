quickhist
=========

This fork is python3 compatible. I built it to visually check changes to an RNG implementation, so it's not very multi-purpose, but it has a couple features that
I might work into PRs for the original:

- aggregate stats: besides percentiles, it's nice to see the mean, min, max, and stdev. I needed to check empirically if out-of-range values were being generated, and that the mean and stdev were near heuristic predictions.
- outlier detection: Assuming a symmetrical unimodal distribution, detects outliers more that 5% out of the expected range based solely on their neighbour in the central direction. There's real math that could be done here instead, but I was just trying to visually locate quantization errors in my math.

-----

**NOTE:** requires `numpy`


Plots the distribution of the input data, and provides some basic stats.

Try:

    for x in $( seq 1 100000 ); do echo $(( ($RANDOM + $RANDOM + $RANDOM) )) ; done  |  quickhist

```

       │                         ╻╻┃┃┃╻┃╻╻╻                         
       │                       ╻┃┃┃┃┃┃┃┃┃┃┃╻╻                       
       │                     ╻┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃╻                     
       │                   ╻┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃                    
 prop. │                 ╻┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃╻                  
       │               ╻┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃╻                
       │              ╻┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃╻              
       │           ╻┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃╻╻           
       │        ╻╻┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃╻╻        
       │   ╻╻╻┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃╻╻    
       └──────────────────────────────────────────────────────────────
       1601.0                                                         97480.0

count      : 100000
25th perc. : 37635.75
50th perc. : 49159.0
75th perc. : 60738.0
```

    for x in $( seq 1 100000 ); do echo $RANDOM ; done  |  quickhist

```
       │                  ┃                                         
       │   ┃ ╻╻╻ ┃ ╻╻┃ ╻ ┃┃ ╻╻╻╻┃  ╻┃ ┃ ┃╻╻╻ ╻╻  ╻╻      ╻  ┃  ╻╻┃╻ 
       │┃┃┃┃ ┃┃┃ ┃┃┃┃┃┃┃┃┃┃╻┃┃┃┃┃╻┃┃┃┃┃╻┃┃┃┃╻┃┃ ┃┃┃┃ ┃ ╻╻┃┃╻┃╻┃┃┃┃┃ 
       │┃┃┃┃╻┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃
 prop. │┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃
       │┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃
       │┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃
       │┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃
       │┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃
       │┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃┃
       └──────────────────────────────────────────────────────────────
       5.0                                                         32765.0

count      : 10000
25th perc. : 8156.5
50th perc. : 16158.5
75th perc. : 24324.0
```
