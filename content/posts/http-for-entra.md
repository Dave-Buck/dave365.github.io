---
date: "2025-04-30T16:13:17+01:00"
draft: false
title: "🚀 Unlocking Hidden Microsoft Graph API Endpoints in Power Automate with HTTP and Entra ID"
---

## TL;DR - Watch the Video Tutorial

{{< youtube Cs-iQBPpCXI >}}

## 🧩 The Question: Can I leverage exsiting connectors to make Graph API calls?

Recently, I found myself needing to query the [Teams Templates API](https://learn.microsoft.com/en-us/graph/api/teamsappinstallation-get?view=graph-rest-beta&tabs=http) via Power Automate. The goal was simple — call the Graph beta endpoint to get available Teams templates.

Using the standard **“Send HTTP request to Graph”** connector? No luck. The request would consistently fail:

> ❌ "The endpoint isn't supported by this connector."

![Standard Connector Error](screenshots/screenshot_0-00-50.png)

Sure, I could have created an **App Registration**, but managing client secrets and access tokens adds overhead I wasn’t looking for. Thankfully, there’s a better way.

---

## ✅ The Solution: HTTP with Entra ID Connector

Enter: the **HTTP with Entra ID connector**. This under-the-radar gem allows you to make authenticated HTTP requests — even to endpoints not officially supported.

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

---

## 🛠️ Granting Permissions via PowerShell

Microsoft provides a [PowerShell script](https://learn.microsoft.com/en-us/power-platform/power-automate/http-with-entra-id#configure-api-permissions) that simplifies updating app permissions.

![Permissions Grant via PowerShell](screenshots/screenshot_0-02-50.png)

📌 **Key Tips:**

- Use **Ctrl+Click** to select multiple scopes — or you’ll overwrite previous ones.
- Choose **“Impersonate All Users”** with caution — ensure security via **DLP and Conditional Access**.

Once you grant the necessary scopes (e.g., `TeamTemplates.Read.All`), your flow should now work like magic.

---

## ✅ Success! API Access Unlocked

After assigning the correct permissions and re-testing:

![Successful Call](screenshots/screenshot_0-06-40.png)

You’ll be able to successfully call the previously blocked Graph endpoint.

---

## ⚠️ Things to Watch Out For

- These are **delegated permissions**, not application-level.
- **MFA** or **conditional access** policies might interfere.
- Ensure your **environment’s DLP policies** don’t allow data leakage.

---

## 📚 Resources

- 📄 [Official Docs – HTTP with Entra ID](https://learn.microsoft.com/en-us/power-platform/power-automate/http-with-entra-id)
- 🧑‍💻 [Timo Pertilä Blog](https://some-example.com)
- 🛠️ [PowerShell Script from Docs](https://learn.microsoft.com/en-us/power-platform/power-automate/http-with-entra-id#configure-api-permissions)

---

## 🎯 Final Thoughts

This technique is a brilliant workaround when you don’t want to dive into full-blown app registrations. Whether you’re building lightweight flows or experimenting with Microsoft Graph’s beta APIs, **HTTP with Entra ID** is a powerful ally.

![Final Confirmation](screenshots/screenshot_0-08-00.png)

Let me know what you build with it — and stay tuned for more Power Platform tricks.
