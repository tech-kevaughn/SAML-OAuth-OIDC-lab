# IAM Federation Lab: SAML 2.0, OIDC, and OAuth 2.0 with Auth0

## Overview
This project demonstrates hands-on identity federation using Auth0. I built and tested SAML 2.0 SSO, OpenID Connect authentication, and OAuth 2.0 authorization flows.

## Tools Used
- Auth0
- SAML Testing Tool
- OIDC Debugger
- JWT.io
- Hoppscotch

## Lab 1: SAML 2.0 SSO
I configured Auth0 as the Identity Provider and a SAML testing application as the Service Provider.

![SAML Auth0 Addon](saml-oauth0-addon.png)

Key concepts:
- Identity Provider
- Service Provider
- ACS URL
- Entity ID
- SAML metadata
- X.509 certificate
- SAML assertion

## Lab 2: OpenID Connect
I configured an OIDC application in Auth0 and tested authentication using OIDC Debugger.

![OIDC Debugger](debugger-request.png)

Key concepts:
- OpenID Provider
- Relying Party
- ID token
- JWT claims
- Redirect URI
- Scopes

## Lab 3: OAuth 2.0
I configured a custom API in Auth0 and tested OAuth authorization using access tokens.

![OAuth API](oauth-api-permissions.png)

Key concepts:
- Resource server
- Client credentials flow
- Access token
- Audience
- Scopes
- Machine-to-machine authorization

## What I Learned
This lab helped me understand the difference between authentication and authorization.

SAML and OIDC are mainly used for authentication and SSO. OAuth 2.0 is used for authorization and API access.

## Security Note
All sensitive values were redacted before uploading, including client secrets, tokens, authorization codes, certificates, and personal information.
