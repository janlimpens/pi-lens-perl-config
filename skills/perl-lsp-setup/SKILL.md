# perl-lsp-setup

Configure PerlNavigator LSP for the current Perl project via pi-lens.

## Prerequisites

- PerlNavigator must be installed. The standard pi-lens wrapper is at
  `~/.pi-lens/bin/perlnavigator-pi`. If missing, install it first.
- The project must have a Perl module include path. For Carton-based projects
  this is `local/lib/perl5`. For plain projects the `PERL5LIB` in `env` below
  may need adjustment.

## Steps

1. **Check PerlNavigator is installed.** Run:
   ```sh
   ls ~/.pi-lens/bin/perlnavigator-pi || which perlnavigator-pi
   ```
   If neither works, tell the user to install PerlNavigator first and stop.

2. **Determine the PERL5LIB.** Ask the user what include paths their Perl
   project needs, or inspect the project for `cpanfile`, `Makefile.PL`, or
   `dist.ini`. Common defaults:
   - Carton projects: `.:local/lib/perl5`
   - With a vendor lib: append it, e.g. `.:local/lib/perl5:vendor/foo/lib`

3. **Create `.pi-lens/lsp.json`** in the project root with this content,
   adjusting `command` and `env.PERL5LIB` as discovered above:

   ```json
   {
     "servers": {
       "perlnavigator": {
         "name": "PerlNavigator",
         "extensions": [".pl", ".pm", ".t", ".psgi"],
         "command": "perlnavigator-pi",
         "args": ["--stdio"],
         "rootMarkers": ["cpanfile", "Makefile.PL", "dist.ini"],
         "env": {
           "PERL5LIB": ".:local/lib/perl5"
         }
       }
     }
   }
   ```

   Prefer `"command": "perlnavigator-pi"` (PATH lookup) over an absolute path,
   unless the user confirms a different location.

4. **Verify.** Open a `.pm` file and confirm LSP is active (check for
   `documentSymbol` results or diagnostics in pi-lens).

## Notes

- pi-lens reads `.pi-lens/lsp.json` at session start. After creating the file,
  the user must restart pi (`/exit` then `pi`) or `/reload` for it to take
  effect.
- For projects using `carton`, the `PERL5LIB` should include
  `.:local/lib/perl5`. For plain Perl projects it may just be `.`.
- If the project has git submodules that provide Perl libs (like
  `vendor/perl-querybuilder`), add them to `PERL5LIB` separated by `:`.
