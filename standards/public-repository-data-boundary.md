# Public Repository Data Boundary Standard

## Public repository may contain

- Sanitised reusable source code, documentation, templates, tests, and CI workflows.
- Non-sensitive examples using synthetic data and placeholder credentials.
- Dependency manifests and lockfiles after reviewing package/source metadata.

## Must remain local/private unless explicitly approved otherwise

- API keys, tokens, cookies, certificates, private keys, `.env`, and credential stores.
- Personal knowledge bases, private prompts, messages, uploads, and datasets.
- Databases, vector indexes, logs, chat histories, generated user content, and backups.
- Employer/client proprietary data and any sensitive or regulated data.

## Required checks before push

1. Review `git status --short` and staged diffs.
2. Run secret scanning on the working tree and relevant Git history.
3. Confirm `.gitignore` covers local state, but do not treat ignore rules as sufficient by themselves.
4. Review filenames, sample data, screenshots, docs, and commit history for sensitive content.
5. If a credential was committed, revoke/rotate it immediately; removing it in a later commit is not sufficient.
