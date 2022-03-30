# 4orth is a Porth to Wasm compiler with Wasm-4 bindings
## Quick Start

You can download the latest [release](https://github.com/FrankWPA/4orth/releases) or [bootstrap](https://github.com/FrankWPA/4orth#bootstrapping) it yourself.

### Bootstrapping

Since Porth is self-hosted you need to bootstrap it first. Follow [Porth](https://gitlab.com/tsoding/porth) bootstrapping instructions on how to do that.

Secondly you will need to install:
- [WASM-4](https://wasm4.org/)
- [WABT](https://github.com/WebAssembly/wabt)
- [Binaryen](https://github.com/WebAssembly/binaryen) (optional, for optimized .wasm files)

```console
$ ./porth com 4orth.porth
```

### Compilation

Compilation generates [WAT](https://developer.mozilla.org/en-US/docs/WebAssembly/Understanding_the_text_format) and converts it to a [WebAssembly Binary Format](https://webassembly.github.io/spec/core/binary/index.html) .wasm file with [WABT](https://developer.mozilla.org/en-US/docs/WebAssembly/Text_format_to_wasm). So make sure you have it available in your `$PATH`.

```console
$ ./4orth com game.porth
...
... compilation logs ...
...
$ w4 run game.wasm
        <or>
$ w4 bundle --linux game game.wasm
$ ./game
```

Or you can use the subcommands `-b` and `-r` to bundle and run after the compilation. (As porth only supports Linux, `-b` creates a Linux executable. For other options, check `w4 bundle --help` or [Wasm-4](https://wasm4.org/docs/guides/distribution) distribution docs)

```console
$ ./4orth com -b -r game.porth
```

Use `-opt` to benefit from [binaryen](https://github.com/WebAssembly/binaryen) optimization. Use `4orth help` to view all other available subcommands.

### Status

4orth is not feature complete with the current open version of Porth and Wasm-4 yet. The current set of features not supported consists of:

- Int64
- Floats
- Negative numbers

Porth operations:
- OP_IFSTAR
- OP_BIND_LET
- OP_BIND_PEEK
- OP_PUSH_BIND
- OP_UNBIND
- OP_PUSH_ADDR
- OP_CALL_LIKE

### Changes

4orth implements some features not available in Porth to facilitate Wasm and Wasm-4 integration:

- Xor Intrinsic
- Hexadecimal numbers
- Bit shift support in const evaluation
- Null terinated string support in const evaluation
- Baked in random number generator (will be removed once there is float and int64 support).
```porth
inline proc rnd-coord -- int int in 20 rnd 20 rnd end
```
This inline proc for example returns two ints between 0 and 20.

- Import proc
```porth
import proc trace ptr in end
```
This adds the ability to call the wasm-4 `trace` function via the defined proc contract. (Porth's `print` intrinsic calls this imported proc, you can use either of them to log to the console)
Imported procs must have an empty body and no return type.

All available [functions](https://wasm4.org/docs/reference/functions), constants and the [memory map](https://wasm4.org/docs/reference/memory) from Wasm-4 are in the [wasm4.porth](./wasm4.porth) library.