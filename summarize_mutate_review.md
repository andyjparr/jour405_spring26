# `summarize()` and `mutate()`: What They Do and When to Use Each

These two functions are workhorses in tidyverse data analysis, but they do fundamentally different things.

---

## `summarize()` — Collapses rows into a summary

`summarize()` takes many rows and produces a single result (or one result per group when combined with `group_by()`). Think of it as answering: *"What's the overall picture?"*

**Example from HW3 (White House Salaries):**

```r
wh_salaries |> 
  summarize(mean_salary = mean(SALARY),
            median_salary = median(SALARY),
            min_salary = min(SALARY),
            max_salary = max(SALARY))
```

This turns hundreds of rows into a single row showing four numbers. You use it when you want to *describe* the dataset as a whole — not keep every row.

**Example from HW8 (M&Ms):**

```r
sample5 |> 
  summarize(mean_red = mean(red), sd_red = sd(red))
```

Same idea — many bags of M&Ms, one summary row.

**Paired with `group_by()`**, it produces one summary row per group:

```r
accidents |>
  group_by(day_of_week) |>
  summarize(avg_accidents = mean(total),
            median_accidents = median(total))
```

Now you get one row per day of the week — useful for spotting patterns across categories.

---

## `mutate()` — Adds new columns, keeps all rows

`mutate()` adds a new column to your existing dataframe without collapsing anything. Every row stays; you're just adding information to each one. Think of it as answering: *"What's true about each individual case?"*

**Example from HW4 (Maryland City Crime):**

```r
md_cities_rates <- md_cities |> 
  mutate(violent_rate_2019 = (violent2019 / pop2019) * 1000,
         property_rate_2019 = (property2019 / pop2019) * 1000)
```

Every city keeps its row. You're just adding rate columns alongside the raw counts. This is the *rates over raw counts* lesson in action — you can't fairly compare Baltimore to Ocean City using raw numbers.

**Example from UMD Fees:**

```r
umd_fees_pct <- umd_fees |> 
  mutate(pct_change = (`Fall 2024` - `Fall 2021`) / `Fall 2021` * 100)
```

Each fee category gets a new column showing how much it changed. You still have all the original rows.

---

## The Key Distinction

|                    | `mutate()`                              | `summarize()`                            |
|--------------------|------------------------------------------|------------------------------------------|
| Rows in output     | Same as input                            | Fewer (one per group, or just one)       |
| Purpose            | Add a calculated column                  | Produce a summary statistic              |
| Ask yourself       | "What should I know about each row?"     | "What's the overall picture?"            |

A common mistake: using `summarize()` when you want to keep your data intact for further analysis (use `mutate()` instead), or using `mutate()` when you just want a single number (use `summarize()`).

---

## Journalism Tip

`mutate()` is your tool for creating the rates and percentages that make comparisons fair. `summarize()` is your tool for finding the numbers that lead your story — the mean, the median, the range. You'll almost always use both in the same analysis.
