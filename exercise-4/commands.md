# Spectral Commands

## Simple Example Using Built-in Rules

```
spectral lint petstore.openapi.yaml

cat ruleset-builtin.yaml

spectral lint petstore.openapi.yaml --ruleset ruleset-builtin.yaml --verbose
```

## Tweaking Rules

```
cat ruleset-tweaking-rules.yaml

spectral lint petstore.openapi.yaml --ruleset ruleset-tweaking-rules.yaml --verbose
```

## Custom Rules

```
spectral lint petstore.openapi.yaml --ruleset ruleset-info-contact.yaml --verbose
```

## Custom Functions

```
spectral lint petstore-content-on-204.openapi.yaml --ruleset ruleset-no-content-on-204.yaml --verbose

spectral lint petstore-content-on-204.openapi.yaml --ruleset ruleset-no-content-on-204-function.yaml --verbose
```

## Git Hooks

```
vi petstore.openapi.yaml

git add petstore.openapi.yaml

git diff --cached

git commit -m "testing hook"

rm .git/hooks/pre-commit
```

## GitHub Actions

GitHub app: Create and publish new branch

```
vi petstore.openapi.yaml
```

GitHub app: Commit file and create pull request

GitHub web: Inspect progress on the PR's page
