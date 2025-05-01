---
date: "2025-04-30T16:13:17+01:00"
draft: false
title: "🚀 Unlocking API Endpoints in Power Automate with HTTP and Entra ID"
tags:
  - Power Automate
  - Graph
---

## TL;DR - Watch the Video Tutorial

{{< youtube Cs-iQBPpCXI >}}

## 🧩 The Question: Can I leverage exsiting connectors to make Graph API calls?

Recently, I found myself needing to query the [Teams Templates API](https://learn.microsoft.com/en-us/graph/api/teamsappinstallation-get?view=graph-rest-beta&tabs=http) via Power Automate. I wanted to try and use out of the box connectors, but found that specific endpoints were blocked.

Using the standard **“Send a Microsoft Graph HTTP Request”** connector? No luck. The request would consistently fail:

> ❌ "URI path is not a valid Graph endpont."

![Standard Connector Error](/http-for-entra/failed_request.png)

Sure, I could have created an **App Registration**, but managing client secrets and access tokens adds overhead I wasn’t looking for. Thankfully, there’s a better way.

---

## ✅ The Solution: HTTP with Entra ID Connector

Enter: the **HTTP with Entra ID connector**. This connector has flown under the radar for me and allows you to make any API request within Microsoft 365 if used correctly.

There are **two flavors** of this connector:

1. **Preauthorized Connector** – Comes with a fixed set of pre-consented scopes.
2. **Customizable Connector** – Lets you configure your own permissions (via PowerShell).

---

## 🔍 Step-by-Step: Enable API Access Without App Registrations

### 1. Add the HTTP with Entra ID Connector

Choose either the preauthorized or customizable variant. If you're targeting non-standard scopes like `TeamTemplates.Read`, you'll need the **customizable** version.

> **Important**: This requires a **Premium License** and **Global Admin** approval.

### 2. Run Your First Request

Use a simple test call like:

GET https://graph.microsoft.com/v1.0/me

This verifies the delegated identity works. If successful, you’re authenticated!

### 3. Try the Beta Teams Templates Endpoint

Input something like:

GET https://graph.microsoft.com/beta/teamsTemplates

If this fails (as expected), it means that scope (`TeamTemplates.Read`) isn’t yet granted.

![HTTP With Microsoft Entra](/http-for-entra/http_with_entra.png)

---

## 🛠️ Granting Permissions via PowerShell

Microsoft provides a [PowerShell script](https://github.com/microsoft/PowerApps-Samples/blob/master/powershell/connectors/HTTPWithMicrosoftEntraId/ManagePermissionGrant.ps1) that simplifies updating app permissions.

![Permissions Grant via PowerShell](/http-for-entra/powershell_script.png)

📌 **Key Tips:**

- Use **Ctrl+Click** to select multiple scopes — or you’ll overwrite previous ones.
- Choose **“Impersonate All Users”** with caution — ensure security via **DLP and Conditional Access**.

Once you grant the necessary scopes (e.g., `TeamTemplates.Read.All`), your flow should now work like magic.

---

## ✅ Success! API Access Unlocked

After assigning the correct permissions and re-testing:

![Successful Call](/http-for-entra/successful_request.png)

You’ll be able to successfully call the previously blocked Graph endpoint.

Note: This process can be repeated for multipe graph requests, or any other available API.

---

## ⚠️ Things to Watch Out For

- These are **delegated permissions**, not application-level.
- **MFA** or **conditional access** policies might interfere.
- Ensure your **environment’s DLP policies** don’t allow data leakage.

---

## 📚 Resources

- 📄 [Official Docs – HTTP with Entra ID](https://learn.microsoft.com/en-us/connectors/webcontentsv2/)
- 🧑‍💻 [Timo Pertilä Blog](https://forwardforever.com/what-is-the-difference-between-http-with-microsoft-entra-id-and-http-with-microsoft-entra-id-preauthorized-connectors/)
- 🛠️ [Davids Blog](https://dev.to/wyattdave/power-automate-the-super-connector-3gbe)

---

## 🎯 Final Thoughts

This technique is a brilliant workaround when you don’t want to dive into full-blown app registrations. Whether you’re building lightweight flows or experimenting with Microsoft Graph’s beta APIs, **HTTP with Entra ID** could suit your needs.

Let me know what you build with it — and stay tuned for more Power Platform tricks.
