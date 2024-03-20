# Captcha for Laravel 5/6/7/8/9

[![Build Status](https://travis-ci.org/mewebstudio/captcha.svg?branch=master)](https://travis-ci.org/mewebstudio/captcha) [![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/mewebstudio/captcha/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/mewebstudio/captcha/?branch=master)
[![Latest Stable Version](https://poser.pugx.org/mews/captcha/v/stable.svg)](https://packagist.org/packages/mews/captcha)
[![Latest Unstable Version](https://poser.pugx.org/mews/captcha/v/unstable.svg)](https://packagist.org/packages/mews/captcha)
[![License](https://poser.pugx.org/mews/captcha/license.svg)](https://packagist.org/packages/mews/captcha)
[![Total Downloads](https://poser.pugx.org/mews/captcha/downloads.svg)](https://packagist.org/packages/mews/captcha)

A simple [Laravel 5/6](http://www.laravel.com/) service provider for including the [Captcha for Laravel](https://github.com/mewebstudio/captcha).

for Laravel 4 [Captcha for Laravel Laravel 4](https://github.com/mewebstudio/captcha/tree/master-l4)

## Preview
![Preview](https://image.ibb.co/kZxMLm/image.png)

- [Captcha for Laravel 5/6/7](#captcha-for-laravel-5-6-7)
  * [Preview](#preview)
  * [Installation](#installation)
  * [Usage](#usage)
  * [Configuration](#configuration)
    + [Custom settings:](#custom-settings)
    + [Disable validation:](#disable-validation)
  * [Example Usage](#example-usage)
    + [Session Mode:](#session-mode)
    + [Stateless Mode:](#stateless-mode)
- [Return Image](#return-image)
- [Return URL](#return-url)
- [Return HTML](#return-html)
- [To use different configurations](#to-use-different-configurations)
  * [Links](#links)
  
## Installation


Require this package with composer:
```
composer require ufucms/captcha
```

Update your packages with ```composer update``` or install with ```composer install```.

In Windows, you'll need to include the GD2 DLL `php_gd2.dll` in php.ini. And you also need include `php_fileinfo.dll` and `php_mbstring.dll` to fit the requirements of `mews/captcha`'s dependencies.




## Usage

To use the Captcha Service Provider, you must register the provider when bootstrapping your Laravel application. There are
essentially two ways to do this.

Find the `providers` key in `config/app.php` and register the Captcha Service Provider.

```php
    'providers' => [
        // ...
        'Ufucms\Captcha\CaptchaServiceProvider',
    ]
```
for Laravel 5.1+
```php
    'providers' => [
        // ...
        Ufucms\Captcha\CaptchaServiceProvider::class,
    ]
```

Find the `aliases` key in `config/app.php`.

```php
    'aliases' => [
        // ...
        'Captcha' => 'Ufucms\Captcha\Facades\Captcha',
    ]
```
for Laravel 5.1+
```php
    'aliases' => [
        // ...
        'Captcha' => Ufucms\Captcha\Facades\Captcha::class,
    ]
```


## Configuration
### Custom settings:
To use your own settings, publish config.

```$ php artisan vendor:publish```

`config/captcha.php`

```php
return [
    'default'   => [
        'length'    => 5,
        'width'     => 120,
        'height'    => 36,
        'quality'   => 90,
        'math'      => true,  //Enable Math Captcha
        'expire'    => 60,    //Captcha expiration
    ],
    // ...
];
```
### Disable validation:
To disable the captcha validation use `CAPTCHA_DISABLE` environment variable. e.g. **.env** config:

```php
CAPTCHA_DISABLE=true
```


## Example Usage
### Session Mode:
```php

    // [your site path]/Http/routes.php
    Route::any('captcha-test', function() {
        if (request()->getMethod() == 'POST') {
            $rules = ['captcha' => 'required|captcha'];
            $validator = validator()->make(request()->all(), $rules);
            if ($validator->fails()) {
                echo '<p style="color: #ff0000;">Incorrect!</p>';
            } else {
                echo '<p style="color: #00ff30;">Matched :)</p>';
            }
        }
    
        $form = '<form method="post" action="captcha-test">';
        $form .= '<input type="hidden" name="_token" value="' . csrf_token() . '">';
        $form .= '<p>' . captcha_img() . '</p>';
        $form .= '<p><input type="text" name="captcha"></p>';
        $form .= '<p><button type="submit" name="check">Check</button></p>';
        $form .= '</form>';
        return $form;
    });
```
### Stateless Mode:
You get key and img from this url
`http://localhost/captcha/api/math`
and verify the captcha using this method:
```php
    //key is the one that you got from json response
    // fix validator
    // $rules = ['captcha' => 'required|captcha_api:'. request('key')];
    $rules = ['captcha' => 'required|captcha_api:'. request('key') . ',math'];
    $validator = validator()->make(request()->all(), $rules);
    if ($validator->fails()) {
        return response()->json([
            'message' => 'invalid captcha',
        ]);

    } else {
        //do the job
    }
```

# Return Image
```php
captcha();
```
or
```php
Captcha::create();
```


# Return URL
```php
captcha_src();
```
or
```
Captcha::src('default');
```

# Return HTML
```php
captcha_img();
```
or
```php
Captcha::img();
```

# To use different configurations
```php
captcha_img('flat');

Captcha::img('inverse');
```
etc.

Based on [Intervention Image](https://github.com/Intervention/image)

^_^

## Links
* [Intervention Image](https://github.com/Intervention/image)
* [L5 Captcha on Github](https://github.com/mewebstudio/captcha)
* [L5 Captcha on Packagist](https://packagist.org/packages/mews/captcha)
* [For L4 on Github](https://github.com/mewebstudio/captcha/tree/master-l4)
* [License](http://www.opensource.org/licenses/mit-license.php)
* [Laravel website](http://laravel.com)
* [Laravel Turkiye website](http://www.laravel.gen.tr)
* [MeWebStudio website](http://www.mewebstudio.com)
