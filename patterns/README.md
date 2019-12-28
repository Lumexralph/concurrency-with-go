# Concurrency Patterns

Ways to compose the Go concurrency primitives into patterns that will help keep your system scalable and maintainable.

## Confinement

It is the idea of ensuring information is ever available from one concurrent process. When this is achieved, a concurrent program is implicitly safe and no synchronization is needed.

They are of 2 types Ad hoc and lexical.

## The `for-select` Loop

It is very common in Go code, it is something like this;

        for { // Either loop infinitely or range over something
            select {
            // Do some work with channels
            case <- done:
                return
            default:
            // do something else
            }
        }

## Preventing Goroutine Leaks

Goroutines are not garbage collected by the runtime, so we don't want to leave them lying about our process. We have to clean them up.

How?

Establish a signal(channel) between parent goroutine and its children, that allows parent to signal cancellation to its children. Signal is read-only channel called `done` by convention. The parent goroutine passes this channel to the child goroutine and the closes the channel when it wants to cancel the child goroutine.