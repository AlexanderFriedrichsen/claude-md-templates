# Platform Compatibility Rules

<!-- INSTRUCTIONS: Uncomment the section for your primary OS. Delete the others. -->
<!-- If targeting multiple platforms, keep all relevant sections. -->

<!-- ============================================================ -->
<!-- WINDOWS -->
<!-- ============================================================ -->

<!-- ## Windows

### Shell

- Default shell is **PowerShell**. All inline commands and scripts must be PowerShell syntax.
- Use `; ` to chain commands, not `&&`.
- Use `$env:VAR_NAME` for environment variables, not `$VAR_NAME`.
- Use `Write-Host` or `Write-Output`, not `echo` (though `echo` is aliased, be explicit).

### Paths

- Use forward slashes in JS/TS imports and config files — Node handles them fine on Windows.
- Be aware of Windows path length limits (260 chars default). Use short, flat directory structures.
- Use `path.join()` or `path.resolve()` in any Node scripts, never hardcode separators.

### File Operations

- Use `Copy-Item`, `Move-Item`, `Remove-Item` in PowerShell scripts.
- In Node code, use `fs/promises` for async file ops.
- Line endings: configure `.gitattributes` with `* text=auto`. Prettier handles the rest.

### Common Gotchas

- File locking: Windows locks open files more aggressively. Close file handles explicitly.
- Reserved filenames: avoid `CON`, `PRN`, `AUX`, `NUL`, `COM1`-`COM9`, `LPT1`-`LPT9` as file/dir names.
- Some npm packages ship with bash shebangs — they work via `npx` or pnpm scripts but may not run directly in PowerShell. -->

<!-- ============================================================ -->
<!-- macOS -->
<!-- ============================================================ -->

<!-- ## macOS

### Shell

- Default shell is **zsh**. Scripts should use `#!/usr/bin/env bash` or `#!/usr/bin/env zsh`.
- Use `&&` to chain commands.
- Use `$VAR_NAME` or `${VAR_NAME}` for environment variables.

### Paths

- macOS is case-insensitive by default (APFS). Never rely on case differences between file names.
- Use forward slashes in all paths.
- Use `path.join()` or `path.resolve()` in Node scripts.

### File Operations

- `cp`, `mv`, `rm` are standard.
- In Node code, use `fs/promises` for async file ops.

### Common Gotchas

- Xcode Command Line Tools required for many build tools: `xcode-select --install`
- `sed` on macOS (BSD) differs from GNU sed — use `sed -i ''` (empty string backup) instead of `sed -i`.
- Homebrew installs to `/opt/homebrew` on Apple Silicon, `/usr/local` on Intel. -->

<!-- ============================================================ -->
<!-- Linux -->
<!-- ============================================================ -->

<!-- ## Linux

### Shell

- Default shell is **bash**. Scripts should use `#!/usr/bin/env bash`.
- Use `&&` to chain commands.
- Use `$VAR_NAME` or `${VAR_NAME}` for environment variables.

### Paths

- Linux is case-sensitive. `Foo.ts` and `foo.ts` are different files.
- Use forward slashes in all paths.
- Use `path.join()` or `path.resolve()` in Node scripts.

### File Operations

- `cp`, `mv`, `rm` are standard.
- In Node code, use `fs/promises` for async file ops.

### Common Gotchas

- Watch file limits: if you hit `ENOSPC` during `dev`, increase inotify watches: `echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p`
- Some distros don't ship `node` or `npm` — use `nvm` or `fnm` for version management.
- On headless servers, Playwright needs `xvfb` or `--headless` flag. -->
