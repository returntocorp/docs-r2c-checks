# r2c-boto3-hardcoded-access-token

**tl;dr**: This check looks for hardcoded AWS access tokens used in [boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html) API calls.


## Description
Leaking secrets is a common problem. This check looks for hardcoded AWS access tokens used in [boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html) API calls. The check detects the leaking of a hardcoded access token for any of the `aws_access_key_id`, `aws_secret_access_key`, `aws_session_token` keyword arguments.

The check will fire in the following cases:

``` python
# Hardcoded, different API calls
from boto3 import client; client("s3", aws_secret_access_key="jWnyVKHgaSRZVdX7ZQRATpoCkVsvPLRKNZCYRXRL")

import boto3; boto3.sessions.Session(aws_secret_access_key="jWnyVKHgaSRZVdX7ZQRATpoCkVsvPLRKNZCYRXRL")

# Assigned to a variable
from boto3 import client
secret_key = "jWnyVKHgaSRZVdX7ZQRATpoCkVsvPLRKNZCYRXRL"
s3 = client("s3", aws_secret_access_key=secret_key)
```

This check considers the following cases acceptable:

``` python
# Config object
IAMAccessKey = config['DEFAULT']["IAMAccessKey"]
IAMSecretKey = config['DEFAULT']["IAMSecretKey"]
s3_region = config['DEFAULT']["s3_region"]
s3_bucket = config['DEFAULT']["s3_bucket"]

s3 = boto3.resource(
    's3',
    aws_access_key_id=IAMAccessKey,
    aws_secret_access_key=IAMSecretKey,
    region_name=s3_region
)

# Environment variables
key = os.environ.get("ACCESS_KEY_ID")
secret = os.environ.get("SECRET_ACCESS_KEY")
s3 = boto3.resource(
    "s3",
    aws_access_key_id=key,
    aws_secret_access_key=secret,
    region_name="sfo2",
    endpoint_url="https://sfo2.digitaloceanspaces.com",
)
```

## Help

If you have accidentally leaked your AWS tokens, follow the advice in this guide supplied by AWS Security: [What to Do If You Inadvertently Expose an AWS Access Key](https://aws.amazon.com/blogs/security/what-to-do-if-you-inadvertently-expose-an-aws-access-key/).

## References

* https://docs.aws.amazon.com/STS/latest/APIReference/API_Credentials.html
* https://docs.aws.amazon.com/cli/latest/reference/sts/get-session-token.html
* https://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html
* https://boto3.amazonaws.com/v1/documentation/api/latest/guide/quickstart.html#configuration
* https://medium.com/philosophically-secure/your-aws-keys-will-be-stolen-or-leaked-prepare-yourself-e807473f9665
