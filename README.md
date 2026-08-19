# xnuports/pkg-bootstrap

Bootstrap script to install [FreeBSD's `pkg`](https://github.com/freebsd/pkg) package manager on macOS, configured to use [xnuports](https://github.com/xnuports/xnuports) packages.

## What is this?

`pkg-bootstrap` is a single script that:

1. Detects your macOS architecture (arm64 or x86_64)
2. Downloads pre-built `pkg` and `pkg_utils` binaries from GitHub Releases
3. Installs to `/opt/xnuports` by default
4. Configures `pkg` to use the xnuports package repository
5. Sets up your shell environment

## Installation

```bash
curl -fsSL https://raw.githubusercontent.com/xnuports/pkg-bootstrap/master/install | bash
```

Or download and run directly:

```bash
curl -fsSL https://raw.githubusercontent.com/xnuports/pkg-bootstrap/master/install -o install
bash install
```

### Options

```
--prefix=PATH          Install to PATH (default: /opt/xnuports)
--yes, --non-interactive   Install without prompting
--verbose              Enable verbose debug output
--build-from-source    Build pkg and pkg_utils from source instead of downloading binaries
--help, -h             Show this help message
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
source /opt/xnuports/etc/pkg/env.sh

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

1. **Binary First**: The script downloads pre-built `pkg` and `pkg_utils` binaries from GitHub Releases.
2. **Source Build (opt-in)**: Pass `--build-from-source` to compile locally if binaries are not yet available for your platform.
3. **Configuration**: Creates `/opt/xnuports/etc/pkg/` with repository and local configuration.
4. **Shell Integration**: Adds `source /opt/xnuports/etc/pkg/env.sh` to your shell RC file.

## Package Format

Packages follow the FreeBSD `pkg` format (`.txz` archives) and install to `/opt/xnuports/opt/<package-name>/`. Symlinks are created in `/opt/xnuports/{bin,sbin,lib,include,share}` to avoid conflicts with system or Homebrew-installed software.

## Repositories

- **xnuports/pkg**: Fork of FreeBSD's pkg package manager with macOS support
- **xnuports/xnuports**: Ports collection (currently empty, under development)

## Uninstall

```bash
curl -fsSL https://raw.githubusercontent.com/xnuports/pkg-bootstrap/master/uninstall | bash
```

Or if you already have it:

```bash
/opt/xnuports/bin/pkg-bootstrap uninstall
```

## pkg_utils

The following utilities are installed alongside `pkg`:

- **pkg_cutleaves** - Remove leaf packages
- **pkgs_which** - Find which package owns a file
- **pkg_cleanup** - Clean up stale packages (requires dialog/ncurses)
- **pkg-rmleaf** - Remove leaf packages
- **pkg_tree** - Display package dependency tree
- **sign_pkg** - Sign packages
- **pkg_aspcud** - ASP solver for package upgrades

## Requirements

- macOS 14.0+ (Sonoma or later)
- arm64 (Apple Silicon) or x86_64 (Intel)
- curl, tar, xz, git

For `--build-from-source`:
- Xcode Command Line Tools
- make, a C compiler (cc or clang)
- Homebrew with libarchive and openssl

## Related Projects

- [xnuports/pkg](https://github.com/xnuports/pkg) - pkg fork with macOS support
- [xnuports/xnuports](https://github.com/xnuports/xnuports) - Ports collection
- [xnuports/pkg_utils](https://github.com/xnuports/pkg_utils) - Additional pkg utilities

## License

BSD 2-Clause (same as FreeBSD pkg)
