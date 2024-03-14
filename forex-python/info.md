For anyone reading this and struggling to find a working API for the forex-python assignment, I've found this one:
```
https://app.freecurrencyapi.com/login
```
This does require an API key as well, but it is free and will let you make 5,000 requests per month, which should be more than enough to complete the assignment.
There are two main endpoints you'd call. This one gives you the conversion rates between the two currencies:
```
"https://api.freecurrencyapi.com/v1/latest?apikey=PUTYOURAPIKEYHERE&currencies=EUR,USD"
```
That output looks like this:
```
{
  "data": {
    "EUR": 0.9193901781,
    "USD": 1
  }
}
```
The other gives you the symbols for each currency, if you want them:
```
"https://api.freecurrencyapi.com/v1/currencies?apikey=PUTYOURAPIKEYHERE&currencies=EUR,USD"
```
That output looks like this:
```
{
  "data": {
    "EUR": {
      "symbol": "€",
      "name": "Euro",
      "symbol_native": "€",
      "decimal_digits": 2,
      "rounding": 0,
      "code": "EUR",
      "name_plural": "Euros",
      "type": "fiat"
    },
    "USD": {
      "symbol": "$",
      "name": "US Dollar",
      "symbol_native": "$",
      "decimal_digits": 2,
      "rounding": 0,
      "code": "USD",
      "name_plural": "US dollars",
      "type": "fiat"
    }
  }
}
```
I know that some have been having trouble with both forex-python and exchangerate.host, and it seems that the latter has started to require an API key that some are having trouble getting.
The above site's API key is free, very quick to sign up for, and will hopefully work well if the other two options do not.
If you aren't sure about how you would incorporate this API into a Flask app, please see this post for an example; you'd just need to replace the url in that example. I hope it helps someone.

## Example Script
```
from pprint import pprint
from typing import Dict, Final, Set

import requests

API_KEY: Final[str] = "PUT YOUR API KEY HERE"
BASE_URL: Final[str] = "http://api.exchangerate.host/convert"
HTTP_ERROR_CODES: Final[Set[int]] = set([i for i in range(400, 600)])

params: Dict[str, str] = {
    "access_key": API_KEY,
    "from": "EUR",
    "to": "GBP",
    "amount": "100"
}


response = requests.get(BASE_URL, params=params, allow_redirects=False)

if response.status_code in HTTP_ERROR_CODES:
    print(f"HTTP error: {response.status_code}, Message: {response.reason}")
    exit(1)

pprint(response.json())
```
