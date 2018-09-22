# Logging to Sentry from SilverStripe

```sh
  $ composer require pstaender/silverstripe-sentry '*'
```

Add the dsn to your config file and your application is sentry ready:

```yaml
---
Name: myproject
---
SilverStripe\Core\Injector\Injector:
  RavenClient:
    constructor:
      - https://…:…@sentry.myserver.com/1
```