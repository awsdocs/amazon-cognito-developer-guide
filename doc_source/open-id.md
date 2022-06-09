# Open ID Connect providers \(identity pools\)<a name="open-id"></a>

[OpenID Connect](http://openid.net/connect/) is an open standard for authentication that a number of login providers support\. Amazon Cognito supports you to link identities with OpenID Connect providers that you configure through [AWS Identity and Access Management](http://aws.amazon.com/iam/)\.

**Adding an OpenID Connect provider**

For information about how to create an OpenID Connect provider, see the [IAM documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/identity-providers-oidc.html)\.

**Associating a provider with Amazon Cognito**

After you create an OpenID Connect provider in the IAM Console, you can associate it with an identity pool\. All providers that you configure are visible in the Edit Identity Pool screen in the Amazon Cognito Console under the OpenID Connect Providers header\.

![\[External Provider Enhanced Authflow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[External Provider Enhanced Authflow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[External Provider Enhanced Authflow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

You can associate multiple OpenID Connect providers with a single identity pool\.

**Using OpenID Connect**

Refer to your provider's documentation for how to login and receive an ID token\.

After you have a token, add the token to the logins map\. Use the URI of your provider as the key\.

**Validating an OpenID Connect token**

When you first integrate with Amazon Cognito, you might receive an `InvalidToken` exception\. It is important to understand how Amazon Cognito validates OpenID Connect \(OIDC\) tokens\.

**Note**  
As specified here \([https://tools\.ietf\.org/html/rfc7523](https://tools.ietf.org/html/rfc7523)\), Amazon Cognito provides a grace period of 5 minutes to handle any clock skew between systems\.

1. The `iss` parameter must match the key that the logins map uses \(such as login\.provider\.com\)\.

1. The signature must be valid\. The signature must be verifiable via an RSA public key\.

1. The fingerprint of the certificate public key matches the fingerprint that you set in IAM when you created your OIDC provider\.

1. If the `azp` parameter is present, check this value against listed client IDs in your OIDC provider\.

1. If the `azp` parameter isn't present, check the `aud` parameter against listed client IDs in your OIDC provider\.

The website [jwt\.io](http://jwt.io/) is a valuable resource that you can use to decode tokens and verify these values\.

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

When you implement the `logins` method, return a dictionary that contains the OIDC provider name that you configured\. This dictionary acts as the key, and the current ID token from the authenticated user acts as the value, as shown in the following code example\.

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

When you instantiate the `AWSCognitoCredentialsProvider`, pass the class that implements AWSIdentityProviderManager as the value of identityProviderManager in the constructor\. For more information, go to the [https://github.com/aws-amplify/aws-sdk-ios](https://github.com/aws-amplify/aws-sdk-ios) reference page and choose [initWithRegionType:identityPoolId:identityProviderManager](https://github.com/aws-amplify/aws-sdk-ios)\.

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