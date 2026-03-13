# CI/CD, CC, & Friends

Slides and demo materials for a Platform Engineering talk on integrating Conventional Commits into CI/CD workflows. The presentation covers the Conventional Commits specification, how we use CC scopes to map to development threads in `pmbench`, and the tooling that turns structured commit messages into automated versioning, changelogs, and releases. The `demo/` directory contains a self-contained project wired with commitlint, husky, semantic-release, and CI configs for both GitHub Actions and GitLab CI.

## Viewing the slides

Live at **https://magsol.github.io/ci-cd-cc-friends/**

Or serve locally:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

Press `S` to open speaker notes.

## Running the demo

The `demo/` directory is a standalone Node.js project with CC tooling pre-configured.

### Setup

```bash
cd demo/
npm install
```

This installs commitlint, husky, and semantic-release. The `prepare` script automatically sets up husky git hooks.

### Try it out

```bash
# Bad commit message — rejected by commitlint
echo "// a change" >> src/greet.js
git add src/greet.js
git commit -m "updated stuff"

# Good commit message — passes
git commit -m "fix(io): handle edge case in greeting"

# Feature commit
echo "function farewell(name) { return 'Goodbye, ' + name; }" >> src/greet.js
git add src/greet.js
git commit -m "feat(api): add farewell function"
```

### CI/CD integration

**GitHub Actions** — Push to `main` and the release workflow (`.github/workflows/release.yml`) runs semantic-release automatically.

**GitLab CI** — An equivalent `.gitlab-ci.yml` example is included. Swap `@semantic-release/github` for `@semantic-release/gitlab` in `.releaserc.json` and set a `GITLAB_TOKEN` CI variable.

See `DEMO_SCRIPT.md` for a full step-by-step walkthrough.
