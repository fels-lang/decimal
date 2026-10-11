# decimal

Fixed-point decimal numbers for [Fels](https://github.com/fels-lang/fels), made for money:
`0.1 + 0.2` is `0.3`, and every rounding is one you chose.

```fels
#import decimal;

price := decimal::parse("19.99") ?? decimal::zero();
net   := price.mul(decimal::from_int(3i64), 2u8, .half_even);          // 59.97
vat   := net.mul(decimal::parse("0.19") ?? decimal::zero(), 2u8, .half_even);   // 11.39
total := net.add(vat);                                                  // 71.36
```

## Use it

```
fels add decimal github.com/fels-lang/decimal
```

## The rules

- A `Decimal` is a whole number of units and a scale: 12.34 is 1234 units at scale 2. The
  scale is 0 to 18 digits after the point; the units are an `i64`.
- `add`, `sub`, `cmp` and `neg` are exact. The result of `add` and `sub` has the larger of the
  two scales.
- `mul`, `div` and `round` take the scale of the result and a `Rounding`. They compute the
  exact value (with 128 bits on the way) and round it once, so `mul` then `round` never rounds
  twice.
- A result that does not fit panics with `decimal: overflow`, and dividing by zero panics: a
  wrong amount must never pass silently.
- `a.eq(b)` compares values (1.50 equals 1.5); `==` compares units and scale.
- `to_f64` is for charts and statistics, never for amounts that are booked.

| Rounding | 2.345 to 2 digits | -2.345 to 2 digits |
| --- | --- | --- |
| `half_even` (banker's) | 2.34 | -2.34 |
| `half_up` | 2.35 | -2.35 |
| `down` (toward zero) | 2.34 | -2.34 |
| `up` (away from zero) | 2.35 | -2.35 |
| `floor` | 2.34 | -2.35 |
| `ceiling` | 2.35 | -2.34 |

## The API

| Function | What it does |
| --- | --- |
| `of(units, scale)`, `from_int(n)`, `zero()`, `one()` | make a value |
| `parse(text)` | `"-1234.56"`; null when it is no number or does not fit |
| `parse_with(text, point, group)` | `parse_with("1.234,56", ',', '.')` |
| `format(d, buf)` | `"-1234.56"` into `buf`, all digits of the scale |
| `format_with(d, buf, point, group)` | `"1.234,56"`; `group` 0 for none |
| `d.add(x)`, `d.sub(x)`, `d.neg()`, `d.abs()` | exact |
| `d.mul(x, scale, mode)`, `d.div(x, scale, mode)`, `d.round(scale, mode)` | rounded once |
| `d.cmp(x)`, `d.eq(x)`, `d.lt(x)`, `d.le(x)`, `d.gt(x)`, `d.ge(x)` | by value |
| `d.sign()`, `d.is_zero()`, `d.to_f64()` | |

## Work on it

```
fels test        run the tests (src/decimal_tests.fels)
fels build run   build and run examples/demo.fels
```

The tests live in their own file, so a program that imports `decimal` does not run them when
it builds.
