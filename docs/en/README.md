[العربية](../ar/README.md) | **English**

# SultanC package index

This repository is the index Supax reads to resolve dependencies such as `json = "1.2"`. It holds no package code: each version points to a git repository, a commit and a content checksum, and the code stays in its author's repository.

## Contents

| Path | What it holds |
|---|---|
| `config.toml` | The index format version (`version = 1`) |
| `packages/` | One file per package |

## Package file path

The path comes from the package name after identity folding (tatweel removed; آ أ إ folded to ا) and ASCII lower-casing:

| Name | Path |
|---|---|
| 1 character | `packages/1/<name>.toml` |
| 2 characters | `packages/2/<name>.toml` |
| 3 characters | `packages/3/<first character>/<name>.toml` |
| 4 or more | `packages/<characters 1–2>/<characters 3–4>/<name>.toml` |
| not ASCII (Arabic and others) | `packages/hash/<first 2 of the hash>/<sha256 of the name>.toml` |

There is no need to work the path out by hand: `supax package` prints it.

## Package file

```toml
name = "json"

[[versions]]
version = "1.0.0"
git = "https://github.com/someone/sn-json"
commit = "2f3a589624b5a839fe13a27ad52323a4e5edabf4"
checksum = "sha256:80d03f48f971bf104d3f3e46eefaba7c54bfa846be17d257847ec7659fe90680"
```

| Field | Meaning |
|---|---|
| `version` | A semantic version; must match `version` in `Supax.toml` at that commit |
| `git` | The public repository URL |
| `commit` | The full commit (40 hex characters) |
| `checksum` | Supax's checksum of that commit's content |
| `yanked` | Optional: `true` keeps the version out of new locks; locks that already have it keep working |

## Adding a version

1. In the package's repository: commit and push, then run `supax package`.
2. Supax prints the file path and the `[[versions]]` entry.
3. Open a pull request that appends the entry to that file, or creates the file with the `name` line first for a new package.

## Rules

- Each version is added once and never edited afterwards. The only allowed change is adding `yanked = true`.
- A version is removed only for malware or a legal reason.
- A new package is submitted by the owner of its repository.
- The index maintainer reviews and merges every pull request.
