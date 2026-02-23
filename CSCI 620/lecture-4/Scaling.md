

| Name               | Formula                                     | Notes                                          |
| ------------------ | ------------------------------------------- | ---------------------------------------------- |
| Min-max            | $$x'=\frac{x-min(x)}{max(x)-min(x)}$$       | The range of numbers will be 0 to 1            |
| Mean               | $$x'=\frac{x-average(x)}{max(x)-min(x)}$$   | The range of numbers can be negative           |
| Z-score (standard) | $$z=\frac{x-\mu}{\sigma}$$                  |                                                |
| Z-score (square)   | $$z^2=\frac{(x-\mu)^2}{Var(X)}$$            |                                                |
| Robust             | $$x'=\frac{x-Q_{2}(x)}{Q_{3}(x)-Q_{1}(x)}$$ | Don't know why its robust but it is what it is |

### Precisions

SQL should use Decimal(32,24)?
Mongo will just use Decimal casting

Some function no matter what we do, will use double precision

