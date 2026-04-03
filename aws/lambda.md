# Work with AWS Lambda from console

## Example values
```bash
export MYPYTHON=myfunctionfile.py
export RUNTIME=python3.13
export LAMBDAFUNCTION=myawesomefunction
export ZIPFILE=my.zip
export HANDLER=myfunction_handler
export IAMROLE=arn:aws:iam::............
```

## Example python
```python
// myfunctionfile.py   # <-- $MYPYTHON
import json

def myfunction_handler(event, context):  # <-- $HANDLER
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda!')
    }
```

## AWS CLI
```bash
# Create lambda
zip $MYPYTHON $ZIPFILE
aws lambda create-function --function-name $LAMBDAFUNCTION \
    --runtime $RUNTIME \
    --zip-file fileb://$ZIPFILE \
    --handler $( basename $MYPYTHON .py).$HANDLER \
    --role $IAMROLE

# Invoke/Test lambda (saves response to outfile(
aws lambda invoke --function-name $LAMBDAFUNCTION outfile

# Delete lambda function
aws lambda delete-function --function-name $LAMBDAFUNCTION
```
