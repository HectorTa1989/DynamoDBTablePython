# DynamoDBTablePython

##Objective: Create a DynamoDB table and load data into it using Python

##Prerequisites:
Access to AWS Cloud9
Rudimentary Python knowledge
Rudimentary JSON knowledge
An AWS account

##Disclaimer: I performed this project on my root account for the sake of completing the task. However, in a production environment, you will want to assign an IAM role with the appropriate permissions.

##Steps
Log into your AWS console
Go to the Cloud9 service
You will need to create the table, in this case we will use Python to create the table. I used the following code:

```
import boto3
def create_sneaker_table(dynamodb=None):
     if not dynamodb:
         dynamodb = boto3.resource('dynamodb', region_name= 'us-east-1')
table = dynamodb.create_table(
         TableName='Sneakers',
         KeySchema=[
             {
                 'AttributeName': 'brand',
                 'KeyType': 'HASH'  # Partition key
             },
             {
                 'AttributeName': 'types',
                 'KeyType': 'RANGE'  # Sort key
             }
         ],
         AttributeDefinitions=[
             {
                 'AttributeName': 'brand',
                 'AttributeType': 'S'
             },
             {
                 'AttributeName': 'types',
                 'AttributeType': 'S'
             },
],
         ProvisionedThroughput={
             'ReadCapacityUnits': 10,
             'WriteCapacityUnits': 10
         }
     )
     return table
if __name__ == '__main__':
     sneaker_table = create_sneaker_table()
     print("Table status:", sneaker_table.table_status)     
```

This code needs to be saved to a .py file and then ran in your Cloud9 environment using the following command “ python insertyourfilenamehere.py”. You can confirm the table was created in the console. You should see something like this:

![image](https://user-images.githubusercontent.com/31132150/151682001-c4b0b20a-f106-4e24-a864-32c20857bc80.png)


4. You will need to create another script to load the data and a json file for that python script to pull the data from in order to load it. The JSON file should look something like this:
```
[
    {
        "brand": "Adidas",
        "types": "Stan Smith"
    },
    {
        "brand": "Adidas",
        "types": "Adidas Gazelle"
    },
    {
        "brand": "Adidas",
        "types": "Adidas Samba"
    }
]
```
5. Create a Python script that will using that JSON file and you will run that python file to load the data in the JSON file into your newly created DynamoDB table. This is the Python script I used:

```
import json
import boto3
dynamodb= boto3.resource('dynamodb', region_name= 'us-east-1')
table= dynamodb.Table('Sneakers')
with open("sneakerdata.json") as json_file:
    Sneakers_list= json.load(json_file)
    for sneaker in Sneakers_list:
        brand= sneaker['brand']
        types= sneaker['types']
print( "Adding Sneaker:", brand, types)
table.put_item (Item= sneaker)
```

6. Run the Python script using “python insertyourfilenamehere.py” in Cloud9 again. Once you successfully run it, you will see an output like this in the terminal:

![image](https://user-images.githubusercontent.com/31132150/151682008-2258da39-2f8e-449a-a83c-96687912ad3a.png)


You can also confirm it ran in the console itself:

![image](https://user-images.githubusercontent.com/31132150/151682018-a5e25dbe-ce62-40d7-9160-5ad298cd5f5c.png)


That is it! That is how you can create a table and load it with data in DynamoDB using Python.
Resources referenced to complete this project:
https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStarted.Python.01.html
https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStarted.Python.02.html
