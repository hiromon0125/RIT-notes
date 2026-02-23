
## Idea
 We create a lot of buckets of equal size or depth

- Equal Depth
	- Each bucket has the same value
	- calculation for each bucket id should be: $$id=\left\lceil  \frac{position*k}{m}  \right\rceil $$
	- k is the number of buckets you would like
	- m is the total count of datapoints
	- position is the index of item being calculated
- Equal Width
	- Each bucket has the same range
	- calculation for each bucket id should be: $$id=\left\lfloor  \frac{x-x_{min}*k}{width}  \right\rfloor $$
	- k is the number of buckets
	- width is the range of each bucket
	- $x_{min}$ is the minimum x
