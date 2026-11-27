# Change Checklists

Use only after the user asks to prepare a change. Never execute or submit without explicit confirmation for the exact target, action, and timing.

## Cancellation

- Identify exact contract, product, or domain.
- Verify renewal date, cancellation period/deadline, minimum term, and channel when visible.
- Check website, database, mailbox, DNS, SSL, backup, domain, redirect, invoice, and retention dependencies.
- Draft text with identifiers redacted unless the final text needs them.
- In KIS flows, verify each step, contract id, product, and cancellation date before continuing.
- Before final submission, verify the final button is enabled and the visible summary still matches the intended target.

Confirmation pattern:

`Ich bestätige: Kündigung für <Vertrag/Produkt/Domain> zum <Datum/nächstmöglichen Termin> vorbereiten/einreichen.`

## Domain Transfer

- Verify domain, TLD, lock status, contacts, auth-code process, DNS hosting, mail routing, and website hosting.
- Export/summarize current DNS.
- Confirm target registrar/provider and nameservers.
- Lower TTL only if appropriate and confirmed.
- Do not request or store auth codes.

## DNS Change

- Record current value and TTL.
- Identify affected services.
- Validate target values: A/AAAA, MX priority, TXT quoting, SPF includes, DKIM selector, DMARC policy, CAA.
- Propose change window and verification commands.
- Ask for exact confirmation before changing records.

Confirmation pattern:

`Ich bestätige: DNS für <domain> ändern: <record> von <old> auf <new>.`

## Mail Change/Migration

- Inventory mailboxes, aliases, forwarders, catch-all, quotas, and mail DNS.
- Identify users, devices, clients, migration window, and retention needs.
- Prepare backup/export steps without collecting passwords.
- Verify MX, SPF, DKIM, DMARC, autodiscover, and SMTP.
- Do not delete mailboxes or aliases until completion and retention are confirmed.

## SSL Change

- Identify domains/SANs, bound product/vhost, issuer, expiry, and auto-renewal.
- Check DNS or HTTP validation needs.
- Confirm renewal/change will not break vhosts or redirects.
- For local issuance, temporary DNS authorization, upload, renewal and live SNI verification, follow [the SSL workflow](ssl-certificates.md).

## Final Preflight

- Restate action, target, old state, new state, timing, impact, sources, unknowns, and rollback/fallback.
- Ask for one explicit confirmation sentence.
