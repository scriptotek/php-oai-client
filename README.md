[![Build Status](https://img.shields.io/travis/scriptotek/php-oai-client.svg)](https://travis-ci.org/scriptotek/php-oai-client)
[![Coverage Status](https://img.shields.io/coveralls/scriptotek/php-oai-client.svg)](https://coveralls.io/r/scriptotek/php-oai-client?branch=master)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/scriptotek/php-oai-client/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/scriptotek/php-oai-client/?branch=master)

## php-oai-client

Simple OAI package for making OAI-PMH requests, using the 
[Guzzle HTTP client](http://guzzlephp.org/)
and returning 
[QuiteSimpleXMLElement](//github.com/danmichaelo/quitesimplexmlelement) instances. 

### Install using Composer

Add the package to the `require` list of your `composer.json` file.

```json
{
    "require": {
        "scriptotek/oai-client": "dev-master"
    },
}
``` 

and run `composer install` to get the latest version of the package.

### Example

```php
require_once('vendor/autoload.php');
use Scriptotek\Oai\Client as OaiClient;

$url = 'http://oai.bibsys.no/repository';

$client = new OaiClient($url, array(
    'schema' => 'marcxchange',
    'user-agent' => 'MyTool/0.1'
));
```

#### Fetching a single record

```php
$record = $client->record('oai:bibsys.no:biblio:113889372');
if ($record->error) {
    echo $record->errorCode . ' : ' . $record->error . "\n";
    die;
}

echo $record->identifier . "\n";
echo $record->datestamp . "\n";
echo $record->data->asXML() . "\n";
```

#### Iterating over a record set

```php
foreach ($client->records('') as $record) {
	echo $record->identifier . "\n";
	echo $record->datestamp . "\n";
}
```

### Events

```php
$client->on('request.start', function($verb, $args) {
    // ...
});
```

```php
$client->on('request.complete', function($verb, $args, $body) {
    // ...    
});
```

### API documentation 

API documentation can be generated using e.g. [Sami](https://github.com/fabpot/sami),
which is included in the dev requirements of `composer.json`.

    php vendor/bin/sami.php update sami.config.php -v

You can view it at [scriptotek.github.io/php-oai-client](//scriptotek.github.io/php-oai-client/)
