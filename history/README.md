# history

The record of how this repository's history was extracted from the monorepo,
[The Test Cabinet](https://github.com/TheClockwyrks/TheTestCabinet), where the
crate here lived until the extraction. Nothing builds or tests against these
files; they are kept so the provenance outlives the scratch clone the
extraction ran in.

## The extraction

The history below the scaffold commit is the monorepo's `nightly` at
`35ac7aff76ff2bbe52aa901e5e0e3888b0d23a93`, taken from the branch
`issue/repos-7` forked from it, filtered to the paths in
[`paths.txt`](paths.txt) by git filter-repo 2.47.0:

```sh
git clone -q --no-local --single-branch --branch issue/repos-7 --no-tags \
  <the-test-cabinet> x
cd x
uvx --from git-filter-repo==2.47.0 git-filter-repo --force \
  --paths-from-file ../paths.txt --prune-empty always --preserve-commit-hashes
```

Of the 6,994 commits reachable from that commit, 15 touched a path in the list
and were kept (3 of them merges); the rest were pruned. No tag was carried. The
tip tree of the moved directory hashes the same as the monorepo's at that
commit:

```text
crates/audio-core/  97f53cc4caf65e2fd92ba847cd79bc8a627e13ce
```

The path is kept where it was in the monorepo, so the filter needed no rename.

## The files

- [`paths.txt`](paths.txt) is the list handed to `--paths-from-file`
  (sha256 `b1c8be37f24c380622dd54df29b26b5a51dd4cfe5abc2529b5a3fbe665378b36`).
  It is generated, not hand-written: the moved directory
  (`crates/audio-core/`), and every path a tracked file under it had before a
  rename or copy, collected by `git log --follow -M -C --name-status` over each
  of the 32 files. No rename or copy was found, so the list is that directory.
- [`commit-map`](commit-map) is filter-repo's map from each monorepo commit (left)
  to the commit it became here (right), all zeros for a pruned one.
  `--preserve-commit-hashes` left the commit ids quoted in the messages as they
  were, so an id in a message below the scaffold commit names a monorepo commit,
  and this map translates it.

## The live-path review

The list names no path outside the moved directory, so no live file of the
monorepo came along and the scaffold commit deletes nothing.
