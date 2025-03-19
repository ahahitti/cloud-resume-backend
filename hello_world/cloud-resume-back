import json  # Handles JSON serialization and deserialization
import boto3  # AWS SDK for Python, used to interact with DynamoDB
import os  # Accesses environment variables (for table name)

# ✅ Initialize DynamoDB client
dynamodb = boto3.resource('dynamodb')  # Connects to AWS DynamoDB
table_name = os.environ['TABLE_NAME']  # Retrieves the DynamoDB table name from Lambda environment variables
table = dynamodb.Table(table_name)  # Selects the DynamoDB table

def lambda_handler(event, context):
    """
    AWS Lambda function that updates and returns the visitor count from DynamoDB.
    """

    try:
        # ✅ Increment visitor count in DynamoDB
        response = table.update_item(
            Key={'id': 'visitor_count'},  # Identifies the item in DynamoDB using "id" = "visitor_count"
            UpdateExpression="SET count = if_not_exists(count, :start) + :inc",  # Increments count by 1, sets to 0 if it doesn't exist
            ExpressionAttributeValues={
                ':start': 0,  # Default value if "count" doesn't exist
                ':inc': 1  # Increment by 1
            },
            ReturnValues="UPDATED_NEW"  # Returns the updated count value
        )

        # ✅ Get the new visitor count
        visitor_count = response['Attributes']['count']  # Extracts the updated visitor count from DynamoDB

        return {
            'statusCode': 200,  # HTTP 200 OK response
            'headers': {'Access-Control-Allow-Origin': '*'},  # Allows frontend to make requests (CORS policy)
            'body': json.dumps({'visitor_count': visitor_count})  # Converts visitor count into JSON format
        }

    except Exception as e:
        # ✅ Error Handling: If something goes wrong, return HTTP 500 with error message
        return {
            'statusCode': 500,  # HTTP 500 Internal Server Error
            'body': json.dumps({'error': str(e)})  # Convert error message into JSON format
        }