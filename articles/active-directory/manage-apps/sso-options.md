---
title: Single sign-on options in Azure AD
description: Learn about the options available for single sign-on (SSO) in Azure Active Directory.
services: active-directory
author: davidmu1
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 12/03/2019
ms.author: davidmu
ms.reviewer: ergreenl
---

# Single sign-on options in Azure AD

Single sign-on provides many benefits over traditional sign-on methods.

- **With single sign-on**, users sign in once with one account to access domain-joined devices, company resources, software as a service (SaaS) applications, and web applications. After signing in, the user can launch applications from the Office 365 portal or My Apps. Administrators can centralize user account management, and automatically add or remove user access to applications based on group membership.

- **Without single sign-on**, users must remember application-specific passwords and sign in to each application. IT staff needs to create and update user accounts for each application such as Microsoft 365, Box, and Salesforce. Users need to remember their passwords, plus spend the time to sign in to each application.

To learn more about single sign-on, see [What is single sign-on?](what-is-single-sign-on.md).

## Choosing a single sign-on method

There are several ways to configure an application for single sign-on. Choosing a single sign-on method depends on how the application is configured for authentication.

- Cloud applications can use OpenID Connect, OAuth, SAML, password-based, linked, or disabled methods for single sign-on.
- On-premises applications can use password-based, Integrated Windows Authentication, header-based, linked, or disabled methods for single sign-on. The on-premises choices work when applications are configured for Application Proxy.

This flowchart helps you decide which single sign-on method is best for your situation.

![Decision flowchart for single sign-on method](./media/what-is-single-sign-on/choose-single-sign-on-method-040419.png)

The following table summarizes the single sign-on methods, and links to more details.

