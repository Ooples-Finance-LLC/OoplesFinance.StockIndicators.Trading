# OoplesFinance.StockIndicators.Trading

Broker and live market data adapters for
[OoplesFinance.StockIndicators](https://github.com/ooples/OoplesFinance.StockIndicators).

[![NuGet](https://img.shields.io/nuget/v/OoplesFinance.StockIndicators.Trading.svg)](https://www.nuget.org/packages/OoplesFinance.StockIndicators.Trading/)
[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE.txt)

## Why this is a separate package

Installing a library to compute an RSI should not also install a trading API client. Only two types
need the Alpaca SDK — everything else in the core library computes indicators — so they live here
instead, and the core package carries no broker dependency.

## What is in it

| Type | Implements |
|---|---|
| `AlpacaBroker` | `IBroker` — account, positions, order submission, cancellation, closing positions |
| `AlpacaMarketDataProvider` | `IMarketDataProvider`, `ICryptoMarketDataProvider` — historical and real-time bars, quotes, trades, snapshots, subscriptions |

Both support paper and live environments.

## Install

```shell
dotnet add package OoplesFinance.StockIndicators
dotnet add package OoplesFinance.StockIndicators.Trading
```

The namespaces are unchanged from when these types shipped inside the core package
(`OoplesFinance.StockIndicators.Builder.Trading` and `...Builder.MarketData`), so moving to this
package means adding a package reference — no `using` needs editing.

## Usage

```csharp
using OoplesFinance.StockIndicators.Builder.Trading;

var broker = new AlpacaBroker(new AlpacaOptions
{
    ApiKey = Environment.GetEnvironmentVariable("ALPACA_KEY"),
    ApiSecret = Environment.GetEnvironmentVariable("ALPACA_SECRET"),
    UsePaper = true
});

var account = await broker.GetAccountAsync();
```

Credentials are read from `ALPACA_KEY` / `ALPACA_SECRET` when not supplied explicitly.

## Versioning

This package versions independently of the core library. It takes a **minimum** core version rather
than a pin, so you control which core version you resolve, provided it is at least the floor this
package is built against.

## Running the tests

Unit tests run with no configuration. The integration tests talk to the live Alpaca **paper**
trading API and skip themselves unless credentials are present:

```shell
export ALPACA_API_KEY=...
export ALPACA_API_SECRET=...
dotnet test
```

Without those variables the integration tests report as skipped rather than failed — a clean
checkout is green.

## License

Apache-2.0. See [LICENSE.txt](LICENSE.txt).
