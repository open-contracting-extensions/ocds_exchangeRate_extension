# Exchange Rate Extension

Every financial value in OCDS data should specify the currency it is provided in. For example:

```json
{
    "amount":1000,
    "currency":"USD"
}
```

There are situations where:

1. A contract specifies that a particular exchange rate should be used when invoices are issued or payments made;
1. A contract, agreement or policy states that any currency conversion that takes place should use a particular exchange rate, or the rate as of a particular date;
1. Values have been converted from an alternative currency (e.g. during data production, or by an application displaying data) into a single normalized currency.

The exchange rate extension can be used to explicitly declare the exchange rate used, where the rate is specified, and the data used for currency conversions.

## Listing exchange rates

The extension adds an `exchangeRates` array to every `value` object. Use of the extension is optional in all places it occurs.

The `exchangeRates` array can contain one or more `ExchangeRate` objects, consisting of:

* `currency` - The destination currency for conversion of amount. Currency should always be specified using the uppercase 3-letter currency code from ISO4217.
* `rate` - The rate to use in converting amount to the alternative currency. Amount x Rate = Alternative Currency Amount.
* `date` - The date at which this exchange rate was current.
* `source` - The source used to provide the value of the exchange rate, taken from the 'exchangeRateSource' codelist.

'exchangeRateSource' is an open codelist with the following initial values:

* contract - The exchange rate is specified in the contract.
* market - The exchange rate was taken from the market on the date provided
* application - The exchange rate was selected by the application processing data

## Example

The following example shows:

```json
{
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
```

## Expressing alternative currency values

The exchange rate extension does not provide a way to express the converted value. However, applications and publishers are encouraged to adopt the following convention for including multiple currency **values** within the `value` object.

* `amount` should always be the value in the currency specified by `currency`.
* `amount_CODE` can be used to provide the amount in an alternative currency, where 'CODE' is the ISO4217 code of that currency.

For example, an application which reads in OCDS data in multiple currencies, but wishes to display all data to users in US Dollars ($) would perform currency conversion (guided by `exchangeRates` where available) and would insert the property `amount_USD` as shown in the example below.

```json
{
  "value": {
    "amount": 6563700,
    "currency": "MXN",
    "amount_USD": 426640.50
  }
}
```

Applications are then able to access the original currency value (`amount` and `currency`, and the normalised amount `amount_USD`).

Note that the schema extension does not currently declare support for this convention, so it's use will trigger 'additional field' warnings on the [OCDS Validator](http://standard.open-contracting.org/validator/)

## ToDo

Add pattern properties to validate `amount_CODE` fields.

## Discussion

This extension was discussed in <https://github.com/open-contracting/standard/issues/384>.

## Issues

Report issues for this extension in the [ocds-extensions repository](https://github.com/open-contracting/ocds-extensions/issues), putting the extension's name in the issue's title.
