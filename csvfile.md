- download the file from https://goo.gl/3zaUlD

```python
path = "D:/google_stock_data.csv"
file = open(path)
lines = [line for line in open(path)]
lines[0]
lines[1]
lines[0].strip()
lines[0].strip().split(',')
dataset = [line.strip().split(',') for line in open(path)]
dataset[0]
```
This example shows the limitation of using python without the csv module  
  
```python
import csv
print(dir(csv))
```

```python
import csv
path = "D:/google_stock_data.csv"
file = open(path, newline='')

