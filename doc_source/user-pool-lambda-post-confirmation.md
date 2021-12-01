# Post confirmation Lambda trigger<a name="user-pool-lambda-post-confirmation"></a>

Amazon Cognito invokes this trigger after a new user is confirmed, allowing you to send custom messages or to add custom logic\. For example, you could use this trigger to gather new user data\.

The request contains the current attributes for the confirmed user\.

**Topics**
+ [Post confirmation Lambda flows](#user-pool-lambda-post-confirmation-flows)
+ [Post confirmation Lambda trigger parameters](#cognito-user-pools-lambda-trigger-syntax-post-confirmation)
+ [User confirmation tutorials](#aws-lambda-triggers-post-confirm-tutorials)
+ [Post confirmation example](#aws-lambda-triggers-post-confirmation-example)

## Post confirmation Lambda flows<a name="user-pool-lambda-post-confirmation-flows"></a>

### Client confirm sign\-up flow<a name="user-pool-lambda-post-confirmation-1"></a>

![\[Client confirm sign-up flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Client confirm sign-up flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Client confirm sign-up flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

### Server confirm sign\-up flow<a name="user-pool-lambda-post-confirmation-2"></a>

![\[Server confirm sign-up\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Server confirm sign-up\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Server confirm sign-up\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

### Confirm forgot password flow<a name="user-pool-lambda-post-confirmation-3"></a>

![\[Confirm forgot password flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Confirm forgot password flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Confirm forgot password flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

## Post confirmation Lambda trigger parameters<a name="cognito-user-pools-lambda-trigger-syntax-post-confirmation"></a>

These are the parameters required by this Lambda function in addition to the [common parameters](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools-working-with-aws-lambda-triggers.html#cognito-user-pools-lambda-trigger-sample-event-parameter-shared)\.

------
#### [ JSON ]

```
{
    "request": {
            "userAttributes": {
                "string": "string",
                . . .
            },
            "clientMetadata": {
            	"string": "string",
            	. . .
            }
        },
    "response": {}
}
```

------

### Post confirmation request parameters<a name="cognito-user-pools-lambda-trigger-syntax-post-confirmation-request"></a>

**userAttributes**  
One or more key\-value pairs representing user attributes\.

**clientMetadata**  
One or more key\-value pairs that you can provide as custom input to the Lambda function that you specify for the post confirmation trigger\. You can pass this data to your Lambda function by using the ClientMetadata parameter in the following API actions: [AdminConfirmSignUp](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminConfirmSignUp.html), [ConfirmForgotPassword](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ConfirmForgotPassword.html), [ConfirmSignUp](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ConfirmSignUp.html), and [SignUp](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SignUp.html)\.

### Post confirmation response parameters<a name="cognito-user-pools-lambda-trigger-syntax-post-confirmation-response"></a>

No additional return information is expected in the response\.

## User confirmation tutorials<a name="aws-lambda-triggers-post-confirm-tutorials"></a>

The post confirmation Lambda function is triggered just after Amazon Cognito confirms a new user\. See these user confirmation tutorials for JavaScript, Android, and iOS\.


| Platform | Tutorial | 
| --- | --- | 
| JavaScript Identity SDK | [Confirm users with JavaScript](https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-integrating-user-pools-javascript.html#tutorial-integrating-user-pools-confirm-users-javascript) | 
| Android Identity SDK | [Confirm users with Android](https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-integrating-user-pools-android.html#tutorial-integrating-user-pools-confirm-users-android) | 
| iOS Identity SDK | [Confirm users with iOS](https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-integrating-user-pools-ios.html#tutorial-integrating-user-pools-confirm-users-ios) | 

## Post confirmation example<a name="aws-lambda-triggers-post-confirmation-example"></a>

This example Lambda function sends a confirmation email message to your user using Amazon SES\. For more information see [Amazon Simple Email Service Developer Guide](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/)\. 

------
#### [ Node\.js ]

```
var aws = require('aws-sdk');

var ses = new aws.SES();

exports.handler = (event, context, callback) => {
    console.log(event);

    if (event.request.userAttributes.email) {
            sendEmail(event.request.userAttributes.email, "Congratulations " + event.userName + ", you have been confirmed: ", function(status) {

            // Return to Amazon Cognito
            callback(null, event);
        });
    } else {
        // Nothing to do, the user's email ID is unknown
        callback(null, event);
    }
};

function sendEmail(to, body, completedCallback) {
    var eParams = {
        Destination: {
            ToAddresses: [to]
        },
        Message: {
            Body: {
                Text: {
                    Data: body
                }
            },
            Subject: {
                Data: "Cognito Identity Provider registration completed"
            }
        },

        // Replace source_email with your SES validated email address
        Source: "<source_email>"
    };

    var email = ses.sendEmail(eParams, function(err, data){
        if (err) {
            console.log(err);
        } else {
            console.log("===EMAIL SENT===");
        }
        completedCallback('Email sent');
    });
    console.log("EMAIL CODE END");
};
```

------

Amazon Cognito passes event information to your Lambda function\. The function then returns the same event object back to Amazon Cognito, with any changes in the response\. In the Lambda console, you can set up a test event with data that’s relevant to your Lambda trigger\. The following is a test event for this code sample:

------
#### [ JSON ]

```
{
    "request": {
        "userAttributes": {
            "email": "user@example.com",
            "email_verified": true
        }
    },
    "response": {}
}
```

------