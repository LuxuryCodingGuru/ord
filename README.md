`ord`
=====

`ord` is an index, block explorer, and command-line wallet. It is experimental
software with no warranty. See [LICENSE](LICENSE) for more details.

Ordinal theory imbues satoshis with numismatic value, allowing them to
collected and traded as curios.

Ordinal numbers are serial numbers for satoshis, assigned in the order in which
they are mined, and preserved across transactions.

See [the docs](https://docs.ordinals.com) for documentation and guides.

See [the BIP](bip.mediawiki) for a technical description of the assignment and
transfer algorithm.

See [the project board](https://github.com/users/casey/projects/3/) for
currently prioritized issues.

See [milestones](https://github.com/casey/ord/milestones) to get a sense of
where the project is and where it's going.

Join [the Discord server](https://discord.gg/87cjuz4FYg) to chat with fellow
ordinal degenerates.

Wallet
------

`ord` relies on Bitcoin Core for private key managment and transaction signing.
This has a number of implications that you must understand in order to use
`ord` wallet commands safely:

- Bitcoin Core does is not aware of inscriptions and does not perform sat
  control. Using `bitcoin-cli` commands and RPC calls with `ord` wallets may
  lead to loss of inscriptions.

- `ord wallet` commands automatically load the `ord` wallet given by the
  `--wallet` option, which defaults to 'ord'. Keep in mind that after running
  an `ord wallet` command, an `ord` wallet may be loaded.

- Because `ord` has access to your Bitcoin Core wallets, `ord` should not be
  used with wallets that contain a material amount of funds. Keep ordinal and
  cardinal wallets segregated.

### Pre-alpha wallet migration

Alpha `ord` wallets are not compatible with wallets created by previous
versions of `ord`. To migrate, use `ord wallet send` from the old wallet to
send sats and inscriptions to addresses generated by the new wallet with `ord
wallet receive`.

Installation
------------

`ord` is written in Rust and can be built from
[source](https://github.com/casey/ord). Pre-built binaries are available on the
[releases page](https://github.com/casey/ord/releases).

You can install the latest pre-built binary from the command line with:

```sh
curl --proto '=https' --tlsv1.2 -fsLS https://ordinals.com/install.sh | bash -s
```

Once `ord` is installed, you should be able to run `ord --version` on the
command line.

Building
--------

On Debian and Ubuntu, `ord` requires `libssl-dev` when building from source:

```
sudo apt-get install libssl-dev
```

Syncing
-------

`ord` requires a synced `bitcoind` node with `-txindex` to build the index of
satoshi locations. `ord` communicates with `bitcoind` via RPC.

If `bitcoind` is run locally by the same user, without additional
configuration, `ord` should find it automatically by reading the `.cookie` file
from `bitcoind`'s datadir, and connecting using the default RPC port.

If `bitcoind` is not on mainnet, is not run by the same user, has a non-default
datadir, or a non-default port, you'll need to pass additional flags to `ord`.
See `ord --help` for details.

Logging
--------

`ord` uses [env_logger](https://docs.rs/env_logger/latest/env_logger/). Set the
`RUST_LOG` environment variable in order to turn on logging. For example, run
the server and show `info`-level log messages and above:

```
$ RUST_LOG=info cargo run server
```