| Single sign-on method | Application types | When to use |
| :------ | :------- | :----- |
| [OpenID Connect and OAuth](#openid-connect-and-oauth) | cloud and on-premises | Use OpenID Connect and OAuth when developing a new application. This protocol simplifies application configuration, has easy-to-use SDKs, and enables your application to use MS Graph.
| [SAML](#saml-sso) | cloud and on-premises | Choose SAML whenever possible for existing applications that do not use OpenID Connect or OAuth. SAML works for applications that authenticate using one of the SAML protocols.|
| [Password-based](#password-based-sso) | cloud and on-premises | Choose password-based when the application authenticates with username and password. Password-based single sign-on enables secure application password storage and replay using a web browser extension or mobile app. This method uses the existing sign-in process provided by the application, but enables an administrator to manage the passwords. |
| [Linked](#linked-sign-on) | cloud and on-premises | Choose linked sign-on when the application is configured for single sign-on in another identity provider service. This option doesn't add single sign-on to the application. However, the application might already have single sign-on implemented using another service such as Active Directory Federation Services.|
| [Disabled](#disabled-sso) | cloud and on-premises | Choose disabled single sign-on when the app isn't ready to be configured for single sign-on. This mode is the default when you create the app.|
| [Integrated Windows Authentication (IWA)](#integrated-windows-authentication-iwa-sso) | on-premises only | Choose IWA single sign-on for applications that use [Integrated Windows Authentication (IWA)](/aspnet/web-api/overview/security/integrated-windows-authentication), or claims-aware applications. For IWA, the Application Proxy connectors use Kerberos Constrained Delegation (KCD) to authenticate users to the application. |
| [Header-based](#header-based-sso) | on-premises only | Use header-based single sign-on when the application uses headers for authentication. Application Proxy uses Azure AD to authenticate the user and then passes traffic through the connector service.  |

## OpenID Connect and OAuth

When developing new applications, use modern protocols like OpenID Connect and OAuth to achieve the best single sign-on experience for your app across multiple device platforms. OAuth enables users or admins to [grant consent](configure-user-consent.md) for protected resources like [Microsoft Graph](/graph/overview). We provide easy to adopt [SDKs](../develop/reference-v2-libraries.md) for your app, and additionally, your app will be ready to use [Microsoft Graph](/graph/overview).

For more information, see:

- [OAuth 2.0](../develop/v2-oauth2-auth-code-flow.md)
- [OpenID Connect 1.0](../develop/v2-protocols-oidc.md)
- [Microsoft identity platform developer’s guide](../develop/index.yml).

## SAML SSO

With **SAML single sign-on**, Azure AD authenticates to the application by using the user's Azure AD account. Azure AD communicates the sign-on information to the application through a connection protocol. With SAML-based single sign-on, you can map users to specific application roles based on rules you define in your SAML claims.

Choose SAML-based single sign-on when the application supports it.

SAML-based single sign-on is supported for applications that use any of these protocols:

- SAML 2.0
- WS-Federation

To configure a SaaS application for SAML-based single sign-on, see [Configure SAML-based single sign-on](configure-saml-single-sign-on.md). Also, many Software as a Service (SaaS) applications have an [application-specific tutorial](../saas-apps/tutorial-list.md) that step you through the configuration for SAML-based single sign-on.

To configure an application for WS-Federation, follow the same guidance to configure application for SAML-based single sign-on. In the step to configure the application to use Azure AD, you will need to replace the Azure AD login URL for the WS-Federation end-point `https://login.microsoftonline.com/<tenant-ID>/wsfed`.

To configure an on-premises application for SAML-based single sign-on, see [SAML single-sign-on for on-premises applications with Application Proxy](../app-proxy/application-proxy-configure-single-sign-on-on-premises-apps.md).

For more information about the SAML protocol, see [Single sign-on SAML protocol](../develop/single-sign-on-saml-protocol.md).

## Password-based SSO

With password-based sign-on, users sign on to the application with a username and password the first time they access it. After the first sign-on, Azure AD supplies the username and password to the application.

Password-based single sign-on uses the existing authentication process provided by the application. When you enable password single sign-on for an application, Azure AD collects and securely stores user names and passwords for the application. User credentials are stored in an encrypted state in the directory.

Additionally, Administrator can enable Azure AD conditional access policies or multi-factor authentication for password-based SSO.

Choose password-based single sign-on when:

- An application doesn't support SAML single sign-on protocol.
- An application authenticates with a username and password instead of access tokens and headers.

Password-based single sign-on is supported for any cloud-based application that has an HTML-based sign-in page. The user can use any of the following browsers:

- Internet Explorer 11 on Windows 7 or later
   > [!NOTE]
   > Internet Explorer is on limited support and no longer receives new software updates. Microsoft Edge is the recommended browser.

- Microsoft Edge on Windows 10 Anniversary Edition or later
- Microsoft Edge for iOS and Android
- Intune Managed Browser
- Chrome on Windows 7 or later, and on macOS X or later
- Firefox 26.0 or later on Windows XP SP2 or later, and on macOS X 10.6 or later

To configure a cloud application for password-based single sign-on, see [Configure password single sign-on](configure-password-single-sign-on-non-gallery-applications.md).

To configure an on-premises application for single sign-on through Application Proxy, see [Password vaulting for single sign-on with Application Proxy](../app-proxy/application-proxy-configure-single-sign-on-password-vaulting.md)

### How authentication works for password-based SSO

To authenticate a user to an application, Azure AD retrieves the user's credentials from the directory and enters them into the application's sign-on page.  Azure AD securely passes the user credentials via a web browser extension or mobile app. This process enables an administrator to manage user credentials, and doesn't require users to remember their password.

> [!IMPORTANT]
> The credentials are obfuscated from the user during the automated sign-on process. However, the credentials are discoverable by using web-debugging tools. Users and administrators need to follow the same security policies as if credentials were entered directly by the user.

### Managing credentials for password-based SSO

Passwords for each application can either be managed by the Azure AD administrator or by the users.

When the Azure AD administrator manages the credentials:  

- The user doesn't need to reset or remember the user name and password. The user can access the application by clicking on it in their My Apps or via a provided link.
- The administrator can do management tasks on the credentials. For example, the administrator can update application access according to user group memberships and employee status.
- The administrator can use administrative credentials to provide access to applications shared among many users. For example, the administrator can allow everyone who can access an application to have access to a social media or document sharing application.

When the end user manages the credentials:

- Users can manage their passwords by updating or deleting them as needed.
- Administrators are still able to set new credentials for the application.

## Linked sign-on

Linked sign-on enables Azure AD to provide single sign-on to an application that is already configured for single sign-on in another service. The linked application can appear to end users in the Office 365 portal or Azure AD MyApps portal. For example, a user can launch an application that is configured for single sign-on in Active Directory Federation Services 2.0 (AD FS) from the Office 365 portal. Additional reporting is also available for linked applications that are launched from the Office 365 portal or the Azure AD MyApps portal. To configure an application for linked sign-on, see [Configure linked sign-on](configure-linked-sign-on.md).

### Linked sign-on for application migration

Linked sign-on can provide a consistent user experience while you migrate applications over a period of time. If you're migrating applications to Azure Active Directory, you can use linked sign-on to quickly publish links to all the applications you intend to migrate.  Users can find all the links in the [MyApps portal](../user-help/my-apps-portal-end-user-access.md) or the [Microsoft 365 application launcher](https://support.office.com/article/meet-the-office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a). Users won't know they're accessing a linked application or a migrated application.  

Once a user has authenticated with a linked application, an account record needs to be created before the end user is provided single sign-on access. Provisioning this account record can either occur automatically, or it can occur manually by an administrator.

>[!NOTE]
>You cannot apply conditional access policies or multi-factor authentication to a linked application. This is because a linked application does not provide single sign-on capabilities through Azure AD. When you configure a linked application you are simply adding a link that will appear in the app launcher or MyApps portal.

## Disabled SSO

Disabled mode means single sign-on isn't used for the application. When single sign-on is disabled, users might need to authenticate twice. First, users authenticate to Azure AD, and then they sign in to the application.

Use disabled single sign-on mode:

- If you're not ready to integrate this application with Azure AD single sign-on, or
- If you're testing other aspects of the application, or
- As a layer of security to an on-premises application that doesn't require users to authenticate. With disabled, the user needs to authenticate.

Note that if you have configured the application for SP-initiated SAML based single sign-on and you change the SSO mode to disable, it won't stop users from signing to the application outside the MyApps portal. To achieve this, you need to [disable the ability for users to sign-in](disable-user-sign-in-portal.md)

## Integrated Windows Authentication (IWA) SSO

[Application Proxy](../app-proxy/application-proxy.md) provides single sign-on (SSO) to applications that use [Integrated Windows Authentication (IWA)](/aspnet/web-api/overview/security/integrated-windows-authentication), or claims-aware applications. If your application uses IWA, Application Proxy authenticates to the application by using Kerberos Constrained Delegation (KCD). For a claims-aware application that trusts Azure Active Directory, single sign-on works because the user was already authenticated by using Azure AD.

Choose Integrated Windows Authentication single sign-on mode to provide single sign-on to an on-premises app that authenticates with IWA.

To configure an on-premises app for IWA, see [Kerberos Constrained Delegation for single sign-on to your applications with Application Proxy](../app-proxy/application-proxy-configure-single-sign-on-with-kcd.md).

### How single sign-on with KCD works

This diagram explains the flow when a user accesses an on-premises application that uses IWA.

![Microsoft Azure AD authentication flow diagram](../app-proxy/media/application-proxy-configure-single-sign-on-with-kcd/AuthDiagram.png)

1. The user enters the URL to access the on premises application through Application Proxy.
1. Application Proxy redirects the request to Azure AD authentication services to preauthenticate. At this point, Azure AD applies any applicable authentication and authorization policies, such as multifactor authentication. If the user is validated, Azure AD creates a token and sends it to the user.
1. The user passes the token to Application Proxy.
1. Application Proxy validates the token and retrieves the User Principal Name (UPN) from the token. It then sends the request, the UPN, and the Service Principal Name (SPN) to the Connector through a dually authenticated secure channel.
1. The connector uses Kerberos Constrained Delegation (KCD) negotiation with the on premises AD, impersonating the user to get a Kerberos token to the application.
1. Active Directory sends the Kerberos token for the application to the connector.
1. The connector sends the original request to the application server, using the Kerberos token it received from AD.
1. The application sends the response to the connector, which is then returned to the Application Proxy service and finally to the user.

## Header-based SSO

Header-based single sign-on works for applications that use HTTP headers for authentication.

Choose header-based single sign-on when Application Proxy is configured for the on-premises application.

To learn more about header-based authentication, see [Header-based SSO](../app-proxy/application-proxy-configure-single-sign-on-with-headers.md).

## Next steps

- [Quickstart Series on Application Management](view-applications-portal.md)
- [Plan a single sign-on deployment](plan-sso-deployment.md)
- [Single sign-on with on-premises apps](../app-proxy/application-proxy-config-sso-how-to.md)
