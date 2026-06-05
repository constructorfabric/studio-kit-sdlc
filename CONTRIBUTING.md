# Contributing

## Developer Certificate of Origin (DCO)

All commits to this repository **MUST** be signed off under the
[Developer Certificate of Origin](https://developercertificate.org/). By signing
off, you certify that you wrote the change or otherwise have the right to submit
it under the project's license.

### How to sign off

Add a `Signed-off-by` trailer to every commit, matching your real name and email:

```text
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

### Fixing a missing sign-off after pushing

If you already pushed commits without a `Signed-off-by`, how you fix it depends
on where those commits live:

- **On your own pull-request / feature branch** — re-sign and update the branch:

  ```bash
  git rebase --signoff <base>
  git push --force-with-lease
  ```

  Rewriting your own PR branch to add DCO is expected and allowed. Always use
  `--force-with-lease` (never plain `--force`) so you do not clobber others'
  work, and never rebase a branch someone else is actively building on.

- **On a shared or protected branch (e.g., `main`)** — do not rewrite the
  published history. Sign off going forward and ask a maintainer how to
  remediate the existing commits (for example a follow-up signed commit, a
  revert-and-recommit, or a maintainer-approved exception).

### Rules

- Every commit MUST include a `Signed-off-by` trailer.
- The sign-off name/email MUST match the commit author identity.
- Sign off your commits before pushing.
- Do not rewrite shared or protected history (no force-push or interactive
  rebase on `main` or other published branches). Rewriting your own
  pull-request branch to add a missing sign-off is allowed — see
  [Fixing a missing sign-off after pushing](#fixing-a-missing-sign-off-after-pushing).
