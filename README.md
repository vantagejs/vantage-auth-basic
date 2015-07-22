# vantage-auth-basic

Default authentication extension used by Vantage.js. 

- Prompts user for usernames and passwords as specified on the Vantage server.
- Provides maximum retry option, and a retry timeout.
- Can lock users out after X amount of retries, with an unlock timeout.

This extension is required by Vantage on installation, so does not require a separate installation.

## Usage

Normally Vantage.js auth extensions are used as follows:

```js
var basicAuth = require("vantage-auth-basic");
vantage.auth(basicAuth, options);
```

Because this extension is built into vantage's release, you can use the string `"basic"` instead:

```bash
npm install -g vantage
```
```js
var users = [
	{ user: "admin", pass: "4k#842jx!%s" },
	{ user: "user", pass: "Unicorn11" }
];

var vantage = require("vantage")();

vantage.auth("basic", {
	"users": users,
  "retry": 3,
  "retryTime": 500,
  "deny": 1,
  "unlockTime": 3000
});

```

## Options

##### options.users

Accepts an array of objects with a `user` and `pass` property.

```js
 vantage.auth("basic", {
	users: [
	  { user: "admin", pass: "4k#842jx!%s" },
	  { user: "user", pass: "Unicorn11" }
  ]
});
```

The logged in user can then be accessed in a Vantage command:

```js
vantage
  .command("whoami", "Outputs logged in user.")
  .action(function(args, cb){
  	console.log("You are " + this.user);
  	cb();
  });
```

##### options.retry

Specifies how many times one can enter the password wrong before being kicked out of the session.

```js
vantage.auth("basic", { ... retry: 2 });
```

```bash
$ vantage 4000
user: user
pass: Unicorn12
Access denied
pass: Unicorn13
Access denied: too many login attempts.
$
$
```

*Defaults to 3.*

##### options.retryTime

Delay length in millis between login retries.

*Defaults to 1000 millis.*

##### options.deny

Number of kick-outs due to login failures permitted before the user is locked out for a given time.

*Defaults to 3.*

##### options.unlockTime

Timeout before user can attempt to login again after they have been locked out.

*Defaults to 30000 millis.*

## License

MIT

