# SAM Local Installation

Install SAM Local on Windows, and test with a simple Lambda function.

According to [AWS](https://github.com/awslabs/aws-sam-local#sam-local-beta), '_sam is the AWS CLI tool for managing Serverless applications written with AWS Serverless Application Model (SAM). SAM Local can be used to test functions locally, start a local API Gateway from a SAM template, validate a SAM template, and generate sample payloads for various event sources._'

## Install Docker for Windows

Link: [Docker for Windows](https://docs.docker.com/docker-for-windows/install/#download-docker-for-windows)<br>
Choose the Stable or Edge version of Docker for Windows.

![Docker](install_pics\SAM_Local_01.PNG)

You will need to close and logout.

![Docker](install_pics\SAM_Local_02.PNG)

Windows will ask you to restart.

![Docker](install_pics\SAM_Local_03.PNG)

## Install AWS SAM Local

Install `aws-sam-local` npm package globally.

```javascript
npm install -g aws-sam-local
sam --version
```

![Node](install_pics\SAM_Local_07.PNG)

## Sample Lambda Function / API Gateway

Sample taken from various AWS samples.

Lambda: `index.js`

```javascript
'use strict';

console.log('Loading function');

exports.handler = (event, context, callback) => {
  console.log('Received event:', JSON.stringify(event, null, 2));
  console.log('value1 =', event.key1);
  console.log('value2 =', event.key2);
  console.log('value3 =', event.key3);
  callback(null, {
    statusCode: 200,
    headers: {
      "x-custom-header": event.key1
    },
    body: event.key2
  });
};
```

Test Event: `event.json`

```json
{
  "key3": "Lambda",
  "key2": "Serverless",
  "key1": "API Gateway"
}
```

SAM Template: `template.yaml`

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: 'AWS::Serverless-2016-10-31'
Description: A starter AWS Lambda function.
Resources:
  helloworld:
    Type: 'AWS::Serverless::Function'
    Properties:
      Handler: index.handler
      Runtime: nodejs6.10
      CodeUri: .
      Description: A starter AWS Lambda function.
      MemorySize: 128
      Timeout: 3
      Events:
        Api:
          Type: Api
          Properties:
            Path: /test
            Method: get
```

### Invoke Lambda with an Event file

To test SAM Local, execute: `sam local invoke -e event.json`.

![Node](install_pics\SAM_Local_08.PNG)

The first time you run SAM local, you will need to share access to your drive.

![Docker](install_pics\SAM_Local_06.PNG)

## SAM Local References

- [SAM Local Requirements](https://docs.aws.amazon.com/lambda/latest/dg/sam-cli-requirements.html)
- [SAM Local Guide](https://github.com/awslabs/aws-sam-local)
- [SAM Node Hello World Sample](https://github.com/awslabs/serverless-application-model/tree/master/examples/apps/hello-world)
