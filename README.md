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

## Creating a REST Service

### Preparations

[Creating a REST Service](https://apigility.org/documentation/intro/first-rest-service)

Install and configure the `zfcampus/statuslib-example` module in order to perform this tutorial :

```bash
composer require zfcampus/statuslib-example
```

Inject it into `config/modules.config.php`.

Create a PHP file `data/statuslib.php` :

```php
<?php
return [];
```

Edit the file `config/autoload/local.php` :

```php
return [
    /* ... */
    'statuslib' => [
        'array_mapper_path' => 'data/statuslib.php',
    ],
];
```
Create a password file,  `data/.htpasswd` :

(login : test, password : test)

```
test:{SHA}qUqP5cyxm6YcTAhz05Hph5gvu9M=
```

### Terminology

<dl>
<dt>Entity</dt>
<dd>An addressable item being returned. Entities are distinguished by a unique identifier present in the URI.</dd>
<dt>Collection</dt>
<dd>A addressable set of entities. Typically, all entities contained in the collection are of the same type, and share the same base URI as the collection.</dd>
<dt>Resource</dt>
<dd>An object that receives the incoming request data, determines whether a collection or entity was identified in the URI, and determines what operation to perform.</dd> 
<dt>Relational Links</dt>
<dd>A URI to a resource that has the described relation. Relational links allow you to describe relations between different entities and collections, as well as directly link to them so that the web service client can perform operations on those relations. These are also sometimes called hypermedia links. </dd>
</dl>

REST services return entities and collections, and provide hypermedia links between related entities and collections. Resource objects coordinate operations, and return entities and collections

### Create a REST service

New service -> Name (**Status** for us)

Go to the `Status` Rest service page.

Change the "Hydrator Service Name" from `Zend\Hydrator\ArraySerializableHydrator` to `Zend\Hydrator\ObjectPropertyHydrator`, in order to work with the statuslib example.

The StatusLib library provides its own Entity and Collection classes. Edit "Entity Class" for `StatusLib\Entity` and "Collection Class" for `StatusLib\Collection`. Save.

### Define fields for the service


* `message` - a status message. Non-empty, less than 140 characters.
* `user` - the user providing the status message. Non-empty, fulfill a regex.
* `timestamp` - an integer timestamp. Optional, must consist of only digits.

Add those fields.

We'll also have an `id` field, just for display. No need to add it.

Create filters and validators for the three fields.

### Documentation

Give descriptions for Service, Collections and Entities

### Authentication and Authorization

To configure authentication, we need to create a new authentication adapter. Select the "Authentication" menu item to bring up the authentication page. Here we make a basic one :

* "Adapter Name" : "status"
* "Type" : "HTTP Basic" 
* "Realm" : "api"
* "htpasswd Location" : "data/.htpasswd"

Set the authentication type of the API to "status (basic)"

Choose which HTTP methods of which service will be under authentication, and that's authorization.

### Defining the resource

In `module/Status/src/V1/Rest/Status/StatusResource.php` :

```php
use StatusLib\MapperInterface;
```

Add a protected property $mapper :

```php
class StatusResource extends AbstractResourceListener
{
    protected $mapper;
```

Create a constructor that accepts a `MapperInterface`, and assigns it to our `$mapper` property :

```php
protected $mapper;

public function __construct(MapperInterface $mapper)
{
    $this->mapper = $mapper;
}
```

* Replace the body of the create() method with `return $this->mapper->create($data);`.
* Replace the body of the delete() method with `return $this->mapper->delete($id);`.
* Replace the body of the fetch() method with `return $this->mapper->fetch($id);`.
* Replace the body of the fetchAll() method with `return $this->mapper->fetchAll();`.
* Replace the body of the patch() method with `return $this->mapper->update($id, $data);`.
* Replace the body of the update() method with `return $this->mapper->update($id, $data);`.

