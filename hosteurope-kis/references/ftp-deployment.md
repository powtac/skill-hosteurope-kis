# FTP deployment

## Verify the destination in KIS

- Have the user sign in to KIS. If the Webhosting overview is empty, open **Produktverwaltung → Vertragsübersicht** and use **Produkt verwalten** on the relevant active hosting contract.
- Under **Allgemeines → Allgemeine Informationen**, confirm the domain belongs to the package and read **Servername**. The FTP server hostname can differ from the website hostname.
- Under **Domains → Domainzuordnung & -verwaltung**, verify the specific domain's assignment. For **Verzeichnis**, use the listed directory as the document root. For **Umleitung**, resolve the destination before uploading. Treat an umlaut domain and its ASCII spelling as distinct DNS names.
- Under **Webspace & Nutzer → FTP-Zugänge**, check the account's start directory and active status. KIS does not reveal existing passwords. Leave matching accounts visible for the user and avoid recording secrets or account identifiers in the skill.
- Match the URL path to the document root, then translate it relative to the account's start directory. Example: if both the root and the FTP start directory are `/www`, use `projekt/` from the FTP session root. With an account starting at `/`, use `www/projekt/`.
- The Classic Hosting **Dateiverwaltung** can verify directory contents. It is a deletion interface, so use it only for read-only preflight.

## Transfer and verify

- Use an existing authenticated connection. For Classic Hosting FTP use explicit TLS (FTPES, port 21) with the KIS server hostname. SFTP uses a separately configured SSH account. Prefer a verified existing SFTP alias or the HostEurope WebCenter. The user signs into WebCenter with their FTP account. Follow the main skill's authorization rules for account or password changes and external uploads.
- For a static site, stage only deployable files, keeping relative asset paths and license files. A WebCenter ZIP can be extracted server-side. Verify the ZIP contents, intended extraction directory, and whether the target already contains files to avoid nesting the directory twice or overwriting unrelated content.
- Verify that the expected index file and assets exist after transfer and check the public URL. A public HTTPS certificate failure is distinct from FTP authentication or upload success. Report the verification limit and do not bypass TLS or change certificate settings as part of a file upload.

## Official references

- [Host Europe instructions for uploading files](https://www.hosteurope.de/faq/webhosting/hochladen-von-webinhalten-ftp/wie-lade-ich-meine-daten-auf-den-web-speicherplatz-hoch)
- [Host Europe FTPES and SFTP settings](https://www.hosteurope.de/faq/webhosting/hochladen-von-webinhalten-ftp/ftp-verschluesselt-uebertragen)
- [Host Europe WebCenter and ZIP upload](https://www.hosteurope.de/faq/webhosting/hochladen-von-webinhalten-ftp/webcenter/)
