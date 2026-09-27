# math

[![CI](https://github.com/alya-lang/math/actions/workflows/ci.yml/badge.svg)](https://github.com/alya-lang/math)
[![License](https://img.shields.io/github/license/alya-lang/math?color=blue&label=License)](LICENSE)
[![Alya](https://img.shields.io/badge/dynamic/toml?url=https%3A%2F%2Fraw.githubusercontent.com%2Falya-lang%2Fmath%2Fmain%2Falya.toml&query=%24.package.alya-version&label=Alya&color=orange&prefix=%3E%3D)](https://github.com/alya-lang/alya)
[![Package Version](https://img.shields.io/badge/dynamic/toml?url=https%3A%2F%2Fraw.githubusercontent.com%2Falya-lang%2Fmath%2Fmain%2Falya.toml&query=%24.package.version&label=Version&color=brightgreen)](alya.toml)

Multi-precision integer arithmetic for Alya (moved out of `crypto` v0.6.0).

---

## 🌟 Features

- 🧮 **Big Integers**: 30-bit-limb add/sub/mul/divmod/modexp for RSA-scale operands
- ⚡ **Zero dependencies**: 100% pure Alya, exact 64-bit signed intermediates
- 🧪 **Well Tested**: ground-truth vectors cross-checked with Python

---

## 📁 Project Architecture

```
math/
├── alya.toml               # Package manifest (v0.1.0)
├── src/
│   ├── lib.alya            # Central public API export facade
│   └── bigint.alya         # Multi-precision add/sub/mul/divmod/modexp
├── tests/
│   └── test_basic.alya     # Ground-truth test suite
└── benches/
    └── bench_basic.alya    # Micro-benchmarks
```

---

## 📦 Installation

Add `math` to your `alya.toml`:

```toml
[dependencies]
math = { git = "https://github.com/alya-lang/math", tag = "v0.1.0" }
```

Or install it directly via CLI:

```bash
alya add math --git https://github.com/alya-lang/math --tag v0.1.0
alya install
```

---

## 🚀 Quick Start

```alya
import "math" as math

function main()
    let p = math::bi_from_bytes([1, 2, 3])
    let q = math::bi_from_bytes([4, 5, 6])
    let sum = math::bi_add(p, q)
    say math::bi_cmp(sum, p) # 1
end

main()
```

---

## 📖 API Reference

| Function | Arguments | Returns | Description |
|---|---|---|---|
| `bi_from_bytes(b)` | `b: list` | `list` | Big-endian bytes to little-endian 30-bit limbs. |
| `bi_to_bytes(a, n)` | `a: list, n: int` | `list` | Limbs to big-endian bytes of exact length `n`. |
| `bi_cmp(a, b)` | `a: list, b: list` | `int` | -1 when `a < b`, 0 when equal, 1 when `a > b`. |
| `bi_add(a, b)` | `a: list, b: list` | `list` | Sum limb array. |
| `bi_sub(a, b)` | `a: list, b: list` | `list` | Difference (`a` must be greater or equal). |
| `bi_mul(a, b)` | `a: list, b: list` | `list` | Schoolbook product. |
| `bi_divmod(a, b)` | `a: list, b: list` | `list` | Array `[quotient, remainder]`. |
| `bi_mod(a, m)` | `a: list, m: list` | `list` | Remainder limb array. |
| `bi_modexp_int(b, e, m)` | `b: list, e: int, m: list` | `list` | `base^exp mod modulo` with int exponent. |
| `bi_modexp(b, e, m)` | `b: list, e: list, m: list` | `list` | `base^exp mod modulo` with multi-limb exponent. |

---

## 🧪 Running Tests & Benchmarks

```bash
alya test
```

Check code formatting:

```bash
alya fmt . --check
```

Run static code linter:

```bash
alya lint . --check
```

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository and clone it locally
2. Install dependencies:
   ```bash
   alya install
   ```
3. Create your feature branch (`git checkout -b feature/my-feature`)
4. Verify tests and formatting before opening a PR:
   ```bash
   alya test
   ```
5. Commit your changes (`git commit -m "feat: add feature"`) and open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
