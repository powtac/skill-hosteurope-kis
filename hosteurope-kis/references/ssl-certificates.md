# Own SSL/TLS certificates

## Identify the target

- Confirm the requested DNS names and the product that terminates HTTPS. For the apex and first-level subdomains, request both `example.org` and `*.example.org` as SANs. Deeper names such as `a.b.example.org` need explicit coverage. Each HTTPS binding must serve a certificate covering its hostnames. An ACME contact email is for registration. Mail signing or message encryption requires S/MIME. Managed IMAP/SMTP uses the mail service's TLS configuration.
- Resolve public NS, A, AAAA and relevant CAA records. In Classic Webhosting, compare public addresses with the KIS standard and SSL addresses for that package. In `SSL administrieren`, map the requested hosts to the directory and inspect existing assignments. `Ja (global)` marks the package fallback. Check whether subdomains have separate bindings before replacing certificates.
- Read the live certificate with SNI for each relevant host. An expired certificate or one whose SANs only match another subdomain can be served even when KIS shows an active assignment. Record names, issuer, expiry and SHA-256 fingerprint as needed. Keep the current assignment available as a fallback if the provider offers it. Changes to A/AAAA records or other bindings require authorization if they were outside the approved scope.

## Issue locally with Certbot

1. Check for an existing ACME client. Reuse the user's designated ACME configuration when applicable. If the client is absent, use an isolated install from a reputable package source. Obtain any outstanding consent for account registration under the [current subscriber agreement](https://letsencrypt.org/repository/), adding and removing the generated ACME TXT values, and installing the certificate and private key on the named hosting binding. The user signs in to KIS in the visible in-app browser. Reuse the signed-in tab after handoff.
2. For a wildcard, use DNS-01. This example uses a private temporary directory outside the repository. Replace the sample names and contact. If registering a new account, proceed through the terms prompt only with the user's authorization.

   ```sh
   umask 077
   acme_dir=$(mktemp -d "${TMPDIR:-/tmp}/hosteurope-acme.XXXXXXXX") || exit 1
   certbot certonly --manual --preferred-challenges dns \
     --email 'admin@example.org' --cert-name 'example.org' \
     --config-dir "$acme_dir/config" --work-dir "$acme_dir/work" --logs-dir "$acme_dir/logs" \
     -d 'example.org' -d '*.example.org'
   ```

3. Place the client-generated TXT values at the authoritative validation name, normally `_acme-challenge.example.org`. Check for existing TXT records or a CNAME/NS delegation at that name. In KIS use `Domainservices → Domain-Administration → Nameserver / DNS-Einträge bearbeiten` (also reachable via `DNS bearbeiten` for the domain) if the listed nameservers match the public delegation. Domains using `nsXX.domaincontrol.com` can still be managed by this KIS form. If the form appends the domain, enter the relative host `_acme-challenge`. If the authoritative zone is elsewhere, use its DNS manager.
4. Keep the CLI session open. For each prompt add the new value alongside any existing values. When apex and wildcard challenges share a name, their values may need to coexist as separate TXT records until issuance finishes. Query all authoritative nameservers serving the validation record using `dig @<nameserver> _acme-challenge.example.org TXT +norecurse` before advancing the client. Allow for negative caching or propagation if the name was just created. After issuance, remove only the exact TXT values created for this order and verify cleanup. If issuance fails, investigate DNS or CAA with the existing session where possible. Avoid blind production retries.

## Upload and verify

- **Classic Webhosting / WebServer.** In the target product open `Konfigurieren → Sicherheit & SSL → SSL administrieren`. Choose `Zuweisen` or `Ersetzen` on the row for the confirmed directory/host group. Under `Übertragen Sie Ihre Zertifikat-Datei`, upload `cert.pem` as the leaf certificate, `privkey.pem` as its private key and `chain.pem` as the CA chain. Certbot places them under `config/live/<cert-name>/`. Confirm the chosen assignment before clicking `Absenden`. Separate directories can have separate certificate bindings.
- **cPanel WebHosting.** Open the hosting product's cPanel, then `SSL/TLS → SSL-Websites verwalten → SSL-Website installieren`. Select the confirmed domain. Map `cert.pem`, `privkey.pem`, `chain.pem` to CRT, KEY and CABUNDLE. Check the plan's AutoSSL behavior when the user asks about managed renewal.
- **Key handling.** Use the browser's file chooser with the local key file when one is available. When the interface requires pasting the PEM into a text field, hand off entry of the private key to the user in the official portal. Keep key material out of model-visible tool output and shared storage.
- **Verification.** Check the KIS or cPanel confirmation, then verify publicly for the apex, `www` and any other in-scope host. For example, use `openssl s_client -connect example.org:443 -servername example.org </dev/null 2>/dev/null | openssl x509 -noout -issuer -dates -ext subjectAltName -fingerprint -sha256` to inspect the served leaf and `curl -sSI --max-time 10 https://example.org/` for TLS validation. Compare the live fingerprint with the new certificate. If IPv4 and IPv6 are published, test both where available. KIS may show a new assignment before the endpoint serves it. Report that distinction and verify deployment when it takes effect.
- **Renewal.** State the verified expiry date and whether renewal and reinstallation are automated. Manual DNS-01 requires repeating validation and re-uploading the renewed certificate unless both steps are automated. Retain local ACME/key files only in a user-approved private location when needed. Clean up temporary key files after successful installation and verification when the user has no need to retain them.

## Official references

- [Host Europe Classic Webhosting upload](https://www.hosteurope.de/faq/software-services/ssl-zertifikate/ssl-zertifikat-einbinden-webhosting/)
- [Host Europe cPanel upload](https://www.hosteurope.de/faq/cpanel-hosting/cpanel-ssl-cert/cpanel-ssl-manuell/)
- [Host Europe AutoDNS](https://www.hosteurope.de/faq/domains/verwaltung/nameserver-eintraege/)
- [Let's Encrypt DNS-01](https://letsencrypt.org/docs/challenge-types/)
- [Certbot manual authenticator and certificate files](https://eff-certbot.readthedocs.io/en/stable/using.html)
