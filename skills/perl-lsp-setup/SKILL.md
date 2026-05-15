---
name: perl-lsp-setup
description: "Configure Perl LSP servers (PerlNavigator + PLS) for the current Perl project via pi-lens."
---

# perl-lsp-setup

Configure Perl LSP servers for the current Perl project via pi-lens.
Sets up both **PerlNavigator** and **PLS** (Perl Language Server) —
they complement each other.

## Prerequisites

- **PerlNavigator** installed globally:
  ```sh
  npm install -g perlnavigator-server
  ```
- **PLS** installed from CPAN:
  ```sh
  cpanm PLS
  ```
- Perl via perlbrew (the skill auto-detects the active perlbrew Perl)

## Steps

1. **Check servers are installed.** Run:
   ```sh
   which perlnavigator && which pls
   ```
   If either is missing, tell the user to install it and stop.

2. **Find the Perl path.** Run:
   ```sh
   which perl
   ```
   Use this path's directory (dirname) as `perlPath` / `syntax.perl`.

3. **Determine include paths.** Ask the user or inspect the project for
   `cpanfile`, `Makefile.PL`, or `dist.ini`. Common defaults:
   - Carton projects: `.`, `local/lib/perl5`
   - With vendor libs: add them, e.g. `vendor/foo/lib`

4. **Create `.pi-lens/lsp.json`** with both servers:

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
       },
       "pls": {
         "name": "Perl Language Server",
         "extensions": [".pl", ".pm", ".t", ".psgi"],
         "command": "pls",
         "args": ["--stdio"],
         "rootMarkers": ["cpanfile", "Makefile.PL", "dist.ini"],
         "settings": {
           "pls": {
             "inc": [".", "local/lib/perl5"],
             "syntax": {
               "enabled": true,
               "perl": "/home/jan/perl5/perlbrew/perls/perl-5.42.1/bin/perl"
             },
             "perlcritic": { "enabled": false },
             "perltidy": {}
           }
         }
       }
     }
   }
   ```

   Adjust `perlPath`, `syntax.perl`, and `inc` paths for the project.
   The `command` for `pls` defaults to `"pls"` (PATH lookup). If the user
   installed PLS via cpanm with local::lib, it may be at `~/perl5/bin/pls`.

5. **Verify.** After restarting pi, open a `.pm` file and check for
   diagnostics, hover, or document symbols.

## What each server provides

| Feature | PerlNavigator | PLS |
|---------|:---:|:---:|
| Document symbols | ✅ | ✅ |
| Diagnostics / syntax check | ❌ | ✅ |
| Linting (perlcritic) | ❌ | ✅ |
| Go to definition | ❌ | ✅ |
| Hover / documentation | ❌ | ✅ |
| Auto-completion | ❌ | ✅ |
| Signature help | ❌ | ✅ |
| Formatting (perltidy) | ❌ | ✅ |
| References | ❌ | ❌ |

## Notes

- pi-lens reads `.pi-lens/lsp.json` at session start. After creating the file,
  restart pi (`/exit` then `pi`) or `/reload`.
- To enable perlcritic linting, set `pls.perlcritic.enabled` to `true` and
  optionally configure `perlcriticrc`.
- For perltidy formatting, set `pls.perltidy.perltidyrc` to the path of your
  `.perltidyrc` file.
- PLS uses `.plsignore` files in the workspace root to exclude files from
  indexing (Perl glob patterns).
