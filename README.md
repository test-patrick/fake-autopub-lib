# fake-autopub-lib

Smoke test repo for:

- `autopub/autopub-action`
- AutoPub v1 pre-release (`autopub-version: pre-release`)
- uv build + publish
- local `invite_contributors` plugin
- TestPyPI publishing (`publish-repository: testpypi`)

## Repo setup checklist

1. Create `test-contributors` team in `test-patrick` org.
2. Add repo secret `BOT_TOKEN` with:
   - repo write access (for pushing release commit/tag)
   - org member invite permissions (for contributor invites)
3. Configure TestPyPI trusted publisher for this repository and package name:
   - package: `fake-autopub-lib-test-patrick`
   - workflow: `.github/workflows/release.yml`
   - environment: optional
4. Ensure workflow permission `id-token: write` remains enabled.

## How to run the e2e test

1. Open a PR that adds/updates `RELEASE.md` with:

```md
---
release type: patch
---
Test release for autopub + uv + invite plugin.
```

2. Merge PR into `main`.
3. Verify:
   - Release workflow publishes package to TestPyPI
   - Release commit/tag is pushed
   - Contributor gets org/team invite (if not already a member and not excluded)

## Notes

- The invite plugin is local (`invite_contributors.py`) so this repo can test immediately.
- `autopub-action` currently exposes `extra-plugins`, but the input is not wired in the action implementation yet.
