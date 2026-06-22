# Inventory Schema

Use for HostEurope/KIS contracts, domains, DNS, mail, SSL, invoices, and local notes. Keep facts, user requirements, and recommendations separate.

## Contract/Product

- contract number or product id
- product name and package type
- redacted customer/account context when needed
- status: active, pending cancellation, cancelled, expired, unknown
- start date, current term, accounted-through date, renewal date, minimum term
- KIS `Änderbar Bis` as `kis_changeable_until`
- cancellation period/deadline/channel when visible; do not derive deadline from `Änderbar Bis`
- cost, billing interval, currency, tax status when visible
- included services: webspace, databases, mailboxes, domains, SSL, backup, extras
- KIS product assignment
- source, timestamp, confidence, notes

## Domain

- domain and TLD
- contract/product association
- registration/status
- renewal date and transfer/cancellation facts when visible
- nameservers and DNS hosting
- mail and website dependency
- KIS product assignment (`VERTRAGS-ID`, product name, or `Unbekannt`)
- auth-code/transfer state if visible

## DNS

- zone/domain
- record type: A, AAAA, CNAME, MX, TXT, SPF, DKIM, DMARC, SRV, CAA, NS
- host/name, value/target, priority, TTL
- purpose, marked as inference when inferred
- source, timestamp, extraction quality
- mark AutoDNS edit-view ambiguities as `needs verification`

## Mail

- owner/customer group when known, using generic labels such as `owner_a`, `owner_b`, or `family_member_1`
- domain
- mailbox/postbox or E-Mail-Adresse, redacted if privacy-sensitive
- type: mailbox, address_with_internal_mailbox_target, address_with_forwarding, address_with_internal_mailbox_and_forwarding, catch-all, mailing list
- keep mailbox/postbox records separate from E-Mail-Adresse routing records
- quota and usage; note whether visible statistics were refreshed
- selected internal HostEurope mailbox target
- forwarding target(s), preserving multiple textbox/textarea lines
- protocols/settings if visible: IMAP, POP3, SMTP, webmail
- MX/SPF/DKIM/DMARC dependencies
- migration risk notes

## SSL

- certificate/common name and SANs
- issuer/type: Let's Encrypt, commercial, imported, unknown
- expiry date and auto-renewal
- bound product, vhost, or domain
- validation, chain, or binding issues

## cPanel

- owner/customer group when known
- KIS `VERTRAGS-ID`
- package type and package name
- visible KIS domain; keep `he-*.dummy` as `visible_kis_domain`, not as live customer domain
- start date, billing interval, amount, `Änderbar Bis`, cancellation period
- live domain binding status, or `unknown`
- source and timestamp

## Owner/Requirement

Use only for user-provided or reconciled administration intent:

- owner/customer group id and display name
- domain scope
- object type: contract, domain, mailbox, alias, DNS zone, SSL binding, requirement
- related contract ids
- person hint, if inferred from an address
- requirement priority: required, optional_or_unknown, not_required
- current coverage and target state
- open questions, source, confidence

## Review Signals

- renewal, SSL expiry, or `Änderbar Bis` within 90/60/30 days
- cancellation deadline before next review
- paid extras without visible use
- domains without visible website, mail, redirect, or DNS purpose
- DNS pointing to old hosts
- high mailbox usage or unclear owner
- missing SPF/DKIM/DMARC on active mail domains
- `Website absichern` while SSL administration shows active/global SSL
- domain product assignment `Unbekannt`
- target state conflicts, such as forwarding-only requirement with paid mailbox/Microsoft 365/cPanel products

## Confidence Labels

- `verified`: directly observed in authoritative source
- `likely`: consistent across sources but not directly contractual
- `inferred`: derived from DNS, names, or dependencies
- `unknown`: unavailable from current evidence
- `needs verification`: too risky to rely on without checking KIS or documents
