A JWT is self-contained and is not designed to be invalidated, it will be valid until it expires.

If you want to block access to backend API's when a user logs out you may consider the following strategies:

1. Use opaque [OAuth access tokens](https://oauth.net/2/access-tokens/) for securing API's instead of JWT's (opaqe access tokens can normally be [revoked on the Authorization Server](https://oauth.net/2/token-revocation/) which issued them).
2. Use a traditional session when for all the communications between your backend and your frontend. (see also [NestJS sessions](https://docs.nestjs.com/techniques/session)).
3. Maintain an blacklist of revoked JWT tokens (or a whitelist of valid tokens) in your backend.

While you could use a JWT for securing access to a backend API (e.g. a REST API) it is considered a [bad practice to use a JWT for maintaining a session](http://cryto.net/%7Ejoepie91/blog/2016/06/13/stop-using-jwt-for-sessions/). If you want to maintain a session between backend and frontend, you should use a traditional session mechanism.