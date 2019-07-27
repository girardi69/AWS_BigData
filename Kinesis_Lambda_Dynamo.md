# E-Commerce Order History App

## Set up DynamoDB:
-	Create `CadabraOrders` table
-	Primary key: CustomerID
-	Sort key: OrderID
-	Set provisioning to stay within free tier

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

- check the Handler that is `lambda_function.lambda_handler`

# Change the configuration of the EC2 Producer
- cd /etc/aws-kinesis/
- sudo vi agent.json
- the new code must have two flows, as follow:
```python
{
  "cloudwatch.emitMetrics": true,
  "kinesis.endpoint": "",
  "firehose.endpoint": "",

  "flows": [
    {
      "filePattern": "/var/log/cadabra/*.log",
      "kinesisStream": "CadabraOrders",
      "partitionKeyOption": "RANDOM",
      "dataProcessingOptions": [
         {
            "optionName": "CSVTOJSON",
            "customFieldNames": ["InvoiceNo", "StockCode", "Description", "Quantity", "InvoiceDate", "UnitPrice", "Customer", "Country"]
         }
      ]
    },
    {
      "filePattern": "/var/log/cadabra/*.log*",
      "deliveryStream": "PurchaseLogs"
    }
  ]
```
- relaunch the service
- sudo service aws-kinesis-agent restart
- cd ~


# Test that it works
- Open the EC2 Instance with putty
- launch
`sudo ./LogGenerator.py 10`

