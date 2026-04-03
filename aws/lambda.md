```bash
export MYPYTHON=myfunctionfile.py
export RUNTIME=python3.13
export LAMBDAFUNCTION=myawesomefunction
export ZIPFILE=my.zip
export HANDLER=myfunction_handler
export IAMROLE=arn:aws:iam::............
```

```python {filename=myfunctionfile.py} {3}
import json

def myfunction_handler(event, context):
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda!')
    }
```


```bash
export MYPYTHON=myfunctionfile.py
export RUNTIME=python3.13
export LAMBDAFUNCTION=myawesomefunction
export ZIPFILE=my.zip
export HANDLER=myfunction_handler
export IAMROLE=arn:aws:iam::............

zip $MYPYTHON $ZIPFILE
aws lambda create-function --function-name $LAMBDAFUNCTION \
    --runtime $RUNTIME \
    --zip-file fileb://$ZIPFILE \
    --handler $( basename $MYPYTHON .py).$HANDLER \
    --role $IAMROLE

aws lambda invoke --function-name $LAMBDAFUNCTION outfile

aws lambda delete-function --function-name $LAMBDAFUNCTION
```
