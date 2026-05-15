# pi-lens-perl-config

pi-lens LSP configuration for Perl projects using
[PerlNavigator](https://github.com/bscan/PerlNavigator) and
[PLS](https://github.com/FractalBoy/perl-language-server) —
two complementary LSP servers.

## What it does

Provides a `/skill:perl-lsp-setup` command that tells pi how to create the
`.pi-lens/lsp.json` file in your Perl project, wiring up both servers with
the correct Perl path and include directories.

## Install

```sh
pi install git:github.com/janlimpens/pi-lens-perl-config
```

## Usage

Inside any Perl project:

```
/skill:perl-lsp-setup
```

pi will detect your project structure, create `.pi-lens/lsp.json`, and tell
you to restart pi for the LSP to become active.

## What gets created

The skill creates a `.pi-lens/lsp.json` with both PerlNavigator and PLS,
adapted to your project paths and system Perl.

See the [skill](./skills/perl-lsp-setup/SKILL.md) for the full template.

## Prerequisites

- [PerlNavigator](https://github.com/bscan/PerlNavigator): `npm install -g perlnavigator-server`
- [PLS](https://github.com/FractalBoy/perl-language-server): `cpanm PLS`
- Perl via [perlbrew](https://perlbrew.pl/) (the skill auto-detects the path)
- [pi-lens](https://github.com/earendil-works/pi-lens) extension active in pi

## Feature comparison

| Feature | PerlNavigator | PLS |
|---------|:---:|:---:|
| Document symbols | ✅ | ✅ |
| Diagnostics / syntax check | ❌ | ✅ |
| Linting (perlcritic) | ❌ | ✅ |
| Go to definition | ❌ | ✅ |
| Hover / documentation | ❌ | ✅ |
| Auto-completion | ❌ | ✅ |
| Formatting (perltidy) | ❌ | ✅ |
