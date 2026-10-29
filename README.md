# OpenSea Mint Bot

[![Tests](https://github.com/Takumixbt/Opensea-bot/actions/workflows/tests.yml/badge.svg)](https://github.com/Takumixbt/Opensea-bot/actions/workflows/tests.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Scan OpenSea drops, inspect a stage, and mint from a wallet you control. Run the
terminal CLI or optional Telegram bot locally; your wallet key signs transactions
on that machine.

## Quick start

You need Python 3.11+, an [Alchemy API key](https://www.alchemy.com/), an
[OpenSea API key](https://docs.opensea.io/reference/api-keys), and a separate
EVM wallet with a small amount of gas. From PowerShell:

```powershell
git clone https://github.com/Takumixbt/Opensea-bot.git
cd Opensea-bot
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python cli.py
```

The first-run wizard writes a local `.env` file and asks for credentials in the
terminal. You can also copy `.env.example` to `.env` and fill it in yourself.
Git ignores `.env`; never commit it or paste secrets into chat, Telegram, or an
issue.

## Use

```powershell
python cli.py help
python cli.py scan base --free --public
python cli.py info https://opensea.io/collection/example
python cli.py wallet base
python cli.py mint 1 --qty 1
python cli.py schedule 1 --yes
python cli.py watch
python status.py
```

`scan` lists OpenSea drops. `info` checks a collection or drop URL. `mint`
previews the selected stage; `--yes` confirms a live action. `schedule` arms a
one-time mint at stage opening. Schedules only run while the process is open;
use `watch` or the Linux service in `deploy/` to keep it alive. `status.py` is a
read-only readiness check.

The bot uses a direct transaction only for public SeaDrop stages when its
on-chain price and opening time match OpenSea's preview. Hosted or allowlist
stages use OpenSea's signing route. If it cannot confirm a safe route, it
refuses to mint. A transaction hash means broadcast succeeded, not that the
mint was included or supply remains.

## Telegram (optional)

Create a bot with `@BotFather`, then set `TELEGRAM_BOT_TOKEN` and
`TELEGRAM_ALLOWED_CHAT_ID` in `.env`. Start it with:

```powershell
python telegram_bot.py
```

The bot ignores other chats. Run only one Telegram process per token, and do
not run it alongside `python cli.py watch` for the same wallet.

## Safety

Live minting is off by default; mint and buy price caps are both `0`. Use a
throwaway wallet and keep live mode off until a dry run shows the exact chain,
stage, quantity, and value you expect. Live sends require enabling the switch,
raising the relevant cap, and confirming the action. Do not import a seed
phrase.

Arc is supported as an EVM chain with native USDC gas. The `.env.example` lists
optional RPC, wallet, Telegram, and safety settings.

For development, see [CONTRIBUTING.md](CONTRIBUTING.md). Report vulnerabilities
as described in [SECURITY.md](SECURITY.md). Licensed under MIT; see [LICENSE](LICENSE).
