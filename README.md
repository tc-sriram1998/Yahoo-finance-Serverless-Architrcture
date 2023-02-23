# Yahoo-finace-Serverless-Architrcture

Step 1A: Create EC2 Instance

1) Navigate to EC2 Services from AWS management console page
2) Choose `instances` available in the left pane. 
3) Click on `launch instances`
4) Choose  the free tier for `Amazon Linux`
5) Choose instance type as t2.micro 
6) Configure instance details keep all the option as be default
7) Keep default storage option and provide optional tag details
8) Create a new security group for the instance.(Configure SSH option) Make it more secure by choosing source as MyIP. 
9) Click on the launch button, It will prompt you to create a new key pair. Provide the name and download the key for login purposes. 

Step 1B: Use SSH to login into EC2 instance
Depending on the operating system you are using do the SSH login on the EC2 instance
For windows system use puttygen to convert the pem file into ppk and then do the login using putty
For linux based system perform the normal SSH guideline available on AWS instance→ connect page


--------------------------------------------------------------------------------------------------------------------------------------------


Step 2 : Run Python Script to Pull data from Public API

Your goal is to get per-hour stock price data for a time range for the ten stocks specified in the doc. Please take the closing price.
Further, you should call the static info api for the stocks to get their current 52WeekHigh and 52WeekLow values.
Update the given python code (StockPriceIngestion.py) accordingly.
Print the data on console and run the script.

--------------------------------------------------------------------------------------------------------------------------------------------

Step 3 : Use boto3 and kinesis client to Push data from EC2 to Kinesis Stream

Go to the Kinesis stream services. 
Configure the data stream to accept the data pushed by the script. 
Create and attach appropriate policy and the IAM role to push the data to Kinesis stream from EC2. (helpful link - https://docs.amazonaws.cn/en_us/streams/latest/dev/tutorial-stock-data-kplkcl-iam.html)
Edit the name of the created input kinesis stream inside the python script as required.
You should craft individual data records with information about the stockid, price, price timestamp, 52WeekHigh and 52WeekLow values and push them individually on the Kinesis stream.

--------------------------------------------------------------------------------------------------------------------------------------------

Step 4A : Write a Lambda function to detect POIs and to push the Notification to SNS and add to DynamoDB

Choose lambda services and create a lambda handler. Set it up to act as a consumer to your Kinesis data stream (https://docs.aws.amazon.com/lambda/latest/dg/with-kinesis.html)
A particular price is a POI (point of interest) if it’s either >= 80% of 52WeekHigh or <= 120% of 52WeekLow. If this event happens for a stock, then that data record should be notified via SNS and stored in a DynamoDB alert table as well.
Please note that you may not find any alerts created due to the price values of the stock on that particular day, compared to the 52WeekHigh/Low values. You are free to change the percentages in point 2 to accomplish an alert trigger.
For each stock, if there is already an alert raised on a particular day, then any further alerts on that day should be skipped. Please think about the dynamodb table structure accordingly, to make this easier to query and accomplish.

Step 4B : SNS topic and subscription creation

Goto AWS services look for SNS service
Create a standard topic
Create a subscription and subscribe to the topic using your email

Step 4C : Dynamodb table creation

Goto AWS services and search  for dynamodb
Create a table by providing the partition key and sort key
