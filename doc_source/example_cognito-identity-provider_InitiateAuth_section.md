# Start authentication with a device tracked by Amazon Cognito using an AWS SDK<a name="example_cognito-identity-provider_InitiateAuth_section"></a>

The following code examples show how to start authentication with a device tracked by Amazon Cognito\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ \.NET ]

**AWS SDK for \.NET**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/dotnetv3/Cognito#code-examples)\. 
  

```
        /// <summary>
        /// Initiates the authorization process.
        /// </summary>
        /// <param name="identityProviderClient">An initialized Identity
        /// Provider client object.</param>
        /// <param name="clientId">The client Id of the application associated
        /// with the user pool.</param>
        /// <param name="userName">The user name to be authorized.</param>
        /// <param name="password">The password of the user.</param>
        /// <returns>The response from the client from the InitiateAuthAsync
        /// call.</returns>
        public static async Task<InitiateAuthResponse> InitiateAuth(AmazonCognitoIdentityProviderClient identityProviderClient, string clientId, string userName, string password)
        {
            var authParameters = new Dictionary<string, string>
            {
                { "USERNAME", userName },
                { "PASSWORD", password },
            };

            var authRequest = new InitiateAuthRequest
            {
                ClientId = clientId,
                AuthParameters = authParameters,
                AuthFlow = AuthFlowType.USER_PASSWORD_AUTH,
            };

            var response = await identityProviderClient.InitiateAuthAsync(authRequest);
            Console.WriteLine($"Result Challenge is : {response.ChallengeName}");

            return response;
        }
```
+  For API details, see [InitiateAuth](https://docs.aws.amazon.com/goto/DotNetSDKV3/cognito-idp-2016-04-18/InitiateAuth) in *AWS SDK for \.NET API Reference*\. 

------
#### [ JavaScript ]

**SDK for JavaScript V3**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cognito#code-examples)\. 
  

```
const initiateAuth = async ({ username, password, clientId }) => {
  const client = createClientForDefaultRegion(CognitoIdentityProviderClient);

  const command = new InitiateAuthCommand({
    AuthFlow: AuthFlowType.USER_PASSWORD_AUTH,
    AuthParameters: {
      USERNAME: username,
      PASSWORD: password,
    },
    ClientId: clientId,
  });

  return client.send(command);
};
```
+  For API details, see [InitiateAuth](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/initiateauthcommand.html) in *AWS SDK for JavaScript API Reference*\. 

------
#### [ Python ]

**SDK for Python \(Boto3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/python/example_code/cognito#code-examples)\. 
Sign in with a tracked device\. To complete sign\-in, the client must respond correctly to Secure Remote Password \(SRP\) challenges\.  

```
class CognitoIdentityProviderWrapper:
    """Encapsulates Amazon Cognito actions"""
    def __init__(self, cognito_idp_client, user_pool_id, client_id, client_secret=None):
        """
        :param cognito_idp_client: A Boto3 Amazon Cognito Identity Provider client.
        :param user_pool_id: The ID of an existing Amazon Cognito user pool.
        :param client_id: The ID of a client application registered with the user pool.
        :param client_secret: The client secret, if the client has a secret.
        """
        self.cognito_idp_client = cognito_idp_client
        self.user_pool_id = user_pool_id
        self.client_id = client_id
        self.client_secret = client_secret

    def sign_in_with_tracked_device(
            self, user_name, password, device_key, device_group_key, device_password,
            aws_srp):
        """
        Signs in to Amazon Cognito as a user who has a tracked device. Signing in
        with a tracked device lets a user sign in without entering a new MFA code.

        Signing in with a tracked device requires that the client respond to the SRP
        protocol. The scenario associated with this example uses the warrant package
        to help with SRP calculations.

        For more information on SRP, see https://en.wikipedia.org/wiki/Secure_Remote_Password_protocol.

        :param user_name: The user that is associated with the device.
        :param password: The user's password.
        :param device_key: The key of a tracked device.
        :param device_group_key: The group key of a tracked device.
        :param device_password: The password that is associated with the device.
        :param aws_srp: A class that helps with SRP calculations. The scenario
                        associated with this example uses the warrant package.
        :return: The result of the authentication. When successful, this contains an
                 access token for the user.
        """
        try:
            srp_helper = aws_srp.AWSSRP(
                username=user_name, password=device_password,
                pool_id='_', client_id=self.client_id, client_secret=None,
                client=self.cognito_idp_client)

            response_init = self.cognito_idp_client.initiate_auth(
                ClientId=self.client_id, AuthFlow='USER_PASSWORD_AUTH',
                AuthParameters={
                    'USERNAME': user_name, 'PASSWORD': password, 'DEVICE_KEY': device_key})
            if response_init['ChallengeName'] != 'DEVICE_SRP_AUTH':
                raise RuntimeError(
                    f"Expected DEVICE_SRP_AUTH challenge but got {response_init['ChallengeName']}.")

            auth_params = srp_helper.get_auth_params()
            auth_params['DEVICE_KEY'] = device_key
            response_auth = self.cognito_idp_client.respond_to_auth_challenge(
                ClientId=self.client_id,
                ChallengeName='DEVICE_SRP_AUTH',
                ChallengeResponses=auth_params
            )
            if response_auth['ChallengeName'] != 'DEVICE_PASSWORD_VERIFIER':
                raise RuntimeError(
                    f"Expected DEVICE_PASSWORD_VERIFIER challenge but got "
                    f"{response_init['ChallengeName']}.")

            challenge_params = response_auth['ChallengeParameters']
            challenge_params['USER_ID_FOR_SRP'] = device_group_key + device_key
            cr = srp_helper.process_challenge(challenge_params)
            cr['USERNAME'] = user_name
            cr['DEVICE_KEY'] = device_key
            response_verifier = self.cognito_idp_client.respond_to_auth_challenge(
                ClientId=self.client_id,
                ChallengeName='DEVICE_PASSWORD_VERIFIER',
                ChallengeResponses=cr)
            auth_tokens = response_verifier['AuthenticationResult']
        except ClientError as err:
            logger.error(
                "Couldn't start client sign in for %s. Here's why: %s: %s", user_name,
                err.response['Error']['Code'], err.response['Error']['Message'])
            raise
        else:
            return auth_tokens
```
+  For API details, see [InitiateAuth](https://docs.aws.amazon.com/goto/boto3/cognito-idp-2016-04-18/InitiateAuth) in *AWS SDK for Python \(Boto3\) API Reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, see [Using this service with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.