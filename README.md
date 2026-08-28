# The Fixture Static
A deliberately worthless website whose only job is to prove the machinery works before anything real is at stake.

---

A fixture is a controlled stand-in used to test a system. It is something deliberately trivial, put in place so the machinery around it can be exercised without anything real depending on the outcome.

A static fixture page is the simplest form. A minimal web page whose content is meaningless by design. It exists to prove the infrastructure around it, for example, deployment, routing, certificates, rollback, and monitoring. Because it carries no value, it can be broken deliberately, and because it's trivial, any failure points at the infrastructure rather than at the page.

## How to use this repository

Point your static-site deployment system at this repository and publish `index.html` from the repository root. The page has no build step, dependencies, or runtime configuration.

Three visually distinct commits provide known-good deployment targets:

| Version | Commit SHA | `index.html` SHA-256 | Noticeable difference |
| --- | --- | --- | --- |
| 00 | `a9e3ba0166b9c0d3e056278857536fcc2b966980` | `2cfb87cd69a8942617db38cfa9c6659f3bb1cb09e4b0cea3ddc8ed20ab21d776` | Green-to-purple background tint; label reads `Static fixture / 00` |
| 01 | `bda8765a4da64ac725b18e6bd906907d217e005a` | `f6f9d8b8861c950de501a99ea4ae5546ff000986575661c773d287676c5c5814` | Red-to-orange background tint; label reads `Static fixture / 01` |
| 02 | `6213517719ee7fbd8e7115ccb96e7540805445e4` | `f83c984181eda1a87c2051612a6c4ea9e1c08ca2695faad7a4ab763d61571519` | Yellow-to-blue background tint; label reads `Static fixture / 02` |

### Concrete checks

1. **Test a deploy:** deploy the current `main` branch. The page should show `Static fixture / 02` with a yellow-to-blue tint.
2. **Test a rollback:** ask your deployment system to redeploy commit `bda8765a4da64ac725b18e6bd906907d217e005a`. The label and background should visibly change to version 01.
3. **Test recovery:** redeploy commit `6213517719ee7fbd8e7115ccb96e7540805445e4` to return to version 02.

To inspect any version locally without moving the current branch, run:

```sh
git show <commit-sha>:index.html > /tmp/fixture-static.html
open /tmp/fixture-static.html
```

Use the visible version label to confirm routing, cache invalidation, and rollback behavior. Use your normal certificate and monitoring checks against the deployed URL.

To verify the exact deployed file, hash the response body and compare the result with the table:

```sh
curl -fsSL https://your-deployed-url.example/ | shasum -a 256
```
