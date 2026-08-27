# Security

## Reporting a vulnerability

Please report security issues privately rather than opening a public issue.
Use GitHub's [private vulnerability reporting](https://github.com/Darkslayer3324j/fastone-website/security/advisories/new)
on this repository, or email nafayhassan3324j@gmail.com.

Please include what you did, what you expected, and what actually happened.
I'll acknowledge within a few days.

## Threat model

This repository holds a **static** customer ordering page and a **client-side**
admin panel. There is no server code here. All real authentication and all
authoritative order data live in a separate backend service, which the admin
panel reaches over HTTPS.

That split matters for reading the code:

- Anything the admin panel checks in JavaScript is advisory only. A browser
  runs code the user controls, so a client-side check can always be stepped
  past in devtools. It is a UI affordance, not a security boundary.
- The only real access control is the backend's `/login` endpoint and the
  token it issues. Treat every client-side branch as untrusted.

## Offline mode is deliberately unauthenticated

When the backend is unreachable, the admin panel falls back to an offline view
that reads only the orders already saved in that browser's `localStorage`. It
shows a banner saying it is not signed in, and it assigns a `local` role that
carries no privilege.

It does not ask for a password, and that is intentional. Earlier revisions
compared the entered password against a constant in the page source. Because
that constant shipped to every visitor, it authenticated nobody while creating
the impression that it did. Since the offline view can only ever read data
already in the current browser, there is no privilege for a password to protect
— so the honest design is to state plainly that no account was verified.

## Known issue: credential in git history

Revisions of this repository before the `harden-admin-auth` change contain a
hardcoded fallback credential in the admin HTML. The repository is public, so
that value must be treated as compromised and permanently disclosed — removing
it from the current files does not remove it from history, and rewriting
history would not help either, since anyone could already have read or forked
it.

The action that actually matters is rotation:

1. If those credentials were ever accepted by the **backend**, change them
   there now. That is the only part with real access to customer orders.
2. Do not reuse that password anywhere else.
3. Confirm the backend rejects the old value.

Removing the constant from the source, as this change does, prevents the
mistake from being repeated. It is not by itself a remediation of the exposure.

## Guidance for future changes

- Never commit a password, API key, or token. If a value must differ per
  deployment, read it from the backend at runtime.
- Do not add client-side role checks that gate anything meaningful. Enforce
  authorisation on the backend, for every request.
- Assume everything in this repository is readable by anyone, because it is.
