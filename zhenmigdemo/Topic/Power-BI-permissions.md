---
title: Power BI permissions
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b19c5d1f-7d7a-4772-a9cd-66abad2bebe6
robots: noindex,nofollow
---
# Power BI permissions
---
Power BI permissions give an application the ability to take certain actions on a user's behalf. All permissions must be approved by a user in order to be valid.

<table><tr><td><b>Display Name</b></td><td><b>Description</b></td><td><b>Scope Value</b></td></tr><tr><td>View all Datasets</td><td>The app can view all datasets for the signed in user and datasets that the user has access to.</td><td>Dataset.Read.All</td></tr><tr><td>Read and Write all Datasets</td><td>The app can view and write to all datasets for the signed in user and datasets that the user has access to.</td><td>Dataset.ReadWrite.All</td></tr>
	<tr><td>View users Groups</td><td>The app can view all groups that the signed in user belongs to.</td><td>Group.Read.All</td></tr>
    <tr><td>View all Dashboards (preview)</td><td>The app can view all dashboards for the signed in user and dashboards that the user has access to.</td><td>Dashboard.Read.All</td></tr>
	</table>

An application can request permissions when it first attempts to log in to a user's page by passing in the requested permissions in the scope parameter of the call. If the permissions are granted, an access token will be returned to the app which can be used on future API calls. The access can only be used by a specific application.

## Requesting Permissions ##
While you can call the API to authenticate with a username and password, in order to take actions on behalf of another user, they will need to request permissions that the user then approves and then send the resulting access token on all future calls. For this process, we will follow the standard [OAuth 2.0](http://oauth.net/2/) protocol. While the actual implementation may vary, the OAuth flow for Power BI has the following elements:

- **Login UI** - This is a UI that the developer can evoke to request permissions. It would require the user to log in if not already. The user would also need to approve the permissions that the application is requesting. The login window will post back either an access code or an error message to a redirect URL that is supplied.
	- A standard redirect URL should be supplied by Power BI for use by native applications.
- **Authorization Code** - Authorization Codes are returned to web applications after login via URL parameters in the redirect URL. Since they are in parameters there is some security risk. Web applications will have to exchange the authorization code for an Authorization Token
- **Authorization Token** - Are used to authenticate API calls on another user's behalf. They will be scoped to a specific application. Tokens have a set lifespan and when they expire they will need to be refreshed.
- **Refresh Token** - When tokens expire there will be a process of refreshing them.
