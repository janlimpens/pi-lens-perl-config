# perl-lsp-setup

Configure PerlNavigator LSP for the current Perl project via pi-lens.

## Prerequisites

- PerlNavigator must be installed globally via npm:
  ```sh
  npm install -g perlnavigator-server
  ```
  This provides the `perlnavigator` binary.
- Perl must be available via perlbrew. The skill will auto-detect the active
  perlbrew Perl.

## Steps

1. **Check PerlNavigator is installed.** Run:
   ```sh
   which perlnavigator
   ```
   If not found, tell the user to run `npm install -g perlnavigator-server`
   and stop.

2. **Find the Perl path.** Run:
   ```sh
   which perl
   ```
   Use this path's directory (dirname) as `perlPath` in the settings below.
   For perlbrew this is typically `~/perl5/perlbrew/perls/perl-X.Y.Z/bin`.

3. **Determine the PERL5LIB.** Ask the user what include paths their Perl
   project needs, or inspect the project for `cpanfile`, `Makefile.PL`, or
   `dist.ini`. Common defaults:
   - Carton projects: `.:local/lib/perl5`
   - With a vendor lib: append it, e.g. `.:local/lib/perl5:vendor/foo/lib`

4. **Create `.pi-lens/lsp.json`** in the project root with this content,
   adjusting `perlPath` and `PERL5LIB` as discovered above:

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

   The `perlPath` setting tells PerlNavigator which Perl binary to use for
   indexing. Without it, it falls back to the system Perl.

5. **Verify.** Open a `.pm` file and confirm LSP is active (check for
   `documentSymbol` results or diagnostics in pi-lens).

## Notes

- pi-lens reads `.pi-lens/lsp.json` at session start. After creating the file,
  the user must restart pi (`/exit` then `pi`) or `/reload` for it to take
  effect.
- `command` uses `"perlnavigator"` (PATH lookup), which works as long as the
  global npm bin directory is in PATH (`~/.npm-global/bin` or similar).
- If the project has git submodules that provide Perl libs (like
  `vendor/perl-querybuilder`), add them to `PERL5LIB` separated by `:`.
