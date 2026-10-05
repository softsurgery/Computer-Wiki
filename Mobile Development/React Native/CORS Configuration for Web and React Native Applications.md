CORS (Cross-Origin Resource Sharing) is a browser security mechanism that controls which web applications are allowed to make requests to an API.
For our API, CORS is configured through a single environment variable:

```env
CORS_ORIGINS=https://web.example,https://landing.example
```

The value is environment-specific and can be configured independently for production, development, and local environments.

## Web Applications

Web applications send an `Origin` header with API requests. The API validates this origin against the configured `CORS_ORIGINS` whitelist.
Only explicitly configured origins are allowed to make browser requests to the API.

## React Native Applications

Native React Native applications do not normally send a browser `Origin` header because they run outside of a browser environment.
Therefore, a native React Native application does not need to be added to `CORS_ORIGINS`.
Requests without an `Origin` header can be allowed by the API:

```ts
if (!origin) {
  return true;
}
```

This also supports other non-browser clients, such as server-to-server requests and command-line tools.
React Native Web is different. Since it runs inside a browser, it sends an `Origin` header and must therefore be included in the configured CORS origins.

## Environment-Based Configuration

CORS origins should not be hard-coded in the application bootstrap code. Instead, each environment provides its own `CORS_ORIGINS` value.

For example:
- **Production:** production web origins
- **Development:** development web origins
- **Local:** localhost origins required for local development

The API loads the appropriate environment configuration and passes the configured origins to the CORS configuration.
This keeps environment-specific values outside the application code and prevents development origins from accidentally being enabled in production.

## Summary

The CORS strategy is:
- **Web applications:** explicitly whitelist their origins.
- **Landing applications:** explicitly whitelist their origins.
- **Native React Native:** no CORS origin is required.
- **React Native Web:** explicitly whitelist its browser origin.
- **Local development:** localhost origins are configured only in local environment files.
- **Production:** only production origins are configured.
- **Application code:** contains no hard-coded environment-specific origins.

This approach keeps CORS configuration environment-driven while supporting both browser-based and native mobile clients securely.