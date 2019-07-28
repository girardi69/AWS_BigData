## create EC2 Instance

- sudo yum install aws-kinesis-agent
- wget http://media.sundog-soft.com/AWSBigData/LogGenerator.zip
- unzip LogGenerator.zip

## Look into the files
OnlineRetail.csv is a .csv file with xxx lines
```
InvoiceNo,StockCode,Description,Quantity,InvoiceDate,UnitPrice,CustomerID,Country  
536365,85123A,WHITE HANGING HEART T-LIGHT HOLDER,6,12/1/2010 8:26,2.55,17850,United Kingdom  
536365,71053,WHITE METAL LANTERN,6,12/1/2010 8:26,3.39,17850,United Kingdom  
536365,84406B,CREAM CUPID HEARTS COAT HANGER,8,12/1/2010 8:26,2.75,17850,United Kingdom  
536365,84029G,KNITTED UNION FLAG HOT WATER BOTTLE,6,12/1/2010 8:26,3.39,17850,United Kingdom  
536365,84029E,RED WOOLLY HOTTIE WHITE HEART.,6,12/1/2010 8:26,3.39,17850,United Kingdom  
536365,22752,SET 7 BABUSHKA NESTING BOXES,2,12/1/2010 8:26,7.65,17850,United Kingdom  
536365,21730,GLASS STAR FROSTED T-LIGHT HOLDER,6,12/1/2010 8:26,4.25,17850,United Kingdom  
```

LogGenerator.py is a Python2.7 file
```python
try:
    with open(placeholder, 'r') as f:
        for line in f:
             startLine = int(line)
except IOError:
    startLine = 0

print("Writing " + str(numLines) + " lines starting at line " + str(startLine) + "\n")

totalLinesWritten = 0
linesInFile = GetLineCount()

while (totalLinesWritten < numLines):
    linesWritten = MakeLog(startLine, numLines - totalLinesWritten)
    totalLinesWritten += linesWritten
    startLine += linesWritten
    if (startLine >= linesInFile):
        startLine = 0

print("Wrote " + str(totalLinesWritten) + " lines.\n")

with open(placeholder, 'w') as f:
    f.write(str(startLine))
```

## Prepare to send the data to Firehose
- sudo mkdir /var/log/cadabra/
- cd /etc/aws-kinesis/
- sudo vi agent.json  <-- change the parameters
- Attach a IAM Role to EC2 to write to KDF
- sudo service aws-kinesis-agent start
- sudo chkconfig aws-kinesis-agent on
- cd ~
- sudo LogGenerator.py 2  <-- write 2 lines on KDF and then to S3. Can put 500.000 if no streaming is connected

# Look Around and see metrics
- cd /var/log/cadabra/
- ls
- tail -f /var/log/aws-kinesis-agent/aws-kinesis/agent.log
- grep 84029G *.log
