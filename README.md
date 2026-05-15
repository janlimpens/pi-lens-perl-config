# pi-lens-perl-config

pi-lens LSP configuration for Perl projects using [PerlNavigator](https://github.com/bscan/PerlNavigator).

## What it does

Provides a `/skill:perl-lsp-setup` command that tells pi how to create the
`.pi-lens/lsp.json` file in your Perl project, wiring up PerlNavigator with
the correct include paths.

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

```json
{
  "servers": {
    "perlnavigator": {
      "name": "PerlNavigator",
      "extensions": [".pl", ".pm", ".t", ".psgi"],
      "command": "perlnavigator",
      "args": ["--stdio"],
      "rootMarkers": ["cpanfile", "Makefile.PL", "dist.ini"],
      "settings": {
        "perlnavigator.perlPath": "/home/jan/perl5/perlbrew/perls/perl-5.42.1/bin"
      },
      "env": {
        "PERL5LIB": ".:local/lib/perl5"
      }
    }
  }
}
```

The skill adapts `perlPath` and `PERL5LIB` to your specific project and system.

## Prerequisites

- [PerlNavigator](https://github.com/bscan/PerlNavigator) installed globally:
  `npm install -g perlnavigator-server`
- Perl via [perlbrew](https://perlbrew.pl/) (the skill auto-detects the path)
- [pi-lens](https://github.com/earendil-works/pi-lens) extension active in
  pi
