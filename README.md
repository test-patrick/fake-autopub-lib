# fake-autopub-lib

Smoke test repo for:

- AutoPub v1 pre-release via `uvx`
- uv build + publish
- `invite_contributors` plugin installed from git (`strawberry-graphql/autopub-plugins`)
- TestPyPI publishing (`publish-repository: testpypi`)

## Repo setup checklist

1. Ensure the `people` team exists in `test-patrick` org.
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

- Workflows call `autopub` directly with:
  `--with "strawberry-autopub-plugins @ git+https://github.com/strawberry-graphql/autopub-plugins.git"`
- PR checks and release check/prepare/build use the default `GITHUB_TOKEN`.
- Publish uses `BOT_TOKEN` because org invites require broader permissions.
