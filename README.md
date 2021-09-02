2020-08-30 performance testing
==============================

Performance testing measures many things
- response time
- throughput
- reliability
- scalability

Should do performance testing throughout a project rather than at the end

Should define performance requirements upfront

Performance testing process: -
- design & build tests
- prepare the test environment
- run the tests
- analyse the results
- optimise
- retest

Load can be generated in terms of
- number of users
- number of requests

Performance test types
- Smoke tests
- Load tests
- stress tests
- spike tests
- endurance tests

JMeter recommended requirements

hardware

- CPU: 4 or more cores
- memory: 16GB RAM
- network: gigabit ethernet

Software requirements

- Java - any OS with a JVM implementation
- use JDK (openjdk) where possible
- set JAVA_HOME environment variable

Plugins

- download from https://jmeter-plugins.org/
- copy to %JMETER_HOME%\lib\ext to install
- use jmeter-plugins-manager to easily install plugins

recommended plugins: -

1. Custom Thread Groups
2. 3 Basic Graphs
3. Throughput Shaping Timer
4. Dummy Sampler

JMeter components
=================

Test Plan 
- overall element of a test
- can specify thread settings and user defined variables

Thread Group
- entry point of a test
- control number of threads/users used to execute tests

Configuration Elements
- set up default configuration and variables for later use
- can place under any element
- types
	1. define variables (e.g. CSV Data Set Config)
	2. define configuration (e.g. Keystore Configuration)
	3. managers ( e.g. HTTP Header Manager)
	4. full configuration managers (e.g. request defaults)

Controllers
- Logic Controllers - control conditional execution
- Samplers - perform a request, generating one more results

Timers
- introduce delay between requests
- only processed in conjunction with a sampler

Assertions
- validate if a response is as expected

Listeners
- listen to responses and aggregate results
- have access to the same data but present it differently
- can use a lot of memory

Execution order
===============

1. Configuration Elements
2. Pre-processors
3. Timers
4. Logic Controllers / Samplers
5. Post-processors
6. Assertions
7. Listeners

Hierarchical execution takes precedence over ordered execution

PRO-TIP - create an element in JMeter, select, then go to Help -> What's this node? for contextual information

Running a test

- via Jmeter GUI
- via command line: -

(use jmeter --help for usage examples and jmeter --? for command-line options)

jmeter --nongui --testfile Example.jmx

Recording a test
================

Need to configure a proxy for JMeter recording
Firefox is the recommended browser for this

Firefox
- about:preferences
- Network Settings
- Manual proxy configuration -> HTTP Proxy: <ip of local machine>	Port: 8888	[x] Use this proxy server for all protocols

- NB: localhost proxies are now disabled in Firefox (unless overridden, which is not recommended!)

Chrome
- install BlazeMeter Chrome Extension

HTTPS proxying requires installing JMeter's SSL certificate into the browser

Dynamic Tests
=============

Can drive tests using CSV data files (CSV Data Set Config)

Pre-processors
- run before samplers and modify requests
- e.g. create variables which can be reused during tests

Post-processors
- run after samplers and process server responses
- e.g. extract values from test outputs, e.g CSS select an HTML attribute from HTML output

Functions
- syntax: ${__functionName(par1, par2, ...)}
- Tools -> Function Helper Dialog

Test fragments
- elements which hold other elements inside
- e.g. Module controller, Include controller

Collecting + Analysing Test Results
===================================

Important Application Performance Metrics
- Response time
- Throughput
- Error rate

Resource utilisation
- CPU
- Memory
- Network bandwidth

Use Listeners
- View Results Tree - good for detail and filtering
- Aggregate Report - great for summarising and data science

Monitoring Server Metrics
- Perfmon plugin - needs perform agent installing on server

JMeter Report Dashboard
- recommended to disable all listeners
- recommended to disable parent samplers
- run at the command line: -

```powershell
jmeter `
	--nongui `
	--testfile ".\recorded-test.jmx" `
	--logfile ".\results.csv" `
	--reportatendofloadtests `
	--reportoutputfolder ".\output\"
```

The report

APDEX (Application Performance Index)
- measures users' satisfaction, taking into account the response time of the application
