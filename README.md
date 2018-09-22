# Logging to Sentry from SilverStripe

First install the module in your SilverStripe 4.x project:

```sh
  $ composer require pstaender/silverstripe-sentry '*'
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