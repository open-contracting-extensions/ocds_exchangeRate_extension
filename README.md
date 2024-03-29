# Exchange rate

**This is a draft extension and is not recommended for use. Please contribute to [this GitHub issue](https://github.com/open-contracting/standard/issues/384).**

Every financial value in OCDS data should specify the currency it is provided in. For example:

```json
{
  "tender": {
    "value": {
      "amount": 1000,
      "currency": "USD"
    }
  }
}
```

There are situations where:

1. A contract specifies that a particular exchange rate should be used when invoices are issued or payments made;
1. A contract, agreement or policy states that any currency conversion that takes place should use a particular exchange rate, or the rate as of a particular date;
1. Values have been converted from an alternative currency (e.g. during data production, or by an application displaying data) into a single normalized currency.

The exchange rate extension can be used to explicitly declare the exchange rate used, where the rate is specified, and the date used for currency conversions.

## Example

The following example shows:

```json
{
  "tender": {
    "value": {
      "amount": 6563700,
      "currency": "MXN",
      "exchangeRates": [
        {
          "currency": "USD",
          "rate": 0.065,
          "date": "2015-11-04T00:00:00-05:00",
          "source": "contract"
        }
      ]
    }
  }
}
```

## Expressing alternative currency values

The exchange rate extension does not provide a way to express the converted value. However, applications and publishers are encouraged to adopt the following convention for including multiple currency **values** within the `value` object.

* `amount` must be the value in the currency specified by `currency`.
* `amount_CODE` may be used to provide the amount in an alternative currency, where `CODE` is the ISO4217 code of that currency.

For example, an application which reads in OCDS data in multiple currencies, but wishes to display all data to users in US Dollars ($) would perform currency conversion (guided by `exchangeRates` where available) and would insert the `amount_USD` field as shown in the example below.

```json
{
  "tender": {
    "value": {
      "amount": 6563700,
      "currency": "MXN",
      "amount_USD": 426640.5
    }
  }
}
```

Applications are then able to access the original currency value (`amount` and `currency`, and the normalized amount `amount_USD`).

Note that the schema extension does not currently declare support for this convention, so it's use will trigger additional field warnings in the [OCDS Data Review Tool](https://review.standard.open-contracting.org/)

## Issues

Report issues for this extension in the [ocds-extensions repository](https://github.com/open-contracting/ocds-extensions/issues), putting the extension's name in the issue's title.

## Changelog

### 2020-06-04

* Set `codelist`, `openCodelist` and `enum` on `ExchangeRate.currency`
* Review normative and non-normative words.

### 2020-04-24

* Add `minProperties`, `minItems` and/or `minLength` properties.

### 2019-11-13

* Add `patternProperties` for `amount_CODE` fields.

### 2019-03-20

* Set `"uniqueItems": true` on array fields.

### 2018-12-21

* Set `wholeListMerge` on `Value.exchangeRates`

This extension was originally discussed in <https://github.com/open-contracting/standard/issues/384>.
