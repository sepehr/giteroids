# Giteroids
Small yet handy collection of useful scripts, git & shell aliases to deal with Git repos on a daily basis. Repeatedly.


## Scripts
### Installation
For standalone scripts, do a `git clone` somewhere in your `$PATH` & make the scripts executable. That's a `chmod +x`. Simple. 

#### `git tags [-n N]`
It's important to version the release changelogs within a repository. Some choose to use a separate `CHANGELOG.md`, however, Git tag annotations are also a great tool for this purpose. 

The `git tags` script provides a readable colored trail of annotated git tags which serve as the chronological trail for the release tags & changelogs. If provided with `-n N` argument, it shows the last `n` release tags.

```
● git tags

... 

v6.5-rc4 at 5d0c230
Sun Jul 30 13:23:47 2023 -0700
Linus Torvalds <torvalds@linux-foundation.org>
Linux 6.5-rc4
Changelog:
- file: reinstate f_pos locking optimization for regular files
- arm64/fpsimd: Sync and zero pad FPSIMD state for streaming SVE
- arm64/fpsimd: Sync FPSIMD state with SVE for SME only systems
- arm64/ptrace: Do not enable SVE when setting streaming SVE
- rust: fix bindgen build error with UBSAN_BOUNDS_STRICT


v6.5-rc5 at 52a93d3
Sun Aug 6 15:07:51 2023 -0700
Linus Torvalds <torvalds@linux-foundation.org>
Linux 6.5-rc5
Changelog:
- fs: rely on ->iterate_shared to determine f_pos locking 
- vfs: get rid of old '->iterate' directory operation
- proc: fix missing conversion to 'iterate_shared'
- open: make RESOLVE_CACHED correctly test for O_TMPFILE
```


```
● git tags -n 1

v6.5-rc5 at 52a93d3
Sun Aug 6 15:07:51 2023 -0700
Linus Torvalds <torvalds@linux-foundation.org>
Linux 6.5-rc5
Changelog:
- fs: rely on ->iterate_shared to determine f_pos locking 
- vfs: get rid of old '->iterate' directory operation
- proc: fix missing conversion to 'iterate_shared'
- open: make RESOLVE_CACHED correctly test for O_TMPFILE
```


#### `git changelog [ref-hash-since]`
What were the changes since the latest annotated tag? `git changelog` has the answer:

```
● git changelog
Changelog:
- fs: rely on ->iterate_shared to determine f_pos locking 
- vfs: get rid of old '->iterate' directory operation
- proc: fix missing conversion to 'iterate_shared'
- open: make RESOLVE_CACHED correctly test for O_TMPFILE
```

#### `git release <version> [--gitlab]`
Time to release a new version? `git release` might be handy, especially if you also use Gitlab as the `origin` remote of the repo.

Given a tag, for example `4.2.0`, it create a new annotated tag using the `git changelog` output and optionally if provided with `--gitlab` argument, creates a corresponding Gitlab release on the remote. 

For the gitlab release functionality to work, a  `GITLAB_RELEASE_ACCESS_TOKEN` environment variable must be in context; which is a personal access token with the `api` scope. Export it somewhere in your shell configuration.

```
export GITLAB_RELEASE_ACCESS_TOKEN="my-gitlab-token-with-api-scope-access'

● git release 4.2.0 --gitlab

New tag was successfully created 🕺

Enumerating objects: 1, done.
Counting objects: 100% (1/1), done.
Writing objects: 100% (1/1), 909 bytes | 909.00 KiB/s, done.
Total 1 (delta 0), reused 0 (delta 0), pack-reused 0
To gitlab.mydomain.ltd:group-name/repo-name.git
 * [new tag]         4.2.0 -> 4.2.0

A new Gitlab release was created 💃🎉
https://gitlab.mydomain.ltd:group-name/repo-name/-/releases/4.2.0
```


#### `git fixup [ref-hash-onto]`
Does a transparent rebase by fixing up the all the uncommitted changes into the last commit, or optionally the given ref hash.

```
● git fixup
[main 517275f] fixup! Add readme.
 1 file changed, 11 insertions(+), 2 deletions(-)
Successfully rebased and updated refs/heads/main.
```

## Git Aliases
### Installation
To install git aliases, add a [`include.path`](https://git-scm.com/docs/git-config#_includes) config directive to your global git config file at `~/.gitconfig`:

```ini
[includes]
  path = /path/to/giteroids-repo/giteroids.gitconfig
```

Check out that file for a list of available aliases.

## Shell Aliases & Helpers
If command line is your main interface with git, you have to be lazy as me. The `giteroids/shell` file includes a set of git shorthands as aliases & helper functions. `gs` for `git status` kind, `git mr` for opening a Gitlab MR & such, you get the idea.

### Installation
Source it into your favorite shell configuration. For example:

```
# .zshrc
[ -f "path/to/giteroids/shell" ] && source "path/to/giteroids/shell"
```
