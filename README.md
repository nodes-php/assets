## Assets

A easy integration for handling files with [Laravel](http://laravel.com/docs).

[![Total downloads](https://img.shields.io/packagist/dt/nodes/assets.svg)](https://packagist.org/packages/nodes/assets)
[![Monthly downloads](https://img.shields.io/packagist/dm/nodes/assets.svg)](https://packagist.org/packages/nodes/assets)
[![Latest release](https://img.shields.io/packagist/v/nodes/assets.svg)](https://packagist.org/packages/nodes/assets)
[![Open issues](https://img.shields.io/github/issues/nodes-php/assets.svg)](https://github.com/nodes-php/assets/issues)
[![License](https://img.shields.io/packagist/l/nodes/assets.svg)](https://packagist.org/packages/nodes/assets)
[![Star repository on GitHub](https://img.shields.io/github/stars/nodes-php/assets.svg?style=social&label=Star)](https://github.com/nodes-php/assets/stargazers)
[![Watch repository on GitHub](https://img.shields.io/github/watchers/nodes-php/assets.svg?style=social&label=Watch)](https://github.com/nodes-php/assets/watchers)
[![Fork repository on GitHub](https://img.shields.io/github/forks/nodes-php/assets.svg?style=social&label=Fork)](https://github.com/nodes-php/assets/network)
[![Travis tests](https://img.shields.io/travis/nodes-php/assets.svg)](https://travis-ci.org/nodes-php/assets)
[![StyleCI](https://styleci.io/repos/45786087/shield)](https://styleci.io/repos/45786087)
[![Code Climate](https://codeclimate.com/github/nodes-php/assets/badges/gpa.svg)](https://codeclimate.com/github/nodes-php/assets)

## 📝 Introduction

One thing we at [Nodes](http://nodesagency.com) have been missing in [Laravel](http://laravel.com/docs) is a fast easy file storage integration system.

The `Storage` facade made it easier, but we still need the option to upload from differente formats and store to a very specific folder structure, retrieve a path and be able to create urls to serve api and sites.

## 📦 Installation

To install this package you will need:

* Laravel 5.1+
* PHP 5.5.9+

You must then modify your `composer.json` file and run `composer update` to include the latest version of the package in your project.

```json
"require": {
    "nodes/assets": "^1.1"
}
```

Or you can run the composer require command from your terminal.

```bash
composer require nodes/assets:^1.1
```

## 🔧 Setup

Setup service provider in `config/app.php`

```php
Nodes\Assets\ServiceProvider::class
```

Setup alias in `config/app.php`

```php
'Assets' => Nodes\Assets\Support\Facades\Assets::class
```

Publish config files

```bash
php artisan vendor:publish --provider="Nodes\Assets\ServiceProvider"
```

If you want to overwrite any existing config files use the `--force` parameter

```bash
php artisan vendor:publish --provider="Nodes\Assets\ServiceProvider" --force
```

### S3 + ImgIX

In general.php config
```php
'upload' => [
        'provider' => function () {
            $s3Config = config('filesystems.disks.s3');
            $awsS3Config = config('nodes.assets.provider.aws-s3');
            return new \Nodes\Assets\Upload\Providers\AmazonS3($awsS3Config, $s3Config);
        },
    ],   
    'url'    => [
        'provider' => function () {
            $imgIxConfig = config('nodes.assets.providers.imgix');

            return new \Nodes\Assets\Url\Providers\ImgIX($imgIxConfig);
        },
    ],

```

Make sure to have .env setup

```.dotenv
AMAZON_BUCKET=bucket
AMAZON_KEY=key
AMAZON_SECRET=secret
AMAZON_REGION=eu-west-1
CDN_BASE_URL=nodes-cdn-development.imgix.net

```

### Vapor Cloud

In general.php config

```php

<?php

return [
   
    'upload' => [
        'provider' => function () {
            $s3Config = config('filesystems.disks.s3');
            $vaporCloudConfig = config('nodes.assets.providers.vapor-cloud');

            return new \Nodes\Assets\Upload\Providers\VaporCloud($s3Config, $vaporCloudConfig);
        },
    ],

    'url'    => [
        'provider' => function () {
            $vaporCloudConfig = config('nodes.assets.providers.vapor-cloud');

            return new \Nodes\Assets\Url\Providers\VaporCloud($vaporCloudConfig);
        },
    ],
];


```


Make sure to have .env setup

```dotenv
AMAZON_BUCKET=bucket
AMAZON_KEY=key
AMAZON_SECRET=secret
AMAZON_REGION=eu-west-1
AMAZON_CLOUDFRONT_URL={insert imgix base url here locally}
AMAZON_CLOUDFRONT_URL_DATA={insert imgix base url here locally}

```

### Public Folder

Public folder method can resize images. To setup, you need Intervention image lib. To install run

```bash
composer require intervention/image
```

Then add the following to app.php

**providers** array
```php
Intervention\Image\ImageServiceProvider::class
```

**aliases** array
```php
'Image' => Intervention\Image\Facades\Image::class
```

finally run
```bash
php artisan vendor:publish --provider="Intervention\Image\ImageServiceProviderLaravel5"
```

#### Configure for public folder
Modify the configuration in `config/nodes/assets/general.php`
```php
return [
    'upload' => [
        'provider' => function () {
            return new \Nodes\Assets\Upload\Providers\PublicFolder();
        },
    ],
    'url'    => [
        'provider' => function () {
            return new \Nodes\Assets\Url\Providers\PublicFolder();
        },
    ],
];
```


## ⚙ Usage

### Global methods

```php
function assets_add($file, $folder = null, UploadSettings $settings = null)
```

```php
function assets_add_uploaded_file(UploadedFile $file, $folder = null, UploadSettings $settings = null)
```

```php
function assets_add_url($url, $folder = null, UploadSettings $settings = null)
```

```php
function assets_add_data_uri($dataUri, $folder = null, UploadSettings $settings = null)
```

```php
function assets_get($path, UrlSettings $settings = null)
```

### Facade methods

```php
Assets::add($file, $folder = null, UploadSettings $settings = null)
```

```php
Assets::addFromUploadedFile(UploadedFile $file, $folder = null, UploadSettings $settings = null)
```

```php
Assets::addFromUrl($url, $folder = null, UploadSettings $settings = null)
```

```php
Assets::addFromDataUri($dataUri, $folder = null, UploadSettings $settings = null)
```

```php
Assets::get($path)
```

## 🏆 Credits

This package is developed and maintained by the PHP team at [Nodes](http://nodesagency.com)

[![Follow Nodes PHP on Twitter](https://img.shields.io/twitter/follow/nodesphp.svg?style=social)](https://twitter.com/nodesphp) [![Tweet Nodes PHP](https://img.shields.io/twitter/url/http/nodesphp.svg?style=social)](https://twitter.com/nodesphp)

## 📄 License

This package is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT)

