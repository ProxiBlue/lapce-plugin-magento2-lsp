# lapce-plugin-magento2-lsp

Lapce plugin (volt) wrapping [@mage-os/magento2-lsp](https://github.com/mage-os-lab/magento2-lsp) — a Magento 2 cross-reference LSP that adds:

- Go-to-definition across DI preferences / plugins
- Plugin chain tracing
- Layout XML block → PHP class navigation
- `config_path` string → `etc/config.xml` resolution
- Module / area / scope awareness

Designed to run **alongside** a general PHP LSP (e.g. Intelephense), not as a replacement. magento2-lsp contributes Magento-specific navigation on top of standard PHP intelligence.

## Prerequisites

The language server binary must be installed and on `PATH`:

```sh
npm install -g @mage-os/magento2-lsp
```

Default lookup path: `/usr/local/bin/magento2-lsp`. Override via [Configuration](#configuration) below.

## Install

### From the Lapce plugin registry (recommended)

Open Lapce → Plugins panel (left sidebar) → search **"Magento 2 LSP"** → click Install.

### From a GitHub release (manual)

1. Download the latest `lapce-plugin-magento2-lsp-<version>.zip` from [Releases](https://github.com/proxiblue/lapce-plugin-magento2-lsp/releases).
2. Extract into the Lapce plugins directory:
   - Linux: `~/.local/share/lapce-stable/plugins/`
   - macOS: `~/Library/Application Support/lapce-stable/plugins/`
   - Windows: `%APPDATA%\lapce-stable\plugins\`
3. The extracted folder must be named `proxiblue.lapce-plugin-magento2-lsp-<version>/` and contain `volt.toml` and `lapce-plugin-magento2-lsp.wasm`.
4. Restart Lapce.

### From source

Requires Rust with `wasm32-wasip1` target:

```sh
git clone https://github.com/proxiblue/lapce-plugin-magento2-lsp
cd lapce-plugin-magento2-lsp
rustup target add wasm32-wasip1
cargo build --release
```

Then copy the plugin folder to the Lapce plugins directory:

```sh
# Linux example
DEST=~/.local/share/lapce-stable/plugins/proxiblue.lapce-plugin-magento2-lsp-0.1.0
mkdir -p "$DEST"
cp volt.toml "$DEST/"
cp target/wasm32-wasip1/release/lapce-plugin-magento2-lsp.wasm "$DEST/"
```

## Activation

Activates only inside Magento 2 workspaces (detected via the presence of `app/etc/di.xml` or `app/etc/config.php`). Attaches to `*.php` and `*.xml` files.

## Configuration

Override the server path in Lapce `settings.toml`:

```toml
[lapce-plugin-magento2-lsp]
"lsp.serverPath" = "/absolute/path/to/magento2-lsp"
```

## Maintainer: Publish to plugins.lapce.dev

Requires a GitHub account + Lapce plugin registry token.

```sh
cargo install volts
# Log in at https://plugins.lapce.dev with GitHub, create an API token,
# then from this repo root:
volts publish
```

`volts publish` reads `volt.toml` + the built `.wasm` and uploads. The `repository` field in `volt.toml` must point at the public GitHub repo for the plugin to be accepted.

## License

MIT.
