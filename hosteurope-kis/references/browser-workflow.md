# Browser Workflow

Use when the user opens HostEurope/KIS in the in-app browser and logs in themselves.

## Login Boundary

- Let the user handle login, 2FA, consent, and account selection.
- Never ask for or store account credentials, cookies, session/local storage, 2FA/TAN values, recovery codes, API keys, or mailbox passwords. For an authorized certificate upload, use the file-handling steps in [the SSL workflow](ssl-certificates.md).
- If secret-like values appear, do not copy them. Ask the user to hide or rotate them when appropriate.

## Read-Only Work

- Read visible post-login pages after the user asks for inspection.
- Extract contracts, domains, DNS, mail, SSL, invoices, renewals, costs, and visible statuses.
- Use only read-only navigation, details, pagination, search/filter, and exports requested by the user.
- Use `support-resources.md` for public terminology, outage, support, and procedure checks.
- Ask the user to navigate when the next step may reveal secrets. For authorized state changes, follow the applicable workflow.

## Local Inventory

Create sanitized files only when useful. Use the current `hosteurope` workspace as root, or create `hosteurope/` under the current workspace.

```text
hosteurope/
  inventory/
    contracts.csv
    domains.csv
    mailboxes.csv
    dns-zones/
    ssl-certificates.csv
    cpanel-packages.csv
    contract-mapping.csv
    requirements-high-level.csv
  notes/
    inventory-summary.md
    migration-plan.md
    cancellation-checklist.md
    audit-log.md
```

Every row or section should include source, observed timestamp, confidence, current value/status, and open verification notes.

Suggested headers:

- `contracts.csv`: `contract_id,product_name,status,start_date,accounted_until,kis_changeable_until,cancellation_period,cancellation_deadline,billing_interval,cost_gross,currency,included_services,kis_product_assignment,source,observed_at,confidence,notes`
- `domains.csv`: `domain,tld,status,contract_id,renewal_date,cancellation_deadline,nameservers,dns_hosting,mail_dependency,web_dependency,kis_product_assignment,source,observed_at,confidence,notes`
- `mailboxes.csv`: `owner_type,domain,address_or_alias,type,status,quota,usage,forwarding_targets,internal_mailbox_target,mx_dependency,source,observed_at,confidence,notes`
- `ssl-certificates.csv`: `common_name,san_domains,issuer,type,expires_at,auto_renewal,status,bound_product_or_domain,source,observed_at,confidence,notes`
- `cpanel-packages.csv`: `owner_type,contract_id,package_type,package_name,visible_kis_domain,status,start_date,kis_changeable_until,cancellation_period,billing_interval,cost_gross,currency,source,observed_at,confidence,notes`
- DNS CSV: `domain,record_type,name,value,priority,ttl,purpose,source,observed_at,confidence,notes`
- `contract-mapping.csv`: `owner_group,owner_display,customer_context,object_type,object_id_or_name,domain,related_contract_ids,relationship,person_hint,source,observed_at,confidence,notes`
- `requirements-high-level.csv`: `owner_group,owner_display,domain_scope,requirement,priority,current_coverage,keep_or_target_state,open_questions,source,observed_at,confidence,notes`

Do not store raw HTML, screenshots, invoices, exports, full payment data, or unnecessary personal data unless the user explicitly confirms the exact artifact and scope.

## Extraction Pattern

1. State the visible page and fields to extract.
2. Extract visible tables/details into structured facts.
3. Resolve ambiguous KIS terms with `support-resources.md` when useful.
4. Ask for user navigation before sensitive or state-changing pages.
5. Update sanitized inventory files, if requested or useful.
6. Report new, changed, unknown, and conflicting fields.
7. Append `notes/audit-log.md` when local files change: timestamp, source, files, summary, open questions.

## KIS Notes

- `Vertrags- Übersicht`: contract id, product, billing interval, amount, `Änderbar Bis`.
- `Domainservices` -> `Bestehende Domains bearbeiten`: domains, `Abgerechnet bis`, product assignment. `Unbekannt` means unknown product assignment.
- `Domainservices` -> `Nameserver / DNS-Einträge bearbeiten`: AutoDNS and records. Prefer per-domain CSV export. If parsing the edit view, do not click save/update/delete/create and mark ambiguous values `needs verification`.
- AutoDNS uses old nested table/form markup. Do not assume one table row equals one DNS record.
- `Webhosting` details: server names, IPs, mail counts, SPF/DKIM snippets, costs, minimum term, cancellation period.
- `E-Mail-Konten verwalten`: mailbox/postbox accounts, quotas, usage, aliases, and settings. Refresh visible statistics before quota-sensitive extraction and record the timestamp.
- `E-Mail-Adressen verwalten`: address routing. Read only the selected mailbox option and full forwarding textbox/textarea. Preserve multiple forwarding targets.
- `-- Kein E-Mail-Konto --`: no internal HostEurope mailbox selected; forwarding may still exist.
- `cPanel WebHosting`: record visible `VERTRAGS-ID`. Treat `he-*.dummy` only as `visible_kis_domain`.
- `Email`: Microsoft 365 / Professional Email product lists and visible mailbox addresses.
- `SSL administrieren`: SSL bindings. Preserve conflicts such as `Website absichern` vs active SSL.

## Risky Pages

Stop before committing a cancellation, deletion, transfer, auth-code action, contract change, downgrade, order, paid add-on, renewal, DNS edit, mail deletion, password reset, SSL change, or nameserver change. A change form can be opened for preparation when doing so neither reveals secrets nor submits the form.

Risk words include `Speichern`, `Löschen`, `Kündigen`, `Transfer`, `Bestellen`, `Kostenpflichtig`, `Aktivieren`, `Deaktivieren`, `Zurücksetzen`, and `Absenden`.

## Before Any Live Action

Prepare:

- target
- before state and source
- intended after state
- expected impact
- unresolved unknowns
- rollback/fallback
- exact confirmation sentence

Do not click final submit/save/cancel/transfer/order controls until that exact confirmation is present.
