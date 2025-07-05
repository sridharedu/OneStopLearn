# Security Combined Notes

## 001.Security-what-are-the-components-of-spring-security.md
✨ **Components of Spring Security**
- → When a request comes from a web browser or RESTful client to a secure Spring Boot application, the first component that intercepts it is the `Authentication Filter`.
- → **Authentication Filter:**
    - → A servlet filter that checks if the request is already authenticated.
    - → If not authenticated, it forwards the request to the `Authentication Manager`.
    - → If authentication is successful, it uses `AuthenticationSuccessHandler` to store user and authentication details in a `SecurityContext` object.
    - → If authentication fails, it uses `AuthenticationFailureHandler` to send an error response back to the client.
- → **Authentication Manager:**
    - → Internally uses an `Authentication Provider`.
- → **Authentication Provider:**
    - → Contains the core authentication logic.
    - → Uses `UserDetailsService` to fetch user details from a database, LDAP, or other sources.
    - → Uses `PasswordEncoder` to encode incoming passwords (as plain text passwords are not used).
    - → If authentication is successful, it sends the appropriate response back.
    - → If authentication fails, it hands the response back to the `Authentication Manager`.

## 002.Security-how-did-you-secure-your-rest-apis.md
✨ **How to Secure REST APIs**
- → Used OAuth along with JWT (JSON Web Tokens) to secure microservice RESTful applications.

## 003.Security-what-is-oauth.md
✨ **What is OAuth**
- → OAuth stands for Open Authentication and Authorization standard.
- → It allows one application to access a user's data within other applications.
- → This is done without the user directly sharing their user ID and password of these applications with the application that needs access to the data.

## 004.Security-what-are-the-key-components-in-oauth.md
✨ **Key Components in OAuth**
- → There are four components in the OAuth workflow:
    - → **Authorization Server:** Authenticates users and generates tokens.
    - → **Resource Server:** Stores secure resources and where applications live.
    - → **Resource Owner:** The user who owns the resource and grants permissions.
    - → **Client:** The application that needs access to resources on the Resource Server.

## 005.Security-what-is-the-oauth-workflow.md
✨ **OAuth Workflow**
- → A typical workflow starts when a user tries to access a client application that needs access to other resources owned by the user.
- → The client application asks the user (resource owner) to share their user ID and password with the `Authorization Server`.
- → The `Authorization Server` validates the user data and password, then generates and sends a token back to the `Client Application`.
- → The `Client Application` uses this token to communicate with the `Resource Server`.
- → The `Resource Server` validates the token by communicating with the `Authorization Server`.
- → If the token is valid, the `Resource Server` sends the required data to the `Client Application`.

## 006.Security-what-are-the-oauth-grant-types.md
✨ **OAuth Grant Types**
- → Grant types allow configuring different workflows for how the username and password are exchanged between the end user (resource owner), the authorization server, and the client application, depending on the use case.

## 007.Security-what-are-the-different-grant-types.md
✨ **Different OAuth Grant Types**
- → There are four different grant types:
    - → **Authorization Code:**
        - → The client application redirects the end user (resource owner) to the authorization server for authentication.
        - → After successful authentication, the authorization server informs the client app.
        - → The client app then requests and receives a token, which it uses to communicate with the resource server.
    - → **Password:**
        - → The client app directly takes the username and password from the resource owner.
        - → It uses these credentials to communicate with the authorization server to get a token.
        - → The token is then used to communicate with the resource server.
    - → **Client Credentials:**
        - → This workflow does not involve an end user or resource owner.
        - → Typically used for communication between microservice applications.
        - → The client app has a user ID and password provided by the authorization server, stored locally.
        - → It uses these credentials to obtain a token when needed to communicate with the resource server.
        - → Very useful for single sign-on (SSO) applications.
    - → **Refresh Token:**
        - → OAuth tokens expire after some time.
        - → If configured, the client app can use an old (expired) token to get a new token from the authorization server without going through the entire initial workflow of other grant types.

