# Macopedia OmnibusDirective module

Implementation of EU Omnibus Directive for Magento 2.

 - [Requirements](#requirements)
 - [Main features](#main-features)
 - [Not implemented features](#not-implemented-features)
 - [Installation](#installation)
 - [Prices import](#prices-import)

## Requirements

   Magento version > 2.4.x

## Main features

1. Historical prices
   1. Adds `Historical price` and `Historical price updated at` product attributes
   2. Keeps minimal price from last 30 days - calculated from `price` and `special_price` attributes change after product save in admin panel
   3. Cronjob which removes historical prices older than 30 days
   4. Displays historical price on product view page
2. Trusted review validation
   1. Extends product review form by an additional input `order_number`
   2. Validates purchased product by order number (Increment ID) - only validated buyer can add review to product

## Not implemented features

- calculate historical price after save by Rest API and Soap API
- calculate historical price after product mass action update
- keep all price changes from last 30 days
- keep historical prices per customer groups
- keep historical prices calculated by catalog rules
- keep historical tier prices

## Installation

1. Using composer: 

```
composer require macopedia/module-omnibusdirective
```

2. Using zip file:
   1. Download zip file
   2. Extract module in directory `app/code/Macopedia/OmnibusDirective`

Enable module and install patches:
```
   bin/magento module:enable Macopedia_OmnibusDirective
   bin/magento setup:upgrade
```


Notice - if you have custom theme and modified review form template file `Magento_Review/templates/form.phtml` you have to add manually `order_number` input (see example in file `view/frontend/templates/review/form.phtml`):

```html
<div class="field review-field-order-id required">
    <label for="order_id_field" class="label"><span><?= $block->escapeHtml(__('Order number')) ?></span></label>
    <div class="control">
        <input type="text" name="order_id" id="order_id_field" class="input-text" data-validate="{required:true}" data-bind="value: review().order_id" />
    </div>
</div>
```

## Prices import

If you have custom script to import prices, you can use method `\Macopedia\OmnibusDirective\Model\Product\HistoricalPrice::getHistoricalPriceUpdateValue` to calculate historical price.
As parameters, you should pass array with original prices `$origData` (before import values):

```php
$origData = 
[
    'special_price' => xxx,
    'historical_price' => xxx,
    'price' => xxx
]
```

and updated prices array `$data` (after import values):

```php
$data = 
[
    'special_price' => xxx,
    'price' => xxx
]
```