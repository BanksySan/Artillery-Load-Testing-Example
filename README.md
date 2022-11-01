# Artillery load testing example.

## Files

| Path                       | Decscription                                                                                    |
|----------------------------|-------------------------------------------------------------------------------------------------|
| `test-definition.yaml`     | Defines the tests to carry out on execution.                                                    |
| `mock-api-definition.json` | [Mockoon](https://mockoon.com/docs/latest/about/) environment definition to use with the tests. |

## Introduction

* When Artillery refers to a _script_, it is referring to the YAML definition file, not to any associated JavaScript files.
* A reasonable familiarity with YAML is needed to avoid chasing frustrating bugs around the script structure.

## Outstanding Questions

* How can we target multiple domains?
  * Yes.  It's a bit ugly.  Use a target for one of the domains, but where you need other domains you use a fully qualified URL.

## Script FIle

### Minimal Script

```yaml
config:
  target: "http://127.0.0.1:3001/api"
  phases:
  - duration: 1
    arrivalRate: 1
scenarios:
  - flow:
    - get:
        url: "/echo/hello-world"
```

The script itself is split into to sections, the `config` and the `scenarios` section.


### `config`

This section contains instructions around how the test should run.  This includes properties that define, for example:

* How many phases, and what in what order, should each test case have.
* What the rate of traffic, and the rate of change of traffic should be.
* How long should each phase last?
* Declare any plugins.
* Identify any JavaScript files that contain extension functions.
* TLS
* Define data sources for data-drived scenarios.

### `scenarios`

This section defines individual tests.  Whilst Artillery supports several web protocols (Socket.io and WebSockets) we'll only be using HTTP.

* What requests should be sent?
* How should requests be created?
  * Data extraction and reuse.
* What extensions should be called and when?
* How should users be spread over scenarios?

## Extensions

These are added as npm packages and are included in a test definition by adding a reference to it in the `config` section.

To add the _Metrics by Endpoint_ plugin:

```bash
npm install artillery-plugin-metrics-by-endpoint;
```

```yaml
config:
    target: "https://develop.lieferando.de"
    plugins:
      metrics-by-endpoint: {}
```

> Notice that we strip the `artillery-plugin-` prefix.

The _Metrics by Endpoint_ plugin doesn't require options, if it did then these would be passed by populating the object.
