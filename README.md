# Apigility Skeleton Application

## Installation

### Via Composer (create-project)

```bash
$ composer create-project -sdev zfcampus/zf-apigility-skeleton path/to/install
```

### Dev Mode (if needed)

Once you have the basic installation, you need to put it in development mode:

```bash
$ composer development-enable
```
### Update

```bash
$ composer update
```

### Launch

```bash
$ php -S 0.0.0.0:8080 -ddisplay_errors=0 -t public public/index.php
# OR use the composer alias:
$ composer serve
```

## Getting started

[Getting started](https://apigility.org/documentation/intro/getting-started)

### Create an API

### Create a RPC service

New Service -> RPC -> define name and route (here **Ping** and `/ping`)

Add a field : Fields -> Name : **ack** -> Description : **Acknowledge the request with a timestamp**

In `module/Status/src/V1/Rpc/Ping/PingController.php` : 

```php
<?php
namespace Status\V1\Rpc\Ping;

use Zend\Mvc\Controller\AbstractActionController;
use ZF\ContentNegotiation\ViewModel;

class PingController extends AbstractActionController
{
    public function pingAction()
    {
        return new ViewModel([
            'ack' => time()
        ]);
    }
}
```