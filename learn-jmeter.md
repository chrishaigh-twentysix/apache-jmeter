# Learn JMeter

## CSV Data Set Config

* help inject multiple sets of data to the request

## DNS Cache Manager

* allows testing applications, which have several servers behind load balancers when user receives content from different IPs
* add this to a thread group or test plan
* works only with HTTPClient4 implementation
* can specify DNS servers to use (verify using DEBUG log level)
* can specify static hosts (like hosts file)

## HTML Link Parser

* Modifier which parses HTML response from the server and extracts links and forms.

### Use Cases

### Spidering

* By creating a **HTTP Request** sampler with `.*` as the Path and attaching a child **HTML Link Parser**, random link is followed from `Server Name or IP` location.

### Random Test Data

* By adding **parameters** to a **HTTP Request** sampler with `.*` as the `Value`, request params / test data is randomised.

## HTML URL Re-writing Modifier

* easier to use than the HTML Link Parser and more efficient
* extract values from the response
* can carry over / modify parameters from previous **HTTP Request** response, e.g. `jsessionId`.

## Regular Expression Extractor

* allows the user to extract values from a server response using a Perl-type regular expression

Useful parameters: -

* `Name of created variable:` variable to store extracted data
* `Regular Expression:` expression to extract data
* `Template ($i$ where i is capturing group number, starts at 1):` e.g. `$1$`
* `Match No. (0 for Random):` which capture group to match (use `-1` for all)
* `Default Value:` when no match found

## JSON Extractor

