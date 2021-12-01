# Open ID Connect providers \(identity pools\)<a name="open-id"></a>

[OpenID Connect](http://openid.net/connect/) is an open standard for authentication that is supported by a number of login providers\. Amazon Cognito supports linking of identities with OpenID Connect providers that are configured through [AWS Identity and Access Management](http://aws.amazon.com/iam/)\.

**Adding an OpenID Connect provider**

For information on how to create an OpenID Connect Provider, see the [IAM documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/identity-providers-oidc.html)\.

**Associating a provider with Amazon Cognito**

Once you've created an OpenID Connect provider in the IAM Console, you can associate it with an identity pool\. All configured providers will be visible in the Edit Identity Pool screen in the Amazon Cognito Console under the OpenID Connect Providers header\.

![\[External Provider Enhanced Authflow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[External Provider Enhanced Authflow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[External Provider Enhanced Authflow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

You can associate multiple OpenID Connect providers with a single identity pool\.

**Using OpenID Connect**

Refer to your provider's documentation for how to login and receive an ID token\.

Once you have a token, add the token to the logins map, using the URI of your provider as the key\.

**Validating an OpenID Connect token**

When first integrating with Amazon Cognito, you may receive an `InvalidToken` exception\. It is important to understand how Amazon Cognito validates OpenID Connect tokens\.

**Note**  
As specified here \([https://tools\.ietf\.org/html/rfc7523](https://tools.ietf.org/html/rfc7523)\), Amazon Cognito allows for a grace period of 5 minutes to handle any clock skew between systems\.

1. The `iss` parameter must match the key used in the logins map \(such as login\.provider\.com\)\.

1. The signature must be valid\. The signature must be verifiable via an RSA public key\.

1. The fingerprint of the certificate hosting the public key matches what's configured on your OpenId Connect Provider\.

1. If the `azp` parameter is present, check this value against listed client IDs in your OpenId Connect provider\.

1. If the `azp` parameter is not present, check the `aud` parameter against listed client IDs in your OpenId Connect provider\.

The website [jwt\.io](http://jwt.io/) is a valuable resource for decoding tokens to verify these values\.

## Android<a name="set-up-open-id-1.android"></a>

```
Map<String, String> logins = new HashMap<String, String>();
logins.put("login.provider.com", token);
credentialsProvider.setLogins(logins);
```

## iOS \- Objective\-C<a name="set-up-open-id-1.ios-objc"></a>

```
credentialsProvider.logins = @{ "login.provider.com": token }
```

## iOS \- Swift<a name="set-up-open-id-1.ios-swift"></a>

To provide the OIDC ID token to Amazon Cognito, implement the `AWSIdentityProviderManager` protocol\.

In the implementation of the logins method, return a dictionary containing the OIDC provider name that you configured\. This dictionary acts as the key and the current ID token from the authenticated user as the value, as shown in the following code example\.

```
class OIDCProvider: NSObject, AWSIdentityProviderManager {
    func logins() -> AWSTask<NSDictionary> {
        let completion = AWSTaskCompletionSource<NSString>()
        getToken(tokenCompletion: completion)
        return completion.task.continueOnSuccessWith { (task) -> AWSTask<NSDictionary>? in
            //login.provider.name is the name of the OIDC provider as setup in the Amazon Cognito console
            return AWSTask(result:["login.provider.name":task.result!])
        } as! AWSTask<NSDictionary>

    }

    func getToken(tokenCompletion: AWSTaskCompletionSource<NSString>) -> Void {
        //get a valid oidc token from your server, or if you have one that hasn't expired cached, return it

        //TODO code to get token from your server
        //...

        //if error getting token, set error appropriately
        tokenCompletion.set(error:NSError(domain: "OIDC Login", code: -1 , userInfo: ["Unable to get OIDC token" : "Details about your error"]))
        //else
        tokenCompletion.set(result:"result from server id token")
    }
}
```

When you instantiate the `AWSCognitoCredentialsProvider`, pass the class that implements AWSIdentityProviderManager as the value of identityProviderManager in the constructor\. For more information, go to the [AWSCognitoCredentialsProvider](http://docs.aws.amazon.com/AWSiOSSDK/latest/Classes/AWSCognitoCredentialsProvider.html) reference page and choose [initWithRegionType:identityPoolId:identityProviderManager](http://docs.aws.amazon.com/AWSiOSSDK/latest/Classes/AWSCognitoCredentialsProvider.html#/api/name/initWithRegionType:identityPoolId:identityProviderManager:)\.

## JavaScript<a name="set-up-open-id-1.javascript"></a>

```
AWS.config.credentials = new AWS.CognitoIdentityCredentials({
 IdentityPoolId: 'IDENTITY_POOL_ID',
 Logins: {
    'login.provider.com': token
 }
});
```

## Unity<a name="set-up-open-id-1.unity"></a>

```
credentials.AddLogin("login.provider.com", token);
```

## Xamarin<a name="set-up-open-id-1.xamarin"></a>

```
credentials.AddLogin("login.provider.com", token);
```