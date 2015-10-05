# Manage Users

This section shows how to sign in, sign out and get user information through `McsSession`. 

## Sign In

Use `McsSession.getInstance().requestSignIn()` to sign user in. 

```java
// call in main thread
McsSession.getInstance().requestSignIn(email, pwd, 
    new McsResponse.SuccessListener<JSONObject>() {
        @Override public void onSuccess(JSONObject response) {
            // Signed in, back to UI thread
        }
    },
    /**
     * Optional.
     * Default error message shows in log.
     */
    new McsResponse.ErrorListener() {
        @Override public void onError(Exception e) {
            // Sign in failed, back to UI thread
        }
    }
);
```

Note that `requestSignIn()` will work in background network thread.

## Sign Out

It's even easier to sign out. 

```java
// call in main thread
McsSession.getInstance().requestSignOut(
    new McsResponse.SuccessListener<JSONObject>() {
      @Override public void onSuccess(JSONObject response) {
        // Signed out, back to UI thread
    }
);
```

Check [Sessions - MCS SDK Android Guide][sdk-guide-sessions]) for detailed description like parameter requirements.


[sdk-guide-sessions]: https://mtk-mcs.gitbooks.io/mcs-sdk-android-guide/content/sessions.html
