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