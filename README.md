# exchange-proxy

Exchange proxy using WebSockets to maintain candlestick/klines data in memory, thus having great performance, reducing the number of API calls to the exchange API, decreases latency and CPU usage.
There is no warranty of correct working. You take all risks of using this.
All improvements are made by me on a voluntary basis in my spare time.

## Note
If you want to run more than a few pairs (50+) it is advised you crank your ulimit up **before** starting the exchange-proxy, otherwise you will get 509 errors leading to crashes of exchange-proxy
```shell
ulimit -n 10000
ulimit -Hn 10000
```



## Installation
### Local
#### Prerequisites
```shell
sudo apt-get install golang-go
sudo apt-get install golang-easyjson
```
#### Main part
```shell
git clone https://github.com/dida1990/exchange-proxy
cd exchange-proxy
make build
```

Optimal run parameters for NFIX2
```shell
dist/exchange-proxy -port 9000 -kucoin-topics-per-ws 10 -ttl-cache-timeout 30m0s -verbose 0
```
#### Configuration
Add these lines to your config
```shell
{
    "exchange": {
        "ccxt_config": {
            "enableRateLimit": false,
            "timeout": 60000,
            "urls": {
                "api": {
                  "public": "http://127.0.0.1:9000/kucoin",
                  "private": "http://127.0.0.1:9000/kucoin"
        }
      }},
        "ccxt_async_config": {
            "enableRateLimit": false,
            "timeout": 60000
        }
    }
}
```
### Docker Compose (just AMD64 currently)
Add the following lines to your docker-compose.yml
```shell
  exchange-proxy:
    image: dida1990/exchange-proxy:v1.2.9
    restart: unless-stopped
    container_name: exchange-proxy
    command: >
      -verbose 1
      -port 9000
      -kucoin-topics-per-ws 10
      -ttl-cache-timeout 30m0s
```
#### Configuration
Add these lines to your config
```shell
{
    "exchange": {
        "ccxt_config": {
            "enableRateLimit": false,
            "timeout": 60000,
            "urls": {
                "api": {
                  "public": "http://exchange-proxy:9000/kucoin",
                  "private": "http://exchange-proxy:9000/kucoin"
        }
      }},
        "ccxt_async_config": {
            "enableRateLimit": false,
            "timeout": 60000
        }
    }
}
```
Then just
```shell
docker compose pull
docker compose up -d
```


## OPS

### Usage
```shell
Usage of ./dist/exchange-proxy:
  -bindaddr string
        bindable address (default "0.0.0.0")
  -cache-size int
        amount of candles to cache (default 1000)
  -client-timeout duration
        client timeout (default 15s)
  -concurrency-limit int
        server concurrency limit (default 262144)
  -kucoin-api-url string
        kucoin api address (default "https://openapi-v2.kucoin.com")
  -kucoin-topics-per-ws int
        amount of topics per ws connection [10-280] (default 200)
  -port string
        listen port (default "8080")
  -ttl-cache-timeout duration
        ttl of blobs of cached data (default 10m0s)
  -verbose int
        verbose level: 0 - info, 1 - debug, 2 - trace
```

#### Note
All unforeseen connection errors or the inaccessibility of the exchange will lead to the proxy crash, which means that you have to handle it on your end 

### Local
```shell
./exchange-proxy -port 8080
```

### Docker (suggested way)

###### Use different tags for different platforms e.g. - latest-amd64, latest-arm-v6, latest-arm-v7, latest-arm64

```shell
docker run --restart=always -p 127.0.0.1:8080:8080 --name exchange-proxy -d dida1990/exchange-proxy:v1.2.9
```

#### Examples of usage:
- [freqtrade](./docs/ops/freqtrade.md)

# Supported exchanges:
- [Kucoin](./docs/exchanges/kucoin.md)

## Donations

Donations are appreciated and will make me motivated to support and improve the project.

- USDT TRC20 - TYssA3EUfAagJ9afF6vfwJvwwueTafMbGY
- XRP - rNFugeoj3ZN8Wv6xhuLegUBBPXKCyWLRkB 1869777767
- DOGE - D6xwe5V9jRkvWksiHiajwZsJ3KJxBVqBUC
- BTC - 35SrQDWAfwXcRGHaKbxNWwvHRNSLAbVjrk
- ETH - 0x37c34bac13cf60f022be1bdea2dec1136cdc838a


### Referral links:
- [Kucoin](https://www.kucoin.com/ucenter/signup?rcode=rJ327D3)

- [Okex](https://www.okex.com/join/3941527)

- [Gate.io](https://www.gate.io/signup/3325373)

- [Currency.com](https://currency.com/trading/signup?c=ciqjuj5y&pid=referral)
