The Auth category can be used to register a user, confirm attributes like email/phone and sign in with optional multi-factor authentication. It is set up to use Amazon Cognito User Pools which manages the users and their properties.

## Register a user

The default CLI flow as mentioned in authentication_start.md requires a <code>username, password and a valid email</code> as parameters to register a user. Invoke the following api to initiate a sign up flow.


```Kotlin
val options = AuthSignUpOptions.builder()
    .userAttribute(AuthUserAttributeKey.email(), "my@email.com")
    .build()

//Callback
Amplify.Auth.signUp("username", "Password123", options,
    { Log.i("AuthQuickStart", "Sign up succeeded: $it") },
    { Log.e ("AuthQuickStart", "Sign up failed", it) }
)
//Coroutine
try {
    val result = Amplify.Auth.signUp("username", "Password123", options)
    Log.i("AuthQuickStart", "Result: $result") 
} catch (error: AuthException) {
    Log.e("AuthQuickStart", "Sign up failed", error)
}
```

The next step in the sign up flow is to confirm the user. A confirmation code will be sent to the email id provided during sign up. Enter the confirmation code received via email in the <code>confirmSignUp</code> call.

```Kotlin
val code = "code you received via email"

//Callback
Amplify.Auth.confirmSignUp("username", code, { result ->
        if (result.isSignUpComplete) {
            Log.i("AuthQuickstart", "Confirm signUp succeeded")
        } else {
            Log.i("AuthQuickstart","Confirm sign up not complete")
        }
    },
    { Log.e("AuthQuickstart", "Failed to confirm sign up", it) }
})
//Coroutine
try {
    val result = Amplify.Auth.confirmSignUp("username", code)
    if (result.isSignUpComplete) {
        Log.i("AuthQuickstart", "Confirm signUp succeeded")
    } else {
        Log.i("AuthQuickstart", "Confirm sign up not complete")
    }    
} catch (error: AuthException) {
    Log.e("AuthQuickstart", "Failed to confirm signup", error)
}
```

You will know the sign up flow complete, if you see the following in your console window:

```Console
Confirm signUp succeeded
````


## Sign In a user

Implement a UI to get the username and password from the user. After the user enters the username and password, you can start the sign in flow by calling the following method:

```Kotlin
//Callback
Amplify.Auth.signIn("username", "password",
    { result ->
        if (result.isSignInComplete) {
            Log.i("AuthQuickstart", "Sign in succeeded")
        } else {
            Log.i("AuthQuickstart", "Sign in not complete")
        }
    },
    { Log.e("AuthQuickstart", "Failed to sign in", it) }
)
//Coroutine
try {
    val result = Amplify.Auth.signIn("username", "password")
    if (result.isSignInComplete) {
        Log.i("AuthQuickstart", "Sign in succeeded")
    } else {
        Log.e("AuthQuickstart", "Sign in not complete")
    }
} catch (error: AuthException) {
    Log.e("AuthQuickstart", "Sign in failed", error)
}
```

You will know the sign in flow is complete if you see the following in your console window:

```Console
Sign in succeeded
````

**You have now successfully registered a user and authenticated with that user's username and password with Amplify.**

