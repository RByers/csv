# csv

Seamlessly Display and Edit CSVs in Visual Studio Code. A fork of jonaraphael/csv.

@AGENTS.md

The project guidelines above come from `AGENTS.md`, including its commit and pull request
guidelines. The git workflow below says how pull requests get to GitHub.

## Testing on argus

argus has Node 22, where `npm test` fails: `node --test out/test` treats the directory as a file
(Node 20 expanded it). Run the tests this way instead, keeping the cap of 4 concurrent test files,
since this is a shared machine:

```sh
npm run compile && node --test --test-concurrency=4 'out/test/**/*.test.js'
```

## Git workflow: restricted, by pull request

This is the restricted workflow from argus-config's `git-workflows.md`, except that changes reach
Rick as pull requests from rbagent's fork, which he reviews and merges on GitHub. rbagent has
read-only access to RByers/csv, so a compromised agent can't change what Rick runs.

Remotes: `origin` is RByers/csv, fetch only (its push URL is disabled). `fork` is rbagent/csv,
where branches go.

- Work on a branch in a worktree under `.claude/worktrees/` (`EnterWorktree`), branched from
  `origin/main` after a `git fetch`. Don't plan from a stale `main`, and never commit on `main`
  itself: local `main` only follows `origin/main`.
- When work is complete (tests pass, changes committed), push the branch to `fork` and open a pull
  request: `gh pr create --repo RByers/csv --base main --head rbagent:<branch>`. Always pass
  `--repo RByers/csv`: RByers/csv is itself a fork, and gh could otherwise target the upstream
  jonaraphael/csv. Say in the PR what changed and why, so the diff is quick to review.
- Never open pull requests or issues on jonaraphael/csv. Sending changes upstream is Rick's call.
- Don't merge pull requests, your own or anyone else's. Once a PR is open, don't push to its
  branch unless Rick asks, so what he reviews is what he merges.
- After Rick merges, run `git pull --ff-only` in the main checkout, then remove the worktree and
  branch and delete the branch from `fork`. First confirm nothing would be lost: either
  `git merge-base --is-ancestor <branch> origin/main`, or, if he squash-merged,
  `gh pr view --repo RByers/csv <number>` shows it merged.
- Never force-push or rewrite history on `origin/main`.
