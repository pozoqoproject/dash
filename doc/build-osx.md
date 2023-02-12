# macOS Build Instructions and Notes

The commands in this guide should be executed in a Terminal application.
The built-in one is located in
```
/Applications/Utilities/Terminal.app
```

## Preparation
Install the macOS command line tools:

```shell
xcode-select --install
```

When the popup appears, click `Install`.

Then install [Homebrew](https://brew.sh).

## Base build dependencies

```shell
brew install automake libtool pkg-config libnatpmp
```

If you run into issues, check [Homebrew's troubleshooting page](https://docs.brew.sh/Troubleshooting).

If you want to build the disk image with `make deploy` (.dmg / optional), you need RSVG:
```shell
brew install librsvg
```

## Building

It's possible that your `PATH` environment variable contains some problematic strings, run
```shell
export PATH=$(echo "$PATH" | sed -e '/\\/!s/ /\\ /g') # fix whitespaces
```

Next, follow the instructions in [build-generic](build-generic.md)

## `disable-wallet` mode
When the intention is to run only a P2P node without a wallet, Pozoqo Core may be
compiled in `disable-wallet` mode with:
```shell
./configure --disable-wallet
```

In this case there is no dependency on Berkeley DB 4.8.

Mining is also possible in disable-wallet mode using the `getblocktemplate` RPC call.

## Running

Pozoqo Core is now available at `./src/pozoqod`

Before running, you may create an empty configuration file:
```shell
mkdir -p "/Users/${USER}/Library/Application Support/PozoqoCore"

touch "/Users/${USER}/Library/Application Support/PozoqoCore/pozoqo.conf"

chmod 600 "/Users/${USER}/Library/Application Support/PozoqoCore/pozoqo.conf"
```

The first time you run pozoqod, it will start downloading the blockchain. This process could
take many hours, or even days on slower than average systems.

You can monitor the download process by looking at the debug.log file:
```shell
tail -f $HOME/Library/Application\ Support/PozoqoCore/debug.log
```

## Other commands:

```shell
./src/pozoqod -daemon # Starts the pozoqo daemon.
./src/pozoqo-cli --help # Outputs a list of command-line options.
./src/pozoqo-cli help # Outputs a list of RPC commands when the daemon is running.
```
