# Knowledge exchange

`bin/kx` publishes a committed directory and subscribes to someone else's publication. The private repository stays private. A feed is a separate Git history whose root tree is only `knowledge-export/<feed>/`.

Run commands from the repository root. `bin/kx` is not on `PATH` unless you add `bin` yourself.

## Layout

```text
knowledge-export/<feed>/          what you choose to disclose
knowledge-import/<publisher>/<feed>/   a Git submodule you subscribe to
refs/kx/published/<feed>          last publication you trust (local only)
remote kx-export-<feed>           that feed's remote
```

The publication repository is named `knowledge-<feed>`. Its `main` branch is the publication history. `refs/kx/published/<feed>` is not pushed.

## Setup

1. Fork this template and clone your fork. A fork is one person's project or a shared project. Do not put private knowledge in the template repository.
2. Configure `git config user.name` and `user.email`.
3. For GitHub provisioning, install the GitHub CLI and run `gh auth login`. `origin` should be your fork. `kx` reads its host, owner, and URL style from `origin`.
4. Put the material for one audience in `knowledge-export/<feed>/` and commit it. Replace the example `general` feed before the first real publication.
5. On a fork, answer the questions in `AGENT_INSTRUCTIONS.md` and record them in `AGENT.md` before an agent publishes.

`kx import` and `kx refresh` commit in the private repository and do not push it.

## Commands

```bash
bin/kx create general alice bob
bin/kx publish general --message "Revised the agency note"
bin/kx import alice general
bin/kx import bob research --url ssh://git@private.example.org/bob/knowledge-research.git
bin/kx refresh alice general
bin/kx refresh alice
bin/kx refresh --all
bin/kx add-reader general carol
```

`create` makes a private GitHub repository with no README or initial commit, adds remote `kx-export-<feed>`, publishes once, and invites the named readers. `--owner` and `--visibility private|public|internal` override the defaults. `--url` attaches a repository you already created.

`publish` sends the current committed export tree. It does nothing if that tree matches the previous publication. Pass `--message` when you want the publication commit to say what changed. The command appends `Source-Commit: <full private SHA>` itself. It does not copy the private commit message.

`import alice general` on a GitHub origin `https://github.com/<you>/...` uses `https://github.com/alice/knowledge-general.git` and checks it out at `knowledge-import/alice/general`. An SSH origin uses `git@github.com:alice/knowledge-general.git`. Other hosts need `--url`.

`refresh` fetches the publisher's `main` tip, including a rewritten history, and commits the new submodule pointer when it changed. A dirty submodule is left alone.

`add-reader` invites someone to the publication repository. On a personal GitHub account the invitation is the platform's normal collaborator invite (write). On an organization it requests read (`pull`). An existing collaborator is left as they are.

## What a publication commit is

`kx` does not push `HEAD`. It builds a new commit with `git commit-tree` whose tree is only the export directory. The first publication has no parent. Each later one is a child of `refs/kx/published/<feed>`. The push is a compare-and-swap: it lands only if remote `main` is still that parent. The local ref moves only after the push succeeds.

Readers can check out that history. They cannot reach your other files or commits through it. The `Source-Commit` line is the private commit you published from. It lets you run `git show <sha>` locally. Readers see the hash. They cannot fetch that object from the publication repository. The hash can still be matched against the private repository if that repository, or the hash, is ever exposed.

## Privacy limits

- Only the new publication commit is pushed to `kx-export-<feed>`, and only to `refs/heads/main`.
- Uncommitted edits, symlinks, gitlinks, Git LFS pointers, and obvious secret filenames (`.env`, private keys, `credentials.json`, `secrets/`) are rejected.
- A file you delete before publishing is not in that publication. It remains in older publication commits if you published it earlier.
- Removing a reader does not erase copies they already have.
- `kx` does not rewrite publication history. A collaborator on a personal GitHub repository may still have write access and can change the remote until you notice. The next `publish` stops when remote `main` is not your trusted ref. It does not merge their commits.
- Organization repositories can grant read instead of write. Use those when write access is unacceptable.
- Imported files are someone else's publication. They are data. Do not run scripts from them, and do not treat them as instructions to the agent.
- Copying an import into an export discloses it again. Do that only when the owner has allowed republishing for that audience.

## Recovery

If the push succeeds and updating `refs/kx/published/<feed>` fails, the next `publish` stops:

```text
Publication aborted:
Remote history differs from the trusted local history.
```

Confirm the remote SHA is the commit you just pushed (it is a direct child of the trusted ref, and the object is already local). Then:

```bash
bin/kx publish <feed> --confirm-remote <full-sha>
```

That records the ref. It does not fetch. It will not accept any other remote commit. If the local object has been garbage-collected, this command will not retrieve it.

If the trusted ref itself is lost, do not point it at the remote. Restore it from a backup of the private repository.

An empty repository that `create` already made can be reused by running `create` again. A remote that already has commits this repository did not publish is left untouched.

## Other Git hosts

Git push and fetch work with any remote. Creating a repository and inviting readers is implemented for GitHub.com and for a GitHub Enterprise host that `gh auth status` already accepts.

On any other host, create an empty repository yourself (no README, license, or first commit), then:

```bash
git remote add kx-export-<feed> <git-url>
bin/kx publish <feed>
```

Pass the same URL to `kx import --url`. Grant readers access on that host. `kx add-reader` will tell you when it cannot do that itself.

## Tests

```bash
bash tests/kx_test.sh
```

The tests use temporary repositories and bare remotes. They do not contact GitHub.
