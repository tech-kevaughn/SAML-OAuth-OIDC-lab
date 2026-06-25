# IAM Federation Lab Report  
**Project:** SAML 2.0, OpenID Connect, and OAuth 2.0 with Auth0  
**Author:** Kevaughn Simpson  
**Focus Area:** Identity and Access Management (IAM), Federation, SSO, Authentication, Authorization  

---

# 1. Executive Summary

This lab was designed to build practical hands-on experience with three major IAM and federation protocols:

- **SAML 2.0**
- **OpenID Connect (OIDC)**
- **OAuth 2.0**

Using Auth0 as the identity platform, I configured and tested real authentication and authorization flows with external tools including a SAML testing service, OIDC Debugger, JWT.io, and an OAuth/API testing workflow.

The purpose of the lab was to understand the **technical differences between authentication and authorization**, how trust is established between identity systems and applications, how identity claims are delivered, and how tokens or assertions are structured and used.

By the end of the lab, I was able to:

- Configure a **SAML service provider integration** using Auth0 as the Identity Provider
- Exchange **SAML metadata** and inspect the SAML assertion / user attributes
- Configure an **OIDC application** and retrieve an **ID token**
- Decode JWTs and identify key claims such as `iss`, `aud`, `sub`, and `email`
- Configure an **OAuth 2.0 machine-to-machine flow** using a custom API in Auth0
- Request and inspect an **access token**
- Understand the functional differences between **SAML, OIDC, and OAuth 2.0**
- Troubleshoot authorization and resource server errors during OAuth configuration

---

# 2. Lab Objectives

The main objectives of this lab were to:

1. Understand how **SAML 2.0** works in an SP-initiated SSO scenario
2. Understand how **OIDC** returns user identity information through ID tokens
3. Understand how **OAuth 2.0** grants API access through access tokens rather than user authentication
4. Learn how Auth0 can act as:
   - an **Identity Provider (IdP)** for SAML
   - an **OpenID Provider (OP)** for OIDC
   - an **Authorization Server** for OAuth 2.0
5. Compare the role of:
   - **SAML assertions**
   - **OIDC ID tokens**
   - **OAuth access tokens**
6. Gain hands-on experience with IAM concepts that are directly relevant to real-world SSO and federation engineering roles

---

# 3. Lab Environment and Tools Used

## Identity Platform
- **Auth0**

## SAML Tools
- **SAML Testing Tool / samlsp.com**
- SAML metadata XML
- Auth0 SAML2 Web App addon

## OIDC Tools
- **OIDC Debugger**
- **JWT.io**

## OAuth Tools
- **Auth0 custom API / resource server configuration**
- API permission / scope configuration
- token inspection with JWT.io

---

# 4. SAML 2.0 

## 4.1 Objective
The objective of the SAML lab was to configure Auth0 as the **Identity Provider (IdP)** and a SAML test application as the **Service Provider (SP)**, then perform a successful SP-initiated login and inspect the resulting user attributes.

## 4.2 Architecture
- **Identity Provider (IdP):** Auth0
- **Service Provider (SP):** SAML testing application

## 4.3 Configuration Steps Performed
I completed the following high-level tasks:

1. Opened the SAML testing tool and collected the **ACS URL** and **Audience / Entity ID**
2. Created an application in Auth0 for the SAML SP integration
3. Enabled the **SAML2 Web App addon**
4. Added the **Assertion Consumer Service URL** from the SP into Auth0
5. Downloaded the **Identity Provider Metadata XML** from Auth0
6. Uploaded the Auth0 metadata into the SAML testing tool
7. Verified that the SP auto-populated:
   - IdP Entity ID
   - Login URL
   - X.509 certificate
8. Initiated SAML login and authenticated through Auth0
9. Reviewed the returned SAML attributes in the SAML testing tool

## 4.4 What I Observed
This lab clearly showed how **SAML trust relationships** are built through **metadata exchange**. Instead of manually typing every endpoint and certificate, the metadata XML allowed the service provider to import the IdP configuration automatically.

The successful SAML flow returned user attributes such as:
- email address
- name
- nameidentifier / subject identifier
- additional Auth0 user attributes

## 4.5 IAM Concepts Reinforced
This lab reinforced the following IAM concepts:

