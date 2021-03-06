# Upgrade from 0.5 to 0.6

## Changes

* When exchanging the code for an access token we are now using the post body instead of query parameters
* Better error handling when exchange from code to access token fails

## BC breaks

There are a few minor BC breaks. We removed the functions below: 

* `LinkedIn::getUserId`, use `LinkedIn::getUser` instead
* `AccessToken::constructFromJson`, Use the constructor instead. 

# Upgrade from 0.4 to 0.5

## Changed signature of `LinkedIn::api`

The signature of `LinkedIn::api` has changed to be more easy to work with. 
```php
// Version 0.4
public function api($resource, array $urlParams=array(), $method='GET', $postParams=array())

// Version 0.5
public function api($method, $resource, array $options=array())
```

This means that you have to modify your calls to: 
```php
// Version 0.5
$options = array('query'=>$urlParams, 'body'=>$postParams);
$linkedIn->api('POST', $resource, $options)
```
See the Readme about more options to the API function. 

## Must inject IlluminateSessionStorage
We have removed the protected `LinkedIn::init` function. That means if you were using `IlluminateSessionStorage` you have
to make a minor adjustment to your code. 

```php
// Version 0.4
$linkedIn=new Happyr\LinkedIn\LinkedIn('app_id', 'app_secret');

// Version 0.5
$linkedIn=new Happyr\LinkedIn\LinkedIn('app_id', 'app_secret');
$linkedIn->setStorage(new IlluminateSessionStorage());
```

If you don't know about `IlluminateSessionStorage` you are probably good ignoring this. 

## Default format 

The default format when communicating with LinkedIn API is changed to  json. 

## Updated RequestInterface

The `RequestInterface::send` was updated with a new signature. We did also introduce `RequestInterface::getHeadersFromLastResponse`. 
