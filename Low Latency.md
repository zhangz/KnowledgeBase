## Measure
#### Service Time
 Time to actually process he request

#### Latency Time
Time spent waiting to be processed

#### Response Time
- Latent time + service time
- The time the consumer perceives

#### Throughput 
The rate at which requests/messages are processed

## Identify your critical path (bottle neck)

## Allocations don't kill performance, Garbage Collections do
Expected GC pauses
- Gen0 - nanoseconds
- Gen1 - 1ms
- Gen2 - 10ms or more

## Serialize, don't synchronize
- Avoid contended resources - use Queue
- Concurrency at system level

## Circular buffers

https://www.youtube.com/watch?v=goICj7G8rHc
 

