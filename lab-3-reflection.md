Lab 3 — OAuth Integration Reflection
1. CSRF and the state Parameter

One thing that stood out to me from the article is that an OAuth login can technically work while still having a security problem. In an OAuth CSRF attack, an attacker starts an OAuth authorization process themselves and then gets the victim’s browser to use the authorization response. This can cause the victim’s application session to become connected to the attacker’s authorization instead of the victim’s own account. The victim may not realize anything happened because the login flow can still appear normal.

The state parameter helps prevent this by giving the application a random value that is created when the login process starts. The application saves that value and expects the same value to come back with the OAuth callback. If the returned state does not match what the application originally created, the application can reject the request. This gives the application a way to verify that the callback actually belongs to the login process that the user started.

2. Redirect URI Attacks

A redirect URI can become dangerous when an application checks only that the domain is correct instead of checking the complete URI. For example, imagine an application normally uses https://example.com/oauth/callback, but its OAuth provider allows any path on example.com. An attacker could find or create a page such as https://example.com/something that can redirect users somewhere else or expose information.

The attacker could then manipulate the OAuth request so that the authorization code is sent to that unintended path. If that page redirects the browser to an attacker-controlled website, the authorization code could potentially be exposed during the process. This is why the article recommends comparing the exact redirect URI, including the scheme, hostname, port, and path, instead of trusting an entire domain or allowing arbitrary subpaths.

3. User Experience vs. Security

The biggest trade-off I see with OAuth is that convenience also creates another security responsibility for the developer. A “Login with Google” or “Login with GitHub” button makes logging in easier because users do not have to create and remember another password. However, the application is now depending on a more complicated process involving another service, redirects, authorization codes, tokens, and user information.

From working with basic authentication, it is easy to think that getting the login to work is the main goal. The article made me realize that the security details are just as important as making the feature functional. A development team has to decide whether the improved user experience is worth adding that complexity, and if they choose OAuth, they have to take responsibility for implementing the flow correctly. Convenience should not come at the cost of exposing a user's account or authorization data.