# E-Commerce Order History App

## Set up Kinesis Streams
-	AWS console: Kinesis / create data stream
-	Warn student that Kinesis streams have an hourly cost whether you're using them or not
-	Name: CadabraOrders / 1 shard

## Create IAM Role for Lambda
- IAM Create Role / Choose Policies
- AmazonKinesisReadOnlyAccess
- AmazonDynamoDBFullAccess
- Give the two policies a Role Name `CadabraOrders`

## Create The Lambda
- Click Create a Function and choose Create from scratch
- give it the name ProcessOrders
- choose as runtime Python 2.7
- choose the role `CadabraOrders`

## Add a trigger to the Lambda
- click on Kinesis
- configure the proper stream as `CadabraOrders`
- cick add and then save

## Add the code to the Lambda

```python
import base64
import json
import boto3
import decimal

def lambda_handler(event, context):
    item = None
    dynamo_db = boto3.resource('dynamodb')
    table = dynamo_db.Table('CadabraOrders')
    decoded_record_data = [base64.b64decode(record['kinesis']['data']) for record in event['Records']]
    deserialized_data = [json.loads(decoded_record) for decoded_record in decoded_record_data]

    with table.batch_writer() as batch_writer:
        for item in deserialized_data:
            invoice = item['InvoiceNo']
            customer = int(item['Customer'])
            orderDate = item['InvoiceDate']
            quantity = item['Quantity']
            description = item['Description']
            unitPrice = item['UnitPrice']
            country = item['Country'].rstrip()
            stockCode = item['StockCode']
            
            # Construct a unique sort key for this line item
            orderID = invoice + "-" + stockCode

            batch_writer.put_item(                        
                Item = {
                                'CustomerID': decimal.Decimal(customer),
                                'OrderID': orderID,
                                'OrderDate': orderDate,
                                'Quantity': decimal.Decimal(quantity),
                                'UnitPrice': decimal.Decimal(unitPrice),
                                'Description': description,
                                'Country': country
                        }
            )
```



-	cd /etc/aws-kinesis
-	sudo vi agent.json

 ```python

{
  "cloudwatch.emitMetrics": true,
  "kinesis.endpoint": "",
  "firehose.endpoint": "",
  
  "flows": [
    {
      "filePattern": "/var/log/cadabra/*.log",
      "deliverystream": "PurchaseLogs"
    }
  ]
}
```

- attach the proper IAM Role to the EC2 Instance
-	sudo service aws-kinesis-agent start
-	sudo chkconfig aws-kinesis-agent on

## Test the stream:
-	cd ~
-	sudo ./LogGenerator.py
-	tail -f /var/log/aws-kinesis-agent/aws-kinesis-agent.log
- cd /var/log/cadabra/
- grep 27727 *.log
- cd ~

## Set up DynamoDB:
-	Create CadabraOrders table
-	Primary key: CustomerID
-	Sort key: OrderID
-	Set provisioning to stay within free tier


## Set up Lambda:




## Set up consumer app:
-	sudo pip install boto3  
-	Create the new file and give it the name credentials  
~/.aws/credentials:  
```
[default]
aws_access_key_id = YOUR_ACCESS_KEY
aws_secret_access_key = YOUR_SECRET_KEY
```
-	Create the new file and give it the name config  
~/.aws/config:  
```
[default]
region=us-east-1
```
-	wget http://media.sundog-soft.com/AWSBigData/Consumer.py
-	Chmod a+x Consumer.py
-	./Consumer.py
-	In another window: sudo ./LogGenerator.py 10
-	Give it a minute, observe Consumer script processing the 10 new lines
-	Observe Items in the DynamoDB table in the console

