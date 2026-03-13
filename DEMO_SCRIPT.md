# Live Demo Script

## Pre-demo setup
```bash
cd demo/
npm install
```
Make sure husky hooks are installed (the `prepare` script runs automatically).

## Demo Steps

### 1. Show the project structure (~30 sec)
```bash
ls -la
cat commitlint.config.js
cat .releaserc.json
cat .husky/commit-msg
```
**Talk through:** commitlint config enforces CC format, husky runs it on every commit, semantic-release reads CC to determine versions.

### 2. Try a BAD commit message (~1 min)
```bash
echo "// a change" >> src/greet.js
git add src/greet.js
git commit -m "updated stuff"
```
**Expected:** commitlint rejects it! Show the error output — it tells you exactly what's wrong.

### 3. Try another bad one (~30 sec)
```bash
git commit -m "foo: this type doesn't exist"
```
**Expected:** Rejected again — `foo` is not a valid type.

### 4. Make a proper fix commit (~1 min)
```bash
git commit -m "fix(io): handle edge case in greeting"
```
**Expected:** Passes! Explain: `fix` = patch version bump (0.0.0 -> 0.0.1).

### 5. Make a feat commit (~1 min)
```bash
echo -e "\nfunction farewell(name) {\n  return \`Goodbye, \${name}!\`;\n}\n\nmodule.exports = { greet, farewell };" >> src/greet.js
git add src/greet.js
git commit -m "feat(api): add farewell function"
```
**Expected:** Passes! Explain: `feat` = minor version bump (0.0.1 -> 0.1.0).

### 6. Push and show CI (~2 min)
```bash
git push origin main
```
**Talk through:** Open GitHub Actions tab, show the release workflow running. While it runs, explain what semantic-release is doing:
1. Analyzing commits since last tag
2. Determining version bump (minor, because of the `feat`)
3. Generating release notes from CC messages
4. Creating a git tag + GitHub Release
5. Updating CHANGELOG.md

### 7. Show the results (~1 min)
- Open the GitHub Releases page — show the auto-created release
- Show the auto-generated CHANGELOG.md
- Show the git tag

## Troubleshooting
- If husky hooks don't fire: run `npx husky` manually
- If semantic-release doesn't create a release: make sure GITHUB_TOKEN has write permissions (check repo Settings > Actions > General > Workflow permissions)
- For the demo, make sure the repo is public OR that you have a PAT with appropriate scopes

## GitLab CI callout
After the demo, flip back to slides and mention:
"For our internal GitLab, the config is nearly identical — swap the CI file and the token variable. See `gitlab-ci-example.yml` in the repo."
