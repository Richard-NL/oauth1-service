[![Build Status](https://api.travis-ci.org/simgroep/oauth1-service.png?branch=master)](https://travis-ci.org/simgroep/oauth1-service)

OAuth 1 Service
===============

This library can be added to an application that wishes to validate their clients' requests with OAuth.

## Installation
For now, just clone the repository and include/autoload everything yourself. Once the initial development is done, 
a Composer/Packagist installation will be made possible.

## Usage
There is a runner.php that serves as an example of usage. In the simplest way, use it like this:

```
use Simgroep\Oauth1Service\Service;
use Simgroep\Oauth1Service\Request;
use Simgroep\Oauth1Service\ConsumerProvider;
use Simgroep\Oauth1Service\TokenProvider;

$os = new Service(new Request, new ConsumerProvider, new TokenProvider);
$valid = $os->isValidRequest();

if ($valid === true) {
    print_r($os->getDetails());
} else {
    print_r($os->getError());
}
```

You will need to know two things in your application: is the request valid, and who is sending the request if it's 
actually valid?

The first question is answered by isValidRequest(). It takes a Request object as a parameter, which can be one of 
several supplied classes:
* Request: a plain class that uses $_SERVER vars to determine its values.
* SymfonyRequest: a class that takes \Symfony\Component\HttpFoundation\Request (from Symfony2 or Silex) 
as a parameter and uses that to determine its values.
* Zf1Request: a class that takes \Zend_Controller_Request_Http (from Zend Framework 1) as a parameter and uses that 
to determine its values.

Next, the Service takes two Token Providers. They are classes that you need to implement yourself: they determine 
which consumerTokens and accessTokens are valid. Implement them in any way you like; as long as the classes you 
create implement the TokenProviderInterface, you're fine. Its getSecret() method should be used to take the token, 
look it up somewhere (in a database, through some service, from an array of values, whatever you want) and return 
its secret.

When the request is invalid, you can find out what is wrong by looking at getError().

When the request is valid, you can fetch the required data from getDetails(). Currently, it returns the two tokens 
used in the request.


