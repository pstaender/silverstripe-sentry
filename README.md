# Logging to Sentry from SilverStripe

First install the module in your SilverStripe 4.x project:

```sh
  $ composer require pstaender/silverstripe-sentry dev-master
```

Activate and setup Sentry logging in the config file of your project. Replace the example dsn `https://…:…@sentry.myserver.com/1` with your Sentry project dsn:

```yaml
---
Name: myproject
---
SilverStripe\Core\Injector\Injector:
  Psr\Log\LoggerInterface: 
    calls:
      LogFileHandler: [ pushHandler, [ '%$SentryLogHandler' ] ]
  RavenClient:
    constructor:
      - https://…:…@sentry.myserver.com/1
```

## Use other formatter

Just override the sentry-logging-config:

```yml
---
Name: myproject
---
SilverStripe\Core\Injector\Injector:
  Psr\Log\LoggerInterface: 
    calls:
      LogFileHandler: [ pushHandler, [ '%$SentryLogHandler' ] ]
  RavenClient:
    constructor:
      - https://…:…@…/18
---
Name: mylogging
After:
  - '#sentry-logging'
---
SilverStripe\Core\Injector\Injector:
  SentryLogHandler:
    properties:
      ContentType: application/json
      Formatter: '%$Monolog\Formatter\JsonFormatter'
```

## Do Everything by injection

You can skip installing this module by installing the sentry-client:

```
  $ composer require sentry/sentry "*"
```

and modifying your project config:

```yml
---
Name: myproject
---
SilverStripe\Core\Injector\Injector:
  Psr\Log\LoggerInterface: 
    calls:
      LogFileHandler: [ pushHandler, [ '%$SentryLogHandler' ] ]
  SentryLogHandler:
    class: Monolog\Handler\RavenHandler
    constructor:
      - '%$RavenClient'
    properties:
      ContentType: text/html
      Formatter: '%$SentryLoggingFormatter'
  SentryLoggingFormatter:
    class: Monolog\Formatter\LineFormatter
    constructor:
      - '%message%'
  RavenClient:
    class: Raven_Client
    constructor:
      - https://…:…@sentry.myserver.com/1
```