* enables the user to extract data from JSON responses using [JsonPath](https://github.com/json-path/JsonPath) syntax
* must be a child of a **HTTP Sampler**

Useful parameters: -

* `Name of created variable:` variable to store extracted data
* `JSON Path expressions:` expressions from which to extract data
* `Match No. (0 for Random):` which capture group to match (use `-1` for all)
* `Compute concatenation var (suffix _ALL):` check to concatenate all results together into an `xxx_ALL` variable
* `Default Values:` when no match found

Usage example
1. Create an order via API HTTP request
2. Capture order ID using **JSON Extractor**
3. Delete order via API HTTP request using captured order ID

## JSON JMESPath Extractor

* allows the user to extract values from structured responses - XML or (X)HTML - using JMESPath query language

* [**J**SON **M**atching **E**xpre**s**sion **P**aths](https://jmespath.org)
* only one JMESPath expression can be entered at any one time

Useful parameters: -

* `Name of created variable:` variable to store extracted data
* `JMESPath expressions:` expressions from which to extract data
* `Match No. (0 for Random):` which capture group to match (use `-1` for all)
* `Default Values:` when no match found

## CSS Selector Extractor

* allows tht user to extract values from a server HTML response using a CSS Selector syntax.

* Uses CSS/jQuery-based syntax
    * JSoup (default)
    * Jodd-Lagarto (CSSelly)

## Boundary Extractor

* allows the user to extract values from a server response using the left and right boundaries.

## XPath2 Extractor

* allows the user to extract value(s) from a structured response - XML or (X)HTML - using XPath2 query language.

Useful parameters: -

* `Name of created variable:` variable to store extracted data
* `XPath query:` XPath query expression from which to extract data
* `Match No. (0 for Random):` which capture group to match (use `-1` for all)
* `Default Value:` when no match found
* `Namespaces aliases list (prefix=full namespace, 1 per line):` 
* `Return entire XPath fragment instead of text content?` 

## Inter-Thread Communication

* enables communication between thread groups
* FIFO queuing
* this is a JMeter plugin (Inter-Thread Communication)

### Elements

* Inter-Thread Communication PostProcessor
* Inter-Thread Communication PreProcessor

Use these to capture a thread variable from one thread group and refer to it in another.

e.g. capture a response variable from a HTTP request using the PostProcessor, retrieve this for use in another thread group using the PreProcessor.

### Functions

* `fifoPut` - put a value onto the queue
* `fifoGet` - get a value from the queue (don't wait for data)
* `fifoPop` - get and remove a string value from the queue (wait for data)
* `fifoSize` - get the number of items in the queue

Use `Tools -> Function Helper Dialog` to build up function calls, etc.

Use the above functions to interact with queues, which are accessible to every thread group for inter-thread communication.

### Functions vs. Plugins

Functions

* queues not cleared automatically
* queues cleared with the first `fifoPut`
* generates random queue name

Plugins

* clears queues at test start and stop

## Timers

* aim is to mimic a real-world scenario.
* injects delays (think time) into tests.
* in JMeter, timers are processed **before** each sampler in the scope

e.g.

* `Test Plan`
  * `Thread Group`
    * `Request 1`
    * `Request 2`
    * `Uniform Random Timer`
    * `View Results Tree`

Order of execution: -

1. `Uniform Random Timer`
2. `Request 1`
3. `Uniform Random Timer`
4. `Request 2`

### Timers in Config

You can update the timer factor in config by changing this config variable: `timer.factor`

1. Open `jmeter.properties` and copy out the commented out section called `Think Time configuration` into `user.properties`

2. Uncomment the `timer.factor` variable and set this to the desired factor.  All timings will be multiplied by this value

3. e.g. `timer.factor=2.0f` will effectively double all timer delay values.

## Uniform Random Timer

### Useful Parameters

* `Random Delay Maximum (in milliseconds):` maximum delay that can be randomly injected
* `Constant Delay Offset (in milliseconds):` constant delay that is always injected

`Total Delay = Random Delay + Constant Delay Offset`

e.g. if `Random Delay Maximum` is 3000ms and `Constant Delay Offset` is 1000ms, the `Total Delay` will be between 1000ms and 4000ms.

## Throughput

* JMeter tries to keep up the throughput
* Throughput depends on available resources, timers and other elements
* Do not change the throughput frequently
* Shared algorithm is more accurate

## Constant Throughput Timer

* This timer introduces variable pauses, calculated to keep the total throughput (samples per minute) as close as possible to a given figure.

* This throughput will be lower if the server isn't capable of handling it or if other timers or time-consuming test elements prevent it.

* Although the Timer is called a Constant Throughput timer, the throughput value doesn't need to be constant - it can be defined in terms of a variable or function call and its value changed during a test.

* The throughput value can be changes in various ways, including: -

  * using a counter variable
  * using a `__jexl3` or `__groovy` function to provide a changing value
  * using the remote BeanShell server to change a JMeter property

## Precise Throughput Timer

* This timer introduces variable pauses, calculated to keep the total throughput (samples per minute) as close as possible to a given figure.

* This throughput will be lower if the server isn't capable of handling it or if other timers or time-consuming test elements prevent it.

* Although the Timer is called a Precise Throughput Timer, it doesn't aim to produce precisely the same number of samples over 1s intervals during testing.

* The timer works best for rates under 36,000 requests/hour; however, mileage may vary!

### Produced Schedule

* `Precise Throughput Timer` models Poisson arrivals schedule, which often happens in real-life so is sensible for load testing.

* Using this, samples are often naturally generated close together, so can reveal concurrency issues.  This is unlikely `Constant Throughput Timer`, which tends to produce samples at even intervals.

### Ramp-up / Startup Spike

* `Precise Throughput Timer` schedules executions in a random way to generate constant load, so don't need to configure ramp-up.  Instead, set `Ramp-up Period` and `Delay` to `0`.

### Multiple Thread Groups Starting at the Same Time

* When using `Precise Throughput Timer` across multiple threads, there's no need to add a "random" delay to each Thread Group to avoid ramp-up issues because `Precise Throughput Timer` schedules executions in a random way.

### Number of Iterations per Hour

* To satisfy business requirements of `N` samples per `M` minutes, map to these parameters as closely to the BRs as possible.  For example "60 samples per hour": -

  * `Target throughput (samples):` `N`
  * `Throughput period (seconds):` (`M` * 60)
  * `Test duration (seconds):` [*desired testing time, e.g. `M` * 60*]

* The first 2 options set the throughput, so `60/3600` could be `30/1800` or `15/900` or `1/60`, but it's best to match the business requirements as closely as possible for clarity.

* Note: `Test duration (seconds)` does **not** limit test duration but is just used as a hint for the timer.  Configure the actual test in the `Thread Group` settings.

## For more info, see [the documentation](https://jmeter.apache.org/usermanual/component_reference.html#Precise_Throughput_Timer)

