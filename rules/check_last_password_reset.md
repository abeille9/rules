---
gallery: true
short_description: Check the last time that a user changed his or her account password
categories:
- access control
---
## Check last password reset

This rule will check the last time that a user changed his or her account password.

> NOTE: This rule leverages the `last_password_reset` user profile field. It's important to understand that this field doesn't get set at the time of password _change_ but only when the user performs the _next login_ with the new password. This works since rules always run after authentication occurs anyway.

```js
function (user, context, callback) {
  
  function daydiff (first, second) {
    return (second-first)/(1000*60*60*24);
  }
  
  var last_password_change = user.last_password_reset || user.created_at;
  
  if (daydiff(new Date(last_password_change), new Date()) > 30) {
    return callback(new UnauthorizedError('please change your password'));
  }
  
  callback(null, user, context);
}
```
