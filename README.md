# homebrew-pdfium-tap

Homebrew tap for [raphaelmansuy/edgequake-pdf2md](https://github.com/raphaelmansuy/edgequake-pdf2md).

This tap provides two formulas:

| Formula | Description |
|---------|-------------|
| `pdf2md` | PDF to Markdown converter using Vision LLMs (self-contained binary with pdfium embedded) |
| `pdfium` | Prebuilt pdfium native libraries from `bblanchon/pdfium-binaries` |

Repository: https://github.com/raphaelmansuy/homebrew-pdfium-tap

## Quick install - pdf2md (recommended)

```bash
brew tap raphaelmansuy/homebrew-pdfium-tap
brew install pdf2md
```

Then use it:

```bash
# Convert a PDF to Markdown (requires an LLM provider API key)
pdf2md convert document.pdf --provider openai --output document.md

# Inspect PDF metadata without calling an LLM
pdf2md --inspect-only document.pdf
```

See `pdf2md --help` for the full list of options and supported LLM providers.

## Quick install - pdfium (prebuilt native library)

```bash
brew tap raphaelmansuy/homebrew-pdfium-tap
brew install pdfium
```

After install, add the native library to your search path:

```bash
# macOS
export DYLD_LIBRARY_PATH="$(brew --prefix pdfium)/lib:$DYLD_LIBRARY_PATH"

# Linux
export LD_LIBRARY_PATH="$(brew --prefix pdfium)/lib:$LD_LIBRARY_PATH"
```

## Formula details

### pdf2md

- Version: 0.4.0 (edgequake-pdf2md)
- Pdfium version: chromium/7690
- The binary is **fully self-contained**: pdfium is embedded at build time, no shared library needed at runtime.
- The formula stages the correct prebuilt pdfium library via a `resource` block so `brew install` works without network access during the cargo build step.
- Supported platforms: macOS arm64, macOS x86_64, Linux x86_64, Linux arm64

### pdfium

- Version: 7690 (chromium/7690)
- Installs `libpdfium.dylib` (macOS) or `libpdfium.so` (Linux) into the formula prefix.

## Updating

### Update pdf2md to a new version

Update `url` and `sha256` in `Formula/pdf2md.rb`:

```bash
curl -sL https://github.com/raphaelmansuy/edgequake-pdf2md/archive/refs/tags/vX.Y.Z.tar.gz | shasum -a 256
```

Paste the hash into the formula, then commit and push.

### Update pdfium to a new chromium build

Use the helper scripts in `release/`:

```bash
release/check_assets.sh Formula/pdfium.rb
release/generate-sha256.sh <asset-url> --apply
```

## Contributing

Open an issue or pull request against this repository.

License: MIT
