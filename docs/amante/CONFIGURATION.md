# Independent Amante services

There are no owner credentials or signing keys in this checkout. Source/help
and browser reporting use the owner-authorized `Amante-viewer-native` GitHub
fork. Notification-only links retain the existing `Amante-viewer` distribution
repository; that separate repository is not a GitHub fork.
OAuth and automatic media distribution remain pending. Public build settings
are optional; missing
configuration is explicit and never falls back to Aurora.

## Public reporting settings

The following public variables are read at compile time in
`aurora-version/src/amante_config.rs`:

| Variable | Required value / purpose |
|---|---|
| `AMANTE_ISSUES_URL` | Owner-controlled GitHub repository issues page |
| `AMANTE_ISSUES_API` | Matching `https://api.github.com/repos/OWNER/REPOSITORY/issues` |
| `AMANTE_GITHUB_CLIENT_ID` | Public identifier of Amante's own GitHub App with Device Flow enabled |

Repository URLs must describe the same destination. Aurora's upstream
repository is explicitly rejected. Browser reporting needs the two repository
settings; direct sending additionally needs the OAuth client ID.
The issue URL/API default to the enabled issue tracker in
`tarahamante-cloud/Amante-viewer-native`, as authorized by the owner.
Unset or empty issue overrides use this approved default; they never use Aurora.
The OAuth client ID still defaults to empty, so connecting/sending directly
stays unavailable. Explicit invalid issue overrides disable remote reporting.

OAuth tokens remain in memory or Amante's separate OS credential service,
according to the retained reporting implementation. Never put a client secret,
access token, refresh token or signing key in source, public variables or logs.
GitHub's device authorization endpoints remain GitHub endpoints; they are not
an Aurora-owned service. No real authorization was performed in automated tests.

## Update and plugin distribution settings

- `AMANTE_RELEASES_API`
- `AMANTE_RELEASE_PAGE`
- `AMANTE_RELEASE_DOWNLOADS`

All default to empty. **They do not activate production updates.**
`PRODUCTION_UPDATES_READY` remains false because authenticated release metadata,
the Amante public trust root, key rotation, anti-rollback policy and owner
release authorization still need implementation and review.

Do not flip this guard just to make update checks work. Checksums delivered
beside an archive do not establish publisher authenticity. CI packages are
downloadable development builds, not automatically trusted update releases.

The existing loopback-only updater fixtures remain available without a real
grid, repository token, production key or published release. Automatic native
media-plugin distribution also needs an Amante-controlled package source;
manual plugin-folder selection and all plugin implementations are retained.

`AMANTE_MEDIA_RELEASE_DOWNLOADS` independently configures the media package
download prefix: `https://github.com/OWNER/REPOSITORY/releases/download`.
It must name an owner-approved repository and must not name Aurora's upstream
repository. Missing/invalid configuration shows a pending/unavailable message
and does not start automatic downloads. It does not enable executable updates.
Loopback-only media test overrides remain separate.

Publish the **exact** platform-specific archives expected by
`aurora-updater/src/media_pack.rs`, under `media-plugins-<version>`, with the
existing archive and manifest names. The pinned byte size and SHA-256 must
match. Do not set the URL to upstream infrastructure merely to satisfy the UI.
If the owner rebuilds a pack, review and pin its new size/hash before shipping.

These variables are compiled into the native binary, not read when it starts.
Supply public values through the build environment and rebuild. Keep all
tokens/signing keys out of these variables. The fork and its issue destinations
are approved; no OAuth client ID or media package source has been supplied.
The workspace's `.github/workflows/amante-build.yml` passes repository
**Variables** with these names to both native jobs; unset values stay empty
and use the approved issue defaults where applicable.
Do not use an OAuth token or client secret as a public client ID.

## Data isolation

- Project directories: qualifier `org`, organization `Amante`, app `AmanteViewer`.
- OS credential services: `Amante Viewer`, `Amante Viewer MFA`,
  `Amante Viewer GitHub`.
- Windows notification identity: `Amante.Viewer`.
- SLURL handoff named pipe: `amante-viewer-slurl-…`.
- Distributed executable: `amante-viewer.exe` / `amante-viewer`.

Legacy protocol/environment names such as `AURORA_DEMO` and the internal
installation-manifest schema remain for source compatibility. They do not
select Aurora's settings directory or authorize an upstream update.

## Retained upstream references

Upstream authorship, licenses, technical compatibility names and original
documentation/news remain in the source. These must not be mistaken for
Amante service ownership. See [the ownership/content audit](SERVICE_OWNERSHIP.md)
for classified service roles, intentional Aurora names and pending decisions.

`service-references.json` records current code/locale/script/workflow external
URL literals, with exact source locations. Regenerate it
with `node scripts/capture-amante-service-references.mjs`. It deliberately does
not classify a reference as an active endpoint merely because it appears in a
comment, credit or offline fixture. Dynamic URLs and scripts still need review.
