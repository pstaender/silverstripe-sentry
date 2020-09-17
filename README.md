# Logging to Sentry from SilverStripe

First install the module in your SilverStripe 4.x project:

```sh
  $ composer require pstaender/silverstripe-sentry dev-master
```

Activate and setup Sentry logging in the config file of your project. Replace the example dsn `https://…:…@sentry.myserver.com/1` with your Sentry project dsn:

```yaml
---
Name: my-sentry-logging
Only:
  environment: "live"
---
SilverStripe\Core\Injector\Injector:
  Psr\Log\LoggerInterface:
    calls:
      LogFileHandler: [ pushHandler, [ '%$SentryLogHandler' ] ]
  RavenClient:
    constructor:
      - '`SENTRY_DSN_URL`'
```

That's it - the exceptions will now be monitored by sentry.

## Use other formatter

Just override the sentry-logging-config:

```yml
---
Name: sentry-logging
Only:
  environment: "live"
---
SilverStripe\Core\Injector\Injector:
  Psr\Log\LoggerInterface:
    calls:
      LogFileHandler: [ pushHandler, [ '%$SentryLogHandler' ] ]
  RavenClient:
    constructor:
      - https://…:…@…/18
---
Name: my-sentry-logging
After:
  - '#sentry-logging'
---
SilverStripe\Core\Injector\Injector:
  SentryLogHandler:
    properties:
      ContentType: application/json
      Formatter: '%$Monolog\Formatter\JsonFormatter'
```

## DIY Sentry Handling

You can user sentry also without this module. Just install the official sentry-client (`composer require sentry/sentry`) and add toy your projects' config:

```yml
---
Name: myproject
---
SilverStripe\Core\Injector\Injector:
  Psr\Log\LoggerInterface:
    calls:
      LogFileHandler: [ pushHandler, [ '%$SentryLogHandler' ] ]
  SentryLogHandler:
    class: Sentry\Monolog\Handler
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