## 008.Security-what-is-jwt.md
✨ **What is JWT**
- → JWT stands for JSON Web Token.
- → In a typical OAuth workflow, when a client application sends an OAuth token to the resource server, the resource server usually communicates with the authorization server to validate the token and get user/role information.
- → JWTs are designed to carry user and user role details directly within the token itself.
- → When a JWT token arrives at the resource server, it does not need to communicate with the authorization server to get user details, as the token already contains them.
- → JWT tokens are signed by the authorization server.
- → The resource server can verify the signature of the JWT using public/private keys, eliminating the need for another call to the authorization server for validation.
- → If the signature is valid, the token is considered valid.

## 009.Security-how-to-configure-jwt.md
✨ **How to Configure JWT**
- → **Step 1: Authorization Server Configuration**
    - → In the authorization server project, use `JwtAccessTokenConverter` to create JWT tokens instead of regular OAuth tokens.
    - → Store these tokens using `JwtTokenStore`.
- → **Step 2: Token Signing Methods**
    - → **Symmetric Key Signing:**
        - → Use a private/symmetric key.
        - → This key is shared with both the authorization server and the resource server.
        - → The authorization server signs the token using this key.
        - → The resource server uses the same key to symmetrically validate the token's signature.
    - → **Asymmetric Key Signing:**
        - → Generate a key pair (private key and public key) using `Java keytool`.
        - → The private key is used by the authorization server to sign the token (stored in a `.jks` file).
        - → The `.jks` file (containing both private and public keys) is shared with the resource server.
        - → The resource server uses the public key (from the `.jks` file) to verify the token's signature.

## 010.Security-how-to-rotate-tokens.md
✨ **How to Rotate Tokens**
- → Token rotation frequency depends on the use case.
- → For read/write applications (e.g., financial/banking), tokens are rotated every 5-10 minutes.
- → For read-only operations (e.g., LinkedIn feed), tokens can be rotated less frequently, such as once every six months or once a year.

## 011.Security-how-to-use-tokens-with-frontends.md
✨ **How to Use Tokens with Frontends**
- → When working with JavaScript frontends using RESTful APIs through OAuth, the OAuth token was stored on the server hosting the frontend.
- → In cases where the token had to be sent to the web browser, TLS (Transport Layer Security) was used to ensure no unwanted access.

## 012.Security-what-is-csrf.md
✨ **What is CSRF**
- → CSRF stands for Cross-Site Request Forgery.
- → Occurs when a malicious website tricks a user's browser into sending a forged request to another trusted website where the user is authenticated.
- → The malicious site can steal cookies set by the trusted website and use them to forge a session, sending requests and potentially stealing user data.

## 013.Security-how-to-prevent-csrf.md
✨ **How to Prevent CSRF**
- → When using Spring Security for web applications, CSRF is automatically prevented.
- → Spring Security includes a `CSRF filter` that generates a token and includes it in every web page sent back to the client.
- → When the client submits a form or request, the CSRF token must be sent back to the server.
- → If the token is missing or invalid, the CSRF filter rejects the request.
- → This prevents malicious websites from forging requests, as they will not have the valid CSRF token generated by the legitimate application.

## 014.Security-what-is-cors.md
✨ **What is CORS**
- → CORS stands for Cross-Origin Resource Sharing.
- → It addresses issues that arise when a frontend application (running on one domain/port) tries to communicate with a RESTful API backend (running on a different domain/port).
- → Without CORS configuration, cross-origin request errors will occur.
- → Spring allows configuring CORS using annotations like `@CrossOrigin`.
- → CORS allows configuring security at the origin level, specifying allowed HTTP methods, and allowed HTTP headers.
- → Using `@CrossOrigin` annotation, you can specify:
    - → Which frontend application (running on which domain) can communicate with the backend (the origin).
    - → Which HTTP methods (e.g., GET, PUT, POST) are allowed from this CORS origin.
    - → What HTTP headers can be included when a CORS origin request comes in.
