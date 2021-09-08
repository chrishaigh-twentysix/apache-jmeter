# Software Performance & Performance Testing

See: https://www.youtube.com/watch?v=7koEc8iX7AM

## Performance Testing

1. Throughput - rate at which software can process information.  Maximum throughput is bandwidth

2. Latency - interval between initiation of an action and its result

These measurements can be polar opposites but can come together to determine *responsiveness*

Useful technical measures for measuring performance.

* Precision and reproducability is a problem
* Need to control the variables
* Need to worry about the performance of the performance tests
* Any form of test needs to run in controlled circumstances
* Variants must be eliminated for meaningful results
* Establish pass/fail performance tests
* End-to-end tests are problematic - results are difficult to understand due to complexities
* In an ideal world, the same tests ran multiple times should produce the same results

## Solutions

* Infra as Code
* Dedicated test environments including networking
* Locked down OS configuration with minimal housekeeping, etc.
* Record worst hour in Production and multiply this which performance testing

* Create threshold-based tests and then tune the thresholds to allow for variance
* Reduce the scope i.e. complexity of what's being measured
* Measure performance-critical pieces individually where possible
* Test mostly components to control variants
* Also test some whole system tests

## Response Times for Usability

How users pereceive the quality of your system

* Excellent: 0 to 150ms
* Good: 150 to 300ms
* Poor: 300 to 450ms
* Unacceptable: > 450ms

## Other Types of Performance Test

* Load tests
* Stress tests
* Scalability tests (run weekly over the weekend?)

Whole system will allow testing of the unexpected - use results for scaling out hardware, etc.