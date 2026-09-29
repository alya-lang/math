# math

[![CI](https://github.com/alya-lang/math/actions/workflows/ci.yml/badge.svg)](https://github.com/alya-lang/math)
[![License](https://img.shields.io/github/license/alya-lang/math?color=blue&label=License)](LICENSE)
[![Alya](https://img.shields.io/badge/dynamic/toml?url=https%3A%2F%2Fraw.githubusercontent.com%2Falya-lang%2Fmath%2Fmain%2Falya.toml&query=%24.package.alya-version&label=Alya&color=orange&prefix=%3E%3D)](https://github.com/alya-lang/alya)
[![Package Version](https://img.shields.io/badge/dynamic/toml?url=https%3A%2F%2Fraw.githubusercontent.com%2Falya-lang%2Fmath%2Fmain%2Falya.toml&query=%24.package.version&label=Version&color=brightgreen)](alya.toml)

Comprehensive mathematics for Alya: big integers (moved out of `crypto` v0.6.0),
exact rationals, number theory, primes, combinatorics, complex numbers,
matrices, and exact rational polynomials.

Complements `std/math` (trigonometry, statistics, vectors): this package owns
exact and structural mathematics with zero dependencies.

---

## 🌟 Features

- 🧮 **Big Integers**: 30-bit-limb add/sub/mul/divmod/modexp/gcd/pow/lcm for RSA-scale operands
- ➗ **Exact Rationals**: sign + numerator/denominator limbs, always reduced, exact decimal rendering
- 🔢 **Number Theory**: overflow-free isqrt, extended GCD, mulmod/powmod/modinv, totient, bit utils, CRT
- 🔍 **Primes**: Eratosthenes sieve, deterministic 64-bit Miller-Rabin, Pollard Rho factorization, prime generation
- 🎲 **Combinatorics**: exact factorial/permutation/combination/Fibonacci in int and bigint domains
- 🌀 **Complex Numbers**: float complex arithmetic with struct-method ergonomics
- 🧱 **Matrices**: arithmetic, transpose, trace, determinant, rank, inverse, and linear solves
- ⛓️ **Polynomials**: exact rational-coefficient algebra (add/mul/divmod/GCD/eval/derivative)
- ⚡ **Zero dependencies**: 100% pure Alya
- 🧪 **Well Tested**: ground-truth vectors cross-checked with Python (207 assertions)

---

## 📁 Project Architecture

```
math/
├── alya.toml               # Package manifest (v0.3.0)
├── src/
│   ├── lib.alya            # Central public API export facade
│   ├── bigint.alya         # Multi-precision add/sub/mul/divmod/modexp/gcd/pow/lcm
│   ├── rational.alya       # Exact fractions over bigint limbs
│   ├── numtheory.alya      # isqrt, xgcd, mulmod, powmod, modinv, bit utils, CRT
│   ├── primes.alya         # Sieve, Miller-Rabin, Pollard Rho, factor, totient, gen
│   ├── combin.alya         # Factorial, nPr, nCr, Fibonacci (int + bigint)
│   ├── complex.alya        # Float complex numbers
│   ├── matrix.alya         # Dense float matrices
│   └── poly.alya           # Exact rational-coefficient polynomials
├── tests/
│   ├── test_basic.alya     # Bigint ground-truth suite
│   ├── test_numtheory.alya # Number theory suite
│   ├── test_primes.alya    # Prime suite
│   ├── test_combin.alya    # Combinatorics suite
│   ├── test_rational.alya  # Rational suite
│   ├── test_complex.alya   # Complex suite
│   ├── test_matrix.alya    # Matrix suite
│   └── test_poly.alya      # Polynomial suite
└── benches/
    └── bench_basic.alya    # Micro-benchmarks
```

---

## 📦 Installation

Add `math` to your `alya.toml`:

```toml
[dependencies]
math = { git = "https://github.com/alya-lang/math", tag = "v0.3.0" }
```

Or install it directly via CLI:

```bash
alya add math --git https://github.com/alya-lang/math --tag v0.3.0
alya install
```

### Package Features

| Feature | Default | Description |
|:---|:---:|:---|
| `bigint` | ✅ | Arbitrary-precision integers (`bi_*`, needed by `rational`, `combin` big variants, `crypto`, `tls`). |
| `numtheory` | ✅ | Integer number theory (`isqrt`, `modexp`, `totient`, ...). |
| `primes` | ✅ | Primes (`sieve`, `next_prime`, `gen_prime`). |
| `combin` | ✅ | Combinatorics (`fact`, `ncr`, `fib`, big variants need `bigint`). |
| `rational` | ✅ | Exact rationals (`rat_*`, needs `bigint`). |
| `complex` | ✅ | Complex numbers (`cx`, ...). |
| `matrix` | ✅ | Matrices (`mat_*`, solve, det). |
| `poly` | ✅ | Polynomials over rationals (needs `rational`). |

```bash
# Full build (default)
alya install
alya test

# Slim build (pick what you need, e.g. primes only)
alya install --no-default-features
alya test --no-default-features --features primes
```

---

## 🚀 Quick Start

```alya
import "math" as math

function main()
    # Exact rational arithmetic: 1/2 + 1/3 = 5/6.
    let s = math::rat_add(math::rat_from_int(1, 2), math::rat_from_int(1, 3))
    say math::rat_to_string(s)

    # 16-bit prime for key material.
    say math::gen_prime(16)

    # Fibonacci meets RSA scale.
    say math::bi_cmp(math::fib_big(100), [1])
end

main()
```

---

## 📖 API Reference

### Big Integers (`bi_*`, 30-bit little-endian limbs)

| Function | Arguments | Returns | Description |
|---|---|---|---|
| `bi_from_bytes(b)` | `b: list` | `list` | Big-endian bytes to limbs. |
| `bi_to_bytes(a, n)` | `a: list, n: int` | `list` | Limbs to big-endian bytes of exact length `n`. |
| `bi_mask()` | — | `int` | 30-bit limb mask (`2^30 - 1`). |
| `bi_trim(a)` | `a: list` | `list` | Strips leading zero limbs. |
| `bi_cmp(a, b)` | `a: list, b: list` | `int` | -1 when `a < b`, 0 when equal, 1 when `a > b`. |
| `bi_is_zero(a)` | `a: list` | `int` | 1 when zero, else 0. |
| `bi_add(a, b)` | `a: list, b: list` | `list` | Sum limb array. |
| `bi_sub(a, b)` | `a: list, b: list` | `list` | Difference (`a` must be greater or equal). |
| `bi_mul(a, b)` | `a: list, b: list` | `list` | Schoolbook product. |
| `bi_shl(a, l, b)` | `a: list, l: int, b: int` | `list` | Left shift by limbs + bits. |
| `bi_shr_limbs(a, b)` | `a: list, b: int` | `list` | Right shift by 1..15 bits. |
| `bi_divmod(a, b)` | `a: list, b: list` | `list` | Array `[quotient, remainder]`. |
| `bi_mod(a, m)` | `a: list, m: list` | `list` | Remainder limb array. |
| `bi_modexp_int(b, e, m)` | `b: list, e: int, m: list` | `list` | `base^exp mod modulo` with int exponent. |
| `bi_modexp(b, e, m)` | `b: list, e: list, m: list` | `list` | `base^exp mod modulo` with multi-limb exponent. |
| `bi_gcd(a, b)` | `a: list, b: list` | `list` | Greatest common divisor limbs. |
| `bi_lcm(a, b)` | `a: list, b: list` | `list` | Least common multiple limbs. |
| `bi_pow_int(b, e)` | `b: list, e: int` | `list` | `base^exp` limbs. |
| `LIMB_BITS` | — | `int` | Limb width contract (`30`). |

### Rationals (`rat_*`, always reduced)

| Function | Arguments | Returns | Description |
|---|---|---|---|
| `rat_from_int(n, d)` | `n: int, d: int` | `Rat` | Reduced fraction (`d` defaults to 1). |
| `rat_add(a, b)` | `a: Rat, b: Rat` | `Rat` | Sum. |
| `rat_sub(a, b)` | `a: Rat, b: Rat` | `Rat` | Difference. |
| `rat_mul(a, b)` | `a: Rat, b: Rat` | `Rat` | Product. |
| `rat_div(a, b)` | `a: Rat, b: Rat` | `Rat` | Quotient (`0` divisor throws). |
| `rat_neg(r)` | `r: Rat` | `Rat` | Negation. |
| `rat_inv(r)` | `r: Rat` | `Rat` | Reciprocal. |
| `rat_cmp(a, b)` | `a: Rat, b: Rat` | `int` | -1, 0, 1. |
| `rat_to_string(r)` | `r: Rat` | `string` | Exact `"3/4"` rendering. |
| `rat_to_float(r)` | `r: Rat` | `float` | Approximation. |

### Number Theory (int domain)

| Function | Arguments | Returns | Description |
|---|---|---|---|
| `isqrt(n)` | `n: int` | `int` | Floor square root (overflow-free). |
| `xgcd(a, b)` | `a: int, b: int` | `map` | `{g, x, y}` with `a*x + b*y == g`. |
| `mulmod(a, b, m)` | `a: int, b: int, m: int` | `int` | Overflow-safe `(a*b) mod m`. |
| `powmod(b, e, m)` | `b: int, e: int, m: int` | `int` | `(base^exp) mod m`. |
| `modinv(a, m)` | `a: int, m: int` | `int` | Modular inverse, or -1. |
| `bit_len(n)` | `n: int` | `int` | Bit length (`0` for `0`). |
| `popcount(n)` | `n: int` | `int` | Number of set bits. |
| `is_pow2(n)` | `n: int` | `int` | 1 for powers of two. |
| `next_pow2(n)` | `n: int` | `int` | Smallest `2^k >= n`. |
| `crt(a1, m1, a2, m2)` | `a1: int, m1: int, a2: int, m2: int` | `map` | `{ok, x, m}` Chinese Remainder. |

### Primes

| Function | Arguments | Returns | Description |
|---|---|---|---|
| `sieve(n)` | `n: int` | `array` | All primes `<= n`. |
| `is_prime_mr(n)` | `n: int` | `int` | Deterministic Miller-Rabin (64-bit). |
| `next_prime(n)` | `n: int` | `int` | Smallest prime `>= n`. |
| `gen_prime(bits)` | `bits: int` | `int` | Random `bits`-bit prime (2..62). |
| `factor(n)` | `n: int` | `array` | Prime factors ascending (Pollard Rho). |
| `totient(n)` | `n: int` | `int` | Euler `phi(n)`. |

### Combinatorics

| Function | Arguments | Returns | Description |
|---|---|---|---|
| `fact_int(n)` | `n: int` | `int` | `n!` (exact to `20!`). |
| `npr(n, k)` | `n: int, k: int` | `int` | Permutations. |
| `ncr(n, k)` | `n: int, k: int` | `int` | Combinations. |
| `fib(n)` | `n: int` | `int` | Fibonacci (exact to `F(92)`). |
| `fact_big(n)` | `n: int` | `list` | `n!` limbs, unbounded. |
| `ncr_big(n, k)` | `n: int, k: int` | `list` | `C(n, k)` limbs, unbounded. |
| `fib_big(n)` | `n: int` | `list` | `F(n)` limbs, unbounded. |

### Complex (`Complex` struct)

| Function | Description |
|---|---|
| `cx(re, im)` | Constructor. |
| `.add(o)` / `.sub(o)` / `.mul(o)` / `.div(o)` | Arithmetic (`div` by zero throws). |
| `.pow(n)` | Integer power (negative inverts). |
| `.neg()` / `.conj()` | Negation, conjugate. |
| `.abs()` | Magnitude. |
| `.arg()` | Phase in radians. |

### Matrices (`Matrix` struct, row-major floats)

| Function | Description |
|---|---|
| `mat(r, c, data)` | Constructor from ints (`null` for zeros). |
| `mat_f(r, c, data)` | Constructor from floats. |
| `mat_zeros(r, c)` / `mat_identity(n)` / `mat_copy(m)` | Standard constructors. |
| `mat_get(m, r, c)` / `mat_set(m, r, c, v)` | Entry access (`set` in place). |
| `mat_add(a, b)` / `mat_sub(a, b)` / `mat_scale(m, s)` | Element-wise ops. |
| `mat_mul(a, b)` | Matrix product. |
| `mat_transpose(m)` | Transpose. |
| `mat_trace(m)` | Diagonal sum (square only). |
| `mat_frobenius(m)` | `sqrt` of sum of squares. |
| `mat_det(m)` | Determinant (`0.0` when singular). |
| `mat_inv(m)` | `{ok, inv}` (Gauss-Jordan). |
| `mat_solve(a, b)` / `mat_solve_f(a, b)` | `{ok, x}` for int/float right-hand sides. |
| `mat_rank(m)` | Rank (rectangular OK). |

### Polynomials (`pq_*`, exact `Rat` coefficients, ascending)

| Function | Description |
|---|---|
| `pq_from_ints(a)` | Constructor from ascending ints. |
| `pq_trim(p)` / `pq_deg(p)` | Normalization / degree (-1 for zero). |
| `pq_add(a, b)` / `pq_sub(a, b)` / `pq_mul(a, b)` | Exact arithmetic. |
| `pq_divmod(n, d)` | `{q, r, error}` long division. |
| `pq_gcd(a, b)` | Monic Euclid GCD. |
| `pq_eval(p, x)` | Horner evaluation at a `Rat`. |
| `pq_deriv(p)` | Formal derivative. |
| `pq_to_string(p)` | Rendering (`"x^2 - 1"`). |

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
