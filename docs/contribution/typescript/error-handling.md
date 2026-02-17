---
title: Error Handling
sidebar_position: 6
---

# Error Handling

## 1. Only throw (subclass of) `Error`
```typescript
// Throw only Errors (also always include new keyword for consistency)
throw new Error("oh noes!");
```

## 2. Always provide additional context
If throwing a new error in, always provide more context so that looking at the stack trace is helpful.
```typescript
throw new Error(`Failed to fetch user with id=${id}: ${err.message}`);
```

## 3. Always limit the amount of code in a try block
*To Do: Add a better of example of a lot of code in a try block*
```typescript
try { // bad!
  const result = methodThatMayThrow();
  use(result);
} catch (error: unknown) {
  // ...
}

let result;
try { // good!
  result = methodThatMayThrow();
} catch (error: unknown) {
  // ...
}
use(result);
```
The reason for this is that the purpose of try blocks are to gracefully handle errors and the handling of errors
can be more specific is the code that is running in a try block is limited. Code in try blocks should only be
code that may throw an error in production. If a line should never throw an error in production then by including
it in the try block may obscure a bug.  

However often times it is overly verbose to make an uninitialized let variable to eventually store the output
of the function that is run in the try block. It is up to the programmer's discretion but it is best practice to
avoid long try blocks for the reasons described above.

### Error Handling for APIs
Consider we have an API which logs a user in where the backend API can return the following possibilities.

```json
{"user_id": 1}, 200 (sucessful response)
```
```json
{
    "detail": {
        "type": "login_failed",
        "message": "The password that the user entered may be incorrect",
        "location": ["email", "password"]
    }
}, 401
```
```json
{
    "detail": {
        "type": "user_not_found",
        "message": "The user does not exist: user=user123",
        "location": ["username"]
    }
}, 404
```
```json
{
    "detail": {
        "type": "user_already_logged_in",
        "message": "User is already logged, multiple logins not allowed",
        "location": []
    }
}, 409
```
More information about how these errors are generated in the backend are available in the python error
handling page.  

The following is boilerplate for how these errors should be handled in the frontend.
```typescript
try {
  const loginResponse = await authApi.login(username, email, password);
} catch (error) {
  let message = "Unknown login error occured"; // default error message
  if (error instanceof AxiosError && error.response) {
    switch (error.response.status) {
      case 401:
        message = "The password entered is incorrect";
        break;
      case 404:
        message = "The username entered is not a valid user";
        break;
      case 409:
        message = "This user is already logged in and multiple logins are not allowed";
        break;
      case 422:
        message = "Invalid input, however case should be handled by input validation on frontend";
        break;
      default:
        message = error.response.detail?.message || "Unknown server error on login";
        message = `Server error: ${message}`;
    }
  } else if (error instanceof Error) {
    message = error.message; // catches non HTTP error cases
  }
  showAlert("info", message);
} finally {
  setTabUiState(null); // cleanup UI state
}
```
Is this really verbose? Yeah. However this logic can be abstracted into the api module if the only difference
between the different errors is that a different message is outputted. If there are going to be different state
changes for different errors then you can either pass in anonymous functions for the state changes or just not
abstract the logic.

Although the best case scenario (which is shown above) is that the different error codes correspond one-to-one
with different possible errors, there are situations where this is not possible such as if multiple different
kinds of errors fall under a `404` error. In this case you would have to encode the different possible errors
into the `type` field in the detail dict and then a nested if statement would be used.

Additionally the `location` field did not serve any purpose in this case however there may be instances where
you want to specify multiple constants to further specify information about the error that can be handled
programmatically easily.
