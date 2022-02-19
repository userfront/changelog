# Userfront changelog

Additions and updates to the Userfront platform

## January 2022

### Improved user search endpoint and documentation

Added search options, fixed some minor display bugs, and improved the documentation for user search. Added examples of advanced filtering with various combinations of `and` and `or` search queries.

### Custom link durations

The [Generate Link](https://userfront.com/docs/api.html#generate-link-credentials), [Invite User](https://userfront.com/docs/api.html#invite-user) and [Invite User to a role](https://userfront.com/docs/api.html#invite-user-to-a-role) endpoints all generate email links with a set expiration by default:

| options.type | Default duration |
| :----------- | :--------------- |
| login        | 1 hour           |
| welcome      | 3 days           |
| verify       | 3 days           |
| reset        | 1 hour           |

Now you can also specify custom durations for each type of link, up to 1 week and down to 10 seconds, using the `options.duration` parameter.

```
{
  options: {
    duration: "5 minutes"
  }
}
```

### Invite user with immediate password set

By default, invite emails contain a login link that will log the user in directly. However, this flow does not ask the user to set a password right away. Now it is possible to use a password reset link in place of a login link in the invite email by using the `options.type` parameter.

```
{
  options: {
    type: "reset"
  }
}
```

## December 2021

### Add API endpoints to create, invalidate, and delete API keys

Programmatically create, invalidate, and delete API keys: `admin`, `readonly`, and `webhook` key types are supported. Rotate keys as needed to secure your application.

### Allow `test`, `dev`, and `password` as passwords in test mode

Now in test mode you can use `test`, `dev`, and `password` when registering a user or changing their password. This allows for faster testing.

### Streamline SSO signup webhooks

Previously, user registration by SSO would fire a `user created` webhook with the user unconfirmed, followed closely by a `user updated` webhook with the user confirmed. Now only the `user created` webhook is fired, with the user already confirmed.

### Improved certificate generation

Simplified the process for generating certificates for live domains, and removed a bug that affected some subdomains when "include subdomains" was checked.

## November 2021

### SAML beta release

3rd party login with SAML is stable and in limited-party testing in advance of a public release.

### Twitter SSO initial spec

Adding Oauth 2.0 login with Twitter, which is based on the Twitter 2 API (in beta). We are waiting for Twitter to implement some features such as profile lookup before we can publish Oauth 2.0 compliant login via Twitter.

### Apple SSO initial spec

Adding "Sign in with Apple" functionality, which is loosely based on Oauth 2.0 standard. Awaiting some confirmation from Apple regarding their long-term timeline before finalizing this implementation for all platforms.

### Webhook sends for user lock/unlock

Whenever a user is locked or unlocked, the User Updated Webhook will be sent.

## October 2021

### Link generation without email

Use a server-to-server API endpoint to generate link credentials (`uuid` and `token`) for use in custom login, welcome, and account verification emails.

### Signup option: `noSignupEmail`

Works the same as standard signup, but with the the included `noSignupEmail: true` flag, Userfront does not send the new user an email. Combined with "Link generation without email" above, this allows you to create fully custom signup flows.

### Include roles as a filter in user search

Filter users by the roles they have in a given tenant.

### Improved client-to-server documentation

Updated code examples for client-to-server docs.

## September 2021

### Add documentation for passwordless flows

Added documentation for "passwordless" flows to give a clearer picture of all the options for signup and login.

### Add endpoint to allow users to update their own password with a valid JWT access token

Users can now present a valid JWT access token, existing password, and the new password to update their password directly from the client.

### Consistent response format for link generating endpoints

Updated the response format for newer link generation endpoints to better match other API endpoints.

Old format (deprecated):

```
{
  Message: 'OK',
  To: 'basic-24942@example.com'
  MessageID: '12ac72f3-355f-4266-8c7d-05d0acdab703',
  SubmittedAt: 2021-09-12T17:21:48.046Z,
}
```

Updated format:

```
{
  message: 'OK',
  result: {
    to: 'basic-24942@example.com',
    messageId: '12ac72f3-355f-4266-8c7d-05d0acdab703',
    submittedAt: 2021-09-12T17:21:48.046Z
  }
}
```

## August 2021

### Include data attributes in user search

Allow search by users' custom data attributes via the user search endpoint.

### Add `httpOnly` option for refresh tokens

Add system and endpoints for creating and using refresh tokens in `httpOnly` mode. These tokens cannot be read by client-side JavaScript and are thus not susceptible to XSS attacks. Usage of `httpOnly` tokens requires a custom domain.

### Custom JWT access token formats

Added the ability to generate custom JWT access token formats (e.g. `compact`, `verbose`, `OIDC`) by manuallys setting a value for the tenant. We will eventually add the ability to configure this directly from the dashboard.

## July 2021

### Tenant deletion by API

Added an API endpoint and webhook to delete tenants programmatically.

### Updated JWT key pair signing configuration

Changed the key pair signing configuration from `pkcs1` to `pkcs8` as the former has compatibility issues with certain PHP libraries. All future keys will use the new format, and existing keys can be rotated to use the new format if needed.

### Added examples for Vue.js and Node.js

Included example projects for setting up Userfront with Vue.js (custom forms) and Node.js authentication + access control.
