Apigility Skeleton Application
==============================

Installation
------------

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