- **Identity Provider (IdP)** vs **Service Provider (SP)**
- **ACS URL** (Assertion Consumer Service URL)
- **Entity ID / Audience**
- **Metadata exchange**
- **X.509 signing certificates**
- **SAML assertions**
- **attribute statements**
- **SP-initiated SSO**

## 4.6 Key Takeaway
SAML is heavily based on **XML**, **metadata files**, **assertions**, and **certificate trust**. It is commonly used for enterprise SSO integrations and legacy or enterprise SaaS applications.

---

# 5. OpenID Connect (OIDC) 

## 5.1 Objective
The objective of the OIDC lab was to configure an Auth0 application, authenticate through an OIDC flow, receive an **ID token**, and inspect the JWT claims returned by the identity provider.

## 5.2 Architecture
- **OpenID Provider (OP):** Auth0
- **Relying Party / Client Application:** OIDC Debugger

## 5.3 Configuration Steps Performed
I completed the following tasks:

1. Created an **OIDC testing application** in Auth0
2. Configured the allowed callback / redirect URL for the debugger application
3. Used **OIDC Debugger** as the client application
4. Entered:
   - Auth0 authorization endpoint
   - Client ID
   - redirect URI
   - scopes such as `openid profile email`
5. Submitted the request and authenticated through Auth0
6. Received a successful response including OIDC tokens
7. Copied the **ID token**
8. Pasted the ID token into **JWT.io**
9. Decoded the JWT header and payload

## 5.4 What I Observed
The OIDC flow returned an **ID token** in JWT format. Unlike SAML, which returns an XML assertion, OIDC returns a lightweight token containing identity claims in JSON format.

I observed claims such as:
- `iss` → issuer
- `aud` → audience / client application
- `sub` → unique subject identifier
- `email`
- `email_verified`
- `iat`
- `exp`
- `nonce`

## 5.5 IAM Concepts Reinforced
This lab reinforced the following IAM concepts:

- **OpenID Provider**
- **Relying Party**
- **ID token**
- **JWT structure**
  - Header
  - Payload
  - Signature
- **Scopes**
  - `openid`
  - `profile`
  - `email`
- **Redirect URI**
- **Authentication vs token issuance**

## 5.6 Key Takeaway
OIDC is used for **authentication** and modern web/mobile login. It is lighter than SAML because it relies on **JSON Web Tokens (JWTs)** rather than XML assertions and metadata-heavy exchanges.

---

# 6. OAuth 2.0 

## 6.1 Objective
The objective of the OAuth 2.0 lab was to build a **machine-to-machine authorization flow** using **Client Credentials**, request an **access token** from Auth0, and understand how OAuth differs from OIDC.

## 6.2 Architecture
- **Authorization Server:** Auth0
- **Resource Server / API:** Custom API configured in Auth0
- **Client:** Machine-to-machine application

## 6.3 Configuration Steps Performed
I completed the following tasks:

1. Created a custom API in Auth0
2. Assigned an API identifier / audience
3. Created API permissions / scopes
4. Created a machine-to-machine application
5. Authorized the M2M application to access the custom API
6. Requested an access token from Auth0
7. Reviewed the token response
8. Decoded the access token using JWT tools

## 6.4 What I Observed
The OAuth flow returned an **access token** intended for API/resource access. Unlike the OIDC ID token, this token’s purpose was **authorization**, not end-user identity.

Important differences I observed:

- The token contained **audience** information for the target API
- The token contained **scope / permissions**
- The token was about **what the client can access**
- The token did **not** function as proof of a user login in the same way an OIDC ID token does

## 6.5 Troubleshooting Encountered
During the OAuth lab, I encountered an authorization error indicating that the client application was not authorized to access the target resource server / API.

This was useful because it forced me to understand an important IAM troubleshooting concept:

- a client application can exist
- an API can exist
- but the client still must be **explicitly authorized** to request tokens for that API and audience

This reinforced the relationship between:
- client application
- authorization server
- resource server
- scopes / permissions
- audience values

## 6.6 IAM Concepts Reinforced
This lab reinforced the following IAM concepts:

- **OAuth 2.0**
- **Client Credentials grant**
- **Machine-to-machine (M2M)**
- **Authorization server**
- **Resource server**
- **API audience**
- **Scopes / permissions**
- **Access token**
- **Client authorization to a resource server**

