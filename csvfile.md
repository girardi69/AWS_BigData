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

method 1  
```python
import csv

path = "D:/google_stock_data.csv"
file = open(path, newline='')
reader = csv.reader(file)

header = next(reader) # The first line is the header

data = [row for row in reader] #Read the remaining data
print(header)
print(data[0])
```
this method1 has the limitation that the data are loaded as strings.  
A better method will load the values with correct formation, we will need more libraries.  

```python
import csv
from datetime import datetime

path = "D:/google_stock_data.csv"
file = open(path, newline='')
reader = csv.reader(file)

header = next(reader) # The first line is the header

data = []
for row in reader:
  # row = [Date, Open, High, Low, Close, Volume, Adj. Close]
  date = datetime.strptime(row[0], '%m%d%Y')
  open_price = float(row[1]) # 'open' is a builtin function
  high = float(row[2])
  low = float(row[3])
  close = float(row[4])
  volume = int(row[5])
  adj_close = float(row[6])
  
  data.append([date, open_price, high, low, close, volume, adj_close)]
  
print(data[0])
