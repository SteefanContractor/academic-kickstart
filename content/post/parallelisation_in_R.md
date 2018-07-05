+++
title =  "Parallelisation in R"
date =  2017-11-23T16:51:46+11:00
draft = true 
highlight = true
highlight_languages = ["r"]
tags = []
+++

I recently decided to take advantage of running some R code on multiple cores. Before this I had not had much experience in parallel programming so did not know what to expect but in hindsight I think I learnt a lot through this experience.

The code I wanted to parallelise, calculates the relative change in 301 quantiles and their confidence intervals (using bootstrapping) of daily rainfall at each grid cell. This meant looping over 360 times 180 grids and calculating 301 quantiles a thousand times. 

The plan was to try out parallelelisation by comparing run times of code that has two nested loops for the longitude and latitude and performing a tiny calculation inside. So the code I came up with initiall was the following. 

{{< highlight R >}}
fun <- function(x, y) {
    return(10*x + y)
}

sequential <- function(iteration) {
  A <- array(dim = c(ndim, ndim, 101))
  B <- array(dim = c(ndim, ndim, 101))
  for (i in 1:ndim) {
    for (j in 1:ndim) {
      A[i,j,] <- fun(i, j)
      B[i,j,] <- fun(j ,i)
    }
  }
  out <- list(A, B)
  return(out)
}
{{< / highlight >}}

To turn this into parallel code you first have to create a node. We first start with creating a node with one less than the number of available cores.


{{< highlight R >}}
fun <- function(x, y) {
num.cores <- detectCores() - 1

cl <- makeCluster(num.cores)
registerDoSNOW(cl)
{{< / highlight  >}}
```r
fun <- function(x, y) {
num.cores <- detectCores() - 1

cl <- makeCluster(num.cores)
registerDoSNOW(cl)
```
There are four different ways to parallelise this code. 

 - replace both the nested for loops with foreach functions from the foreach package
 - replace only the outer loop wih the parallel foreach
 - 

There are effectively two ways of making loops use multiple cores in R. One is to replace the for loop with a foreach loop with a %dopar% construct, or you could convert your loop into one of the apply family of functions, and then replace the apply/lapply/sapply with a parApply/parLappyly/parSapply. Personally I like to write immutable code, so generally tend to avoid using for loops which use side-effects. As such my preferred way of writing parallel code is to use the parApply family of functions. 

When converting nested for loops into parallel code, one can either conver the inner loop or the outer loop into foreach with %dopar%. This raises the added complication of which is faster. Generally if the loop has fewer iterations than the number of processors to be used, then that loop is not suitable for parallelising. The foreach function comes with another construct for nesting two foreach fuctions. This is the %:% construct. If the outer loop iterates over m things and the inner loop iterates over n things, when using the %:% construct the jobs are split into $m \times n$ jobs instead of just m or n jobs. 

Memory management is something else that is very important to consider when writing parallel code. Generally reading and writing to memory takes a lot longer than a computational step. When a loop is split into multiple jobs, the same objects are either copied multiple times for the 