## 6.7 Key Takeaway
OAuth 2.0 is primarily about **authorization**, not authentication. It answers the question:

> “Is this application allowed to access this API and what scope of access does it have?”

It does **not** by itself prove a user identity in the way OIDC does.

---

# 7. SAML vs OIDC vs OAuth – My Technical Understanding After the Lab

## 7.1 SAML
SAML is an **enterprise federation protocol** used primarily for **SSO** between an Identity Provider and a Service Provider. It uses:

- XML
- SAML assertions
- metadata exchange
- certificates
- browser redirects / POSTs

### In simple terms:
SAML is a way for an application to trust an external identity provider and accept the user’s authenticated identity through an assertion.

---

## 7.2 OIDC
OIDC is a modern identity layer built on top of OAuth 2.0 and is used for **authentication**. It returns **ID tokens** containing user identity claims.

### In simple terms:
OIDC answers:
> “Who is the user that just authenticated?”

It is typically used for:
- web apps
- mobile apps
- modern SSO
- social login / customer identity platforms

---

## 7.3 OAuth 2.0
OAuth 2.0 is an authorization framework that issues **access tokens** so clients can access protected resources.

### In simple terms:
OAuth answers:
> “Is this client allowed to access this API, and what is it allowed to do?”

OAuth by itself is **not** the same as user authentication.

---

## 7.4 The biggest distinction I learned

### SAML
Focuses on **federated authentication / SSO** using assertions.

### OIDC
Focuses on **authentication and identity claims** using ID tokens.

### OAuth 2.0
Focuses on **authorization and API access** using access tokens.

---

# 8. Security and IAM Lessons Learned

This lab taught me several important security and IAM lessons:

## 8.1 Redirect URIs and callback URLs matter
A misconfigured redirect URI can break authentication flows or create security risk.

## 8.2 Audience values matter
OAuth access tokens must be requested for the correct API / resource server audience.

## 8.3 Client-to-API authorization must be explicitly configured
Just because an app exists does not mean it can automatically request tokens for every API.

## 8.4 Claims inspection is essential for troubleshooting
Being able to inspect:
- SAML attributes
- JWT claims
- issuer values
- audience values
- subject identifiers
is a core IAM troubleshooting skill.

## 8.5 Metadata and certificates are critical in SAML
SAML relies much more heavily on metadata exchange and certificate trust than OIDC.

---

# 9. How This Lab Maps to Real IAM / Cybersecurity Work

This project maps directly to real IAM engineering and analyst responsibilities such as:

- onboarding enterprise applications to SSO
- troubleshooting SAML/OIDC login failures
- validating callback URLs and redirect URIs
- reviewing claims and user attributes
- configuring scopes and resource server access
- understanding how authentication and authorization differ
- working with federation metadata, tokens, and assertions
- supporting CIAM / workforce identity integrations

It also maps well to **SC-300 concepts**, especially:
- identity providers and relying parties
- authentication protocols
- SSO and federation
- application integrations
- token and claim inspection

---

# 10. Conclusion

This IAM federation lab gave me practical experience with three foundational identity protocols: **SAML 2.0, OpenID Connect, and OAuth 2.0**.

The biggest value of the lab was not just “making the login work,” but understanding **why** each protocol exists, **what artifact it returns** (assertion vs ID token vs access token), **what problem it solves**, and **how to troubleshoot it**.

If I were asked to explain the difference in an interview today, I would summarize it like this:

- **SAML** → enterprise SSO using XML assertions and metadata exchange  
- **OIDC** → authentication and identity claims using ID tokens / JWTs  
- **OAuth 2.0** → authorization and API access using access tokens  

This lab strengthened my understanding of federation, SSO, identity claims, scopes, resource access, and the operational work involved in IAM application onboarding and troubleshooting.

---

# 11. Future Improvements

If I continue building this project, I would extend it by adding:

- **Microsoft Entra ID** as a second identity provider
- **federated identity chaining** between providers
- **MFA / Conditional Access style policies**
- **SCIM provisioning**
- **group / role claims mapping**
- **custom claims in OIDC tokens**
- **more advanced OAuth flows** such as Authorization Code + PKCE
- **a troubleshooting guide for common federation errors**
