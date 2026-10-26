# evo-agents-market

The default plugin market of [evo-agents](https://github.com/maycuatroi1/evo-agents) hubs. `market.json` lists the
plugins a hub shows under Market, in the format plugin-market-v1 (`docs/market.md` of evo-agents). The market holds
no code: each plugin's bundle and its signature are assets of a GitHub Release of the plugin's own repository.

A hub reads this repository's `market.json` at `main` (`EVO_HUB_MARKET_DEFAULT_SOURCE`, `maycuatroi1/evo-agents-market`
unless set) when it starts and every six hours.

What it lists now:

| id | tier | price | repository |
| --- | --- | --- | --- |
| crm, pm, marketing, telegram, chats | core, in the hub's image (`evo-agents-core-plugins`) | free | maycuatroi1/evo-agents-core-plugins |
| lumen-hello | isolated, installed from its release | free | maycuatroi1/evo-plugin-template |

The seller `omelet` (https://agents.omelet.tech) sells the paid plugins this index will list; its key signs their
licences.

To list a plugin, open a pull request: [CONTRIBUTING.md](CONTRIBUTING.md). CI checks every pull request and `main`
with:

```sh
evo-agents plugin market-check market.json --releases
```

Licensed under Apache-2.0 ([LICENSE](LICENSE)). Each plugin carries its own licence.
