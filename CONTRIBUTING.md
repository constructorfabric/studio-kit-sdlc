# Contributing

## Developer Certificate of Origin (DCO)

All commits to this repository **MUST** be signed off under the
[Developer Certificate of Origin](https://developercertificate.org/). By signing
off, you certify that you wrote the change or otherwise have the right to submit
it under the project's license.

### How to sign off

Add a `Signed-off-by` trailer to every commit, matching your real name and email:

```
Signed-off-by: Your Name <you@example.com>
```

The easiest way is the `-s` / `--signoff` flag:

```bash
git commit -s -m "your message"
```

To add sign-off to existing local (unpushed) commits:

```bash
git rebase --signoff <base>
```

### Rules

- Every commit MUST include a `Signed-off-by` trailer.
- The sign-off name/email MUST match the commit author identity.
- Never push, amend already-pushed history, force-push, or use interactive
  rebase to satisfy DCO; sign off before pushing instead.
