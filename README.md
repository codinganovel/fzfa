# fzfa - fzf with AutoCD Integration

This is a fork of [fzf](https://github.com/junegunn/fzf) with integrated directory changing functionality using [autocd-go](https://github.com/codinganovel/autocd-go).

## What's Different

This fork adds a single flag: `--autocd` (or `-a`)

When using `--autocd`, instead of printing the selected path to stdout, fzfa will:
- Change to the selected directory (if it's a directory)
- Change to the parent directory (if it's a file)
- Replace the current shell process so you end up in the target directory

## Usage

```bash
# Normal fzfa behavior - prints path
find . -type d | fzfa

# AutoCD behavior - changes to selected directory
find . -type d | fzfa --autocd
```

## Important Notes

**⚠️ Output is not pipeable when using `--autocd`**

The `--autocd` flag fundamentally changes fzfa's behavior. Instead of outputting text that can be piped or captured, it replaces the current process with a shell in the target directory. This means:

- ✅ `fzfa --autocd` - Works, changes to selected directory
- ❌ `fzfa --autocd | other-command` - Won't work, no output to pipe
- ❌ `result=$(fzfa --autocd)` - Won't work, no output to capture

## Implementation

The integration required only **18 lines of code** across 2 files:
- Added autocd-go dependency
- Added `--autocd` flag parsing  
- Replaced the output printer with autocd functionality when flag is enabled

## Maintenance Status

**This is not an actively maintained fork.** I created this as a proof-of-concept integration.

However, the integration is extremely straightforward and should be easily portable to any future version of fzf. The changes are minimal and isolated:

1. Add dependency to `go.mod`
2. Add boolean flag to Options struct  
3. Add command line parsing case
4. Override the printer function when flag is enabled

If you want to apply this to a newer version of fzf, simply locate where fzf prints its final output (the `opts.Printer` function) and replace it with the autocd call when the flag is enabled.

---

For the original fzf documentation, see [fzf-readme.md](fzf-readme.md).