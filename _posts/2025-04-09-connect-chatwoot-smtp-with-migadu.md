---
title: How to set up Chatwoot with Migadu
layout: post
---

If you're self-hosting [Chatwoot](https://www.chatwoot.com/) and want to send emails using [Migadu](https://migadu.com/), here’s a quick guide. This helps you send out conversations, replies, and notifications from your own domain.

Update your `.env` file with the following SMTP settings. Replace all instances of `example.com` with your own domain:

```env
SMTP_DOMAIN=example.com
SMTP_ADDRESS=smtp.migadu.com
SMTP_PORT=465
SMTP_USERNAME=support@example.com
SMTP_PASSWORD=your-password-here
SMTP_AUTHENTICATION=login
SMTP_ENABLE_STARTTLS_AUTO=false
SMTP_OPENSSL_VERIFY_MODE=peer
SMTP_TLS=true
```

Note: These values are for secure sending via SSL (port 465). You can use STARTTLS (port 587) if you prefer, but adjust the config accordingly.

To apply the changes, restart your Chatwoot server.

Want to customize more? Check out all environment variable options at [chatwoot.com/docs/self-hosted/configuration/environment-variables](https://chatwoot.com/docs/self-hosted/configuration/environment-variables).
