# xnuports/pkg-bootstrap

Bootstrap script to install [FreeBSD's `pkg`](https://github.com/freebsd/pkg) package manager on macOS, configured to use [xnuports](https://github.com/xnuports/xnuports) packages.

## What is this?

`pkg-bootstrap` is a single script that:

1. Detects your macOS architecture (arm64 or x86_64)
2. Ensures build dependencies are available (Xcode Command Line Tools, Homebrew libraries)
3. Downloads a pre-built `pkg` binary or builds from source
4. Installs to `/opt/xnuports` by default
5. Configures `pkg` to use the xnuports package repository
6. Sets up your shell environment

## Installation

```bash
curl -fsSL https://raw.githubusercontent.com/xnuports/pkg-bootstrap/main/install | bash
```

Or download and run directly:

```bash
curl -fsSL https://raw.githubusercontent.com/xnuports/pkg-bootstrap/main/install -o install
bash install
```

### Options

```
--prefix=PATH          Install to PATH (default: /opt/xnuports)
--yes, --non-interactive   Install without prompting
--verbose              Enable verbose debug output
--help, -h             Show help message
```

### Environment Variables

```
XNUPORTS_PREFIX    Installation prefix (default: /opt/xnuports)
NONINTERACTIVE     Set to 1 for non-interactive install
XNUPORTS_DEBUG     Set to 1 for verbose debug output
```

## After Installation

Restart your shell or run:

```bash
# Initialize pkg in your current shell
eval "$(/opt/xnuports/bin/pkg shellinit)"

# Update package repository
pkg update

# Install packages
pkg install <package-name>
```

## Directory Layout

```
/opt/xnuports/
├── bin/                    # Executable symlinks
├── sbin/                   # System executable symlinks
├── lib/                    # Library symlinks
├── include/                # Header symlinks
├── share/                  # Shared data symlinks
├── etc/
│   └── pkg/
│       ├── xnuports.conf   # pkg configuration
│       └── repos/
│           └── xnuports.conf  # Repository configuration
├── var/
│   ├── db/
│   │   └── pkg/           # Package database
│   ├── cache/
│   │   └── pkg/           # Package cache
│   └── tmp/                # Build temporary files
├── distfiles/              # Downloaded source archives
├── opt/                    # Installed package files
│   └── <package-name>/
└── xnuports/               # Ports tree (future use)
```

## How It Works

1. **Binary First**: The script attempts to download a pre-built `pkg` binary from the [xnuports/pkg releases](https://github.com/xnuports/pkg/releases).
2. **Source Fallback**: If no binary is available, it builds `pkg` from source using the [xnuports/pkg](https://github.com/xnuports/pkg) fork.
3. **Configuration**: Creates `/opt/xnuports/etc/pkg/` with repository and local configuration.
4. **Shell Integration**: Adds `eval "$(/opt/xnuports/bin/pkg shellinit)"` to your shell RC file.

## Package Format

Packages follow the FreeBSD `pkg` format (`.txz` archives) and install to `/opt/xnuports/opt/<package-name>/`. Symlinks are created in `/opt/xnuports/{bin,sbin,lib,include,share}` to avoid conflicts with system or Homebrew-installed software.

## Repositories

- **xnuports/pkg**: Fork of FreeBSD's pkg package manager with macOS support
- **xnuports/xnuports**: Ports collection (currently empty, under development)

## Uninstall

```bash
curl -fsSL https://raw.githubusercontent.com/xnuports/pkg-bootstrap/main/uninstall | bash
```

Or if you already have it:

```bash
/opt/xnuports/bin/pkg-bootstrap uninstall
```

## Requirements

- macOS 14.0+ (Sonoma or later)
- arm64 (Apple Silicon) or x86_64 (Intel)
- Xcode Command Line Tools (auto-installed if missing)
- curl, tar, xz, git, make, a C compiler

## Related Projects

- [xnuports/pkg](https://github.com/xnuports/pkg) - pkg fork with macOS support
- [xnuports/xnuports](https://github.com/xnuports/xnuports) - Ports collection
- [xnuports/pkg_utils](https://github.com/xnuports/pkg_utils) - Additional pkg utilities

## License

BSD 2-Clause (same as FreeBSD pkg)
