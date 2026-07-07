# qubes-gentoo

A Gentoo **ebuild overlay** (Portage repository) providing the Qubes OS
integration packages needed to build and run a Gentoo TemplateVM on
[Qubes OS](https://www.qubes-os.org).

> **Status:** early / experimental. Targets Qubes **R4.3**.

## What this is

This is a [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository)
— **not** a template image and not a build system. It holds the `*.ebuild`
files (and their profiles/metadata) for the Qubes guest components that a Gentoo
template needs, e.g.:

- `qubes-core-agent` and networking
- the GUI agent
- Qubes utilities and helpers (split-GPG, file/PDF converters, etc.)

The companion repo [qubes-gentoo-template](https://github.com/slchris/qubes-gentoo-template)
consumes this overlay to actually build the template.

## Why it exists

Upstream [QubesOS/qubes-gentoo](https://github.com/QubesOS/qubes-gentoo) has no
`release4.3` branch and has not been updated since 2023, and the official Gentoo
build scripts are pinned to the (now deprecated) 17.1 Portage profile. This
overlay is a self-maintained, R4.3-oriented fork so the Gentoo template stays
buildable without waiting on stale upstream.

## Layout

```
metadata/
  layout.conf          # masters = gentoo, profile format, etc.
profiles/
  repo_name            # repository id: qubes
  categories           # ebuild categories provided here
app-emulation/
  qubes-core-agent/    # <category>/<package>/<package>-<version>.ebuild
  ...
```

## Usage

Register the overlay with Portage (inside a Gentoo build chroot or template):

```sh
cat > /etc/portage/repos.conf/qubes.conf <<'EOF'
[qubes]
location = /var/db/repos/qubes
sync-type = git
sync-uri = https://github.com/slchris/qubes-gentoo.git
masters = gentoo
auto-sync = true
EOF

emaint sync -r qubes
```

Then the Qubes packages become available to `emerge` (e.g.
`emerge app-emulation/qubes-core-agent`).

## Related

- [qubes-gentoo-template](https://github.com/slchris/qubes-gentoo-template) — builds the template using this overlay
- [qubes-salt-config](https://github.com/slchris/qubes-salt-config) — Salt formulas that consume the resulting template (`salt/gentoo`, `salt/templates/gentoo-dev`)

## License

SPDX-License-Identifier: MIT
