The Amplify Auth category provides an interface for authenticating a user. Behind the scenes, it provides the necessary authorization to the other Amplify categories. It comes with default, built-in support for Amazon Cognito User Pool and Identity Pool. The Amplify CLI helps you to create and configure the auth category with an authentication provider.

## Configure Auth Category

To start provisioning auth resources in the backend, go to your project directory and **execute the command:**
```console
amplify add auth
```

Enter the following when prompted:
```console
? Do you want to use the default authentication and security configuration?
    `Default configuration`
? How do you want users to be able to sign in?
    `Username`
? Do you want to configure advanced settings?
    `No, I am done.`
```

To push your changes to the cloud, **execute the command**
```console
amplify push
```

Upon completion, <code>amplifyconfiguration.json</code> should be updated to reference provisioned backend auth resources, Note that these files should already be a part of your project.

## Install Amplify Libraries

Add the following dependency to your app's <code>build.grade</code>.

```
dependencies {
    implementation 'com.amplifyframework:aws-auth-cognito:1.30.0'
}
```

## Kotlin Coroutine Recap 

Imports:
```Kotlin
import com.amplifyframework.kotlin.core.Amplify
import androidx.lifecycle.lifecycleScope
import kotlinx.coroutines.launch
import kotlinx.coroutines.withContext
```

Code:
``` Kotlin
lifecycleScope.launch {
    withContext(Dispatcher.IO) {
        try {
            val res = Amplify.Auth.singUp(TAG, "")
        } catch ( error: AuthException ) {
            Log.e(TAG, "--- $error")
        }
    }
}
```

## Initialize Amplify Auth

Add the Auth plugin before calling <code>Amplify.configure</code>.

```Kotlin
override fun onCreate() {
    ...
    //Add this line, to include Auth plugin.
    Amplify.addPlugin(AWSCognitoAuthPlugin())
    Amplify.configure(applicationContext)
    ...
}
```

## Check the current auth session

We can now check the current auth session.
For testing purposes, we can run this from MainActivity's <code>onCreate</code> method.

```Kotlin
Amplify.Auth.fetchAuthSession(
    { Log.i(TAG, "Auth session = $it") },
    { Log.e(TAG, "Failed to fetch auth session = $it" ) }
)
```

The <code>isSignedIn</code> property of the authSession will be false since we haven't signed in to the category yet.
