# Claude Agent Notes

Use `../SKILL.md` as the source of truth for this skill.

This is an unofficial HostEurope/KIS administration skill. It is not affiliated with, endorsed by, or maintained by HostEurope.

Default behavior:

- Work read-only unless the user explicitly confirms a specific live action.
- Never ask for, store, or expose credentials, cookies, session tokens, 2FA/TAN values, recovery codes, API keys, private keys, or mailbox passwords.
- Keep HostEurope/KIS facts, user-provided requirements, and recommendations separate.
- Read reference files only when relevant:
  - `../references/browser-workflow.md`
  - `../references/inventory-schema.md`
  - `../references/change-checklists.md`
  - `../references/support-resources.md`

Suggested invocation:

`Use this skill to inventory HostEurope/KIS contracts, domains, DNS, mailboxes, SSL, terms, costs, and cancellation windows.`
