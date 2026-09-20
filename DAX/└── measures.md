# DAX Measures

## Job Market Measures

### Total Jobs

```DAX
Total Jobs =
COUNTROWS(Jobs)
```

### Total Companies

```DAX
Total Companies =
DISTINCTCOUNT(Jobs[company_name])
```

### Total Cities

```DAX
Total Cities =
DISTINCTCOUNT(Jobs[primary_city])
```

---

## Salary Measures

### Average Salary

```DAX
Average Salary =
CALCULATE(
    AVERAGE(Jobs[salary_midpoint_lpa]),
    Jobs[salary_disclosed] = TRUE()
)
```

### Maximum Salary

```DAX
Maximum Salary =
CALCULATE(
    MAX(Jobs[salary_max_lpa]),
    Jobs[salary_disclosed] = TRUE()
)
```

### Minimum Salary

```DAX
Minimum Salary =
CALCULATE(
    MIN(Jobs[salary_min_lpa]),
    Jobs[salary_disclosed] = TRUE()
)
```

### Salary Disclosure %

```DAX
Salary Disclosure % =
DIVIDE(
    CALCULATE(
        [Total Jobs],
        Jobs[salary_disclosed] = TRUE()
    ),
    [Total Jobs],
    0
)
```

---

## Fresher Measures

### Fresher Jobs

```DAX
Fresher Jobs =
CALCULATE(
    [Total Jobs],
    Jobs[is_fresher_friendly] = TRUE()
)
```

### Fresher Job %

```DAX
Fresher Job % =
DIVIDE(
    [Fresher Jobs],
    [Total Jobs],
    0
)
```

### Fresher Jobs With Salary

```DAX
Fresher Jobs With Salary =
CALCULATE(
    [Total Jobs],
    Jobs[is_fresher_friendly] = TRUE(),
    Jobs[salary_disclosed] = TRUE()
)
```

### Fresher Salary Disclosure %

```DAX
Fresher Salary Disclosure % =
DIVIDE(
    [Fresher Jobs With Salary],
    [Fresher Jobs],
    0
)
```

---

## Experience Measures

### Average Experience

```DAX
Average Experience =
AVERAGEX(
    Jobs,
    DIVIDE(
        Jobs[experience_min_yrs] +
        Jobs[experience_max_yrs],
        2
    )
)
```

### Senior Jobs

```DAX
Senior Jobs =
CALCULATE(
    [Total Jobs],
    Jobs[is_senior] = TRUE()
)
```

### Senior Job %

```DAX
Senior Job % =
DIVIDE(
    [Senior Jobs],
    [Total Jobs],
    0
)
```
