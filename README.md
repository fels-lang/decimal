# decimal

Fixed-point decimal numbers for [Fels](https://github.com/fels-lang/fels), made for money:
`0.1 + 0.2` is `0.3`, and every rounding is one you chose.

**State:** started for Strata (milestone M1). The code is still the library template.

## Planned

- A decimal value with a fixed number of places (an `i64` count of the smallest unit, or wider
  where needed), so sums of money are exact.
- Add, subtract, multiply and divide, with rounding modes: half up, half even (banker's), down,
  up.
- Parsing from text (`"1234.56"`, `"1.234,56"` with a locale) and formatting with digit grouping
  and a fixed number of places.
- Comparison and checks for overflow.

## Use it

```
fels add decimal github.com/<you>/decimal
```

```fels
#import decimal;
```

## Work on it

```
fels test        run the tests
fels build run   build and run examples/demo.fels
```
