yii2-jsonld-helper
================

Yii2 helper class for registering structured data markup in [JSON-LD](http://json-ld.org/) format.

## Resources
 * Yii2 [extension page](http://www.yiiframework.com/extension/yii2-jsonld-helper)
 * JSON-LD [documentation](http://json-ld.org/learn.html)
 * Google [Structured Data Testing Tool](https://search.google.com/structured-data/testing-tool)

## Installation

### Composer

Add extension to your `composer.json` and update your dependencies as usual, e.g. by running `composer update`
```js
{
    "require": {
        "nirvana-msu/yii2-jsonld-helper": "1.0.*@dev"
    }
}
```

##Sample Usage

To let search engines know how to display your website name in search results,
you can add the following JSON-LD document somewhere on your landing page:

```php
$doc = (object)[
    "@type" => "http://schema.org/WebSite",
    "http://schema.org/name" => Yii::$app->params['brand'],
    "http://schema.org/url" => Yii::$app->urlManager->hostInfo
];

JsonLDHelper::add($doc);
```

You may  pass `$context` as an optional second parameter if you need to use something other than default `["@vocab" => "http://schema.org/"]`:

```php
JsonLDHelper::add($doc, $context);
```

Note that doing so may cause resulting script to not pass validation by the Google's [SDTT] (https://search.google.com/structured-data/testing-tool) - refer this this [stackoverflow question](http://stackoverflow.com/questions/35879351/google-structured-data-testing-tool-fails-to-validate-type-as-an-alias-of-type) for details.

You can also use `JsonLDHelper::addBreadcrumbList` to add `BreadcrumbList` schema.org markup 
based on the application view `breadcrumbs` parameter. E.g. in the beginning of your layout add:

```php
JsonLDHelper::addBreadcrumbList();
```

Finally, you must invoke `JsonLDHelper::registerScripts` method in the `<head>` section of your layout, e.g.

```html
<head>
    <!-- ... -->
    <?php JsonLDHelper::registerScripts(); ?>
    <?php $this->head() ?>
</head>
```

###Example with nested data:

```php
$doc = [
    "@type" => "http://schema.org/BlogPosting",
    "http://schema.org/mainEntityOfPage" => (object)[
        "@type" => "http://schema.org/WebPage",
        "@id" => "http://example.com/awesome-blog-post",
    ],
    "http://schema.org/headline" => "Post Title",
    "http://schema.org/articleBody" => "Post Body",
    "http://schema.org/author" => (object)[
        "@type" => "http://schema.org/Person",
        "http://schema.org/name" => "Jon Snow",
        "http://schema.org/url" => "http://example.com",
        "http://schema.org/sameAs" => [
            "https://www.instagram.com/kitharingtonn/",
        ]
    ],
];

JsonLDHelper::add($doc);
```

Note that this extension is just a thin wrapper around [lanthaler/JsonLD](https://github.com/lanthaler/JsonLD) processor - refer to this library for the full documentation.

##License

Extension is released under MIT license.
