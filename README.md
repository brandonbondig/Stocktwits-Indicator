# Stocktwits Indicator
Neat little indicator showing the ratio between bull and bears on stocktwits.
The file iterates throug the top trending stocks on stocktwits and counts all bull/bears cases.
it then makes a ratio where 0 < x is bullish and the oppersite is bearish.

### INPUT
```
stocktwits_indicator()
```
### OUTPUT
```
4.253
```

## The Code:
```
import requests
import time

def stocktwits_indicator():
    tickers = []

    # Find all trending stocks today
    url = requests.get('https://api.stocktwits.com/api/2/trending/symbols.json')
    api = url.json()
    for x in api['symbols']:
        tickers.append(x['symbol'])

    # While loop for iterating through bull/bear cases
    while True:
        bull = 0
        bear = 0

        # for loop for iterating through top trending stocks,
        # then appending according to bull/bear side
        for i in tickers:
            url = requests.get('https://api.stocktwits.com/api/2/streams/symbol/{}.json?filter=all&limit=30'.format(i))
            api = url.json()

            # Max range is limited to 30 pr ticker
            for x in range(30):
                if api['messages'][x]['entities']['sentiment'] is not None:
                    if api['messages'][x]['entities']['sentiment']['basic'] == 'Bearish':
                        bear += 1
                    else:
                        bull += 1

        # Indicator
        indicator = round((bull/bear), 3)

        print(end='\r')
        print(indicator, end='')

        time.sleep(1)
```
