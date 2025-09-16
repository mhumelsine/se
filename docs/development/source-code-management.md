# Source Code Management

Assume we are usig Git for source control management (SCM).  All content below assumes Git.

## Bootstrap a New Project



## Helpful Definitions
- Git is simple.  A branch = a file that contains a commit hash.
- A commit hash is a position in the revision history.
- To test this out, you can use `cat .git/refs/heads/{branchName}` in your current project to print the commit hash.
- To find the commit details, you can use `git cat-file commit {commit hash}` and `git log -3 {commit hash}` to show the last 3 previous commits.  Example output:

```bash
$ ~/cptis> cat ./.git/refs/remotes/origin/dev
ff9e4ff5eddf5bd24bc537045e3bde2cc12a9485

$ ~/cptis> git cat-file commit ff9e4ff5eddf5bd24bc537045e3bde2cc12a9485

tree d99f873cf8e8b418136528335a663b405a158b2d
parent 15b0cafd4311d4d86feff11f47047e3a70bb72f8
author Goyal, Alka <Alka.Goyal@flhealth.gov> 1757450705 +0000
committer Goyal, Alka <Alka.Goyal@flhealth.gov> 1757450705 +0000

Merged PR 586: cleanup

cleanup

Related work items: #15754

$ ~/cptis> git log -3 ff9e4ff5eddf5bd24bc537045e3bde2cc12a9485

commit ff9e4ff5eddf5bd24bc537045e3bde2cc12a9485 (origin/dev, origin/HEAD, dev)
Author: Goyal, Alka <Alka.Goyal@flhealth.gov>
Date:   Tue Sep 9 20:45:05 2025 +0000

    Merged PR 586: cleanup

    cleanup

    Related work items: #15754

commit 15b0cafd4311d4d86feff11f47047e3a70bb72f8
Author: Goyal, Alka <Alka.Goyal@flhealth.gov>
Date:   Tue Sep 9 20:38:07 2025 +0000

    Merged PR 585: Interfaces

    Interfaces

    Related work items: #15742

commit f9e79e481e78146783d05bd91237158e47e3fb53
Author: Marron, Najash <Najash.Marron@flhealth.gov>
Date:   Tue Sep 9 16:17:10 2025 +0000

    Merged PR 587: 15653 - Neonatal Abstinance Syndrome (NAS) FE

    Related work items: #15653
```


- Use trunk based strategy - use short-lived task branches that are merged into a main trunk frequently.  
- Only allow squash merges on `main` and `production`.

## Permanent Branches
- `main` - main trunk branch where continuous integration takes place.
- `production` - only commits are production releases.

## Temporary Branches
- `release/{Major}.{minor}.{patch}` ex: `release/12.0.1`.  Used to freeze code prior to release.  Ends when version is released to production.
- `task/{work item #}` ex: `task/14551`.  Used to while developr is working on new task or bugfix.  Ends with successful pull request is accepted.

## Example Workflow
1. Developer starts task 14813
2. `$> git fetch && git checkout -b task/14813 origin/main`
    1. `git fetch` pull all remote content
    1. `&&` (except Windows) execute the next command only in the first succeeds
    1. `git checkout -b task/14813 origin/main` switch to a new branch named task/14813 based on `origin/main`.
3. Development work is completed.
4. `git commit -m "Commit message #14813"` creates a commit and links the assoicated work item.
5. `git fetch && git rebase origin/main` pull the latest from remote and changes the starting point of branch `task/14813` from the original start to the latest commit on `origin/main`.
6.  `git push -u origin task/14813` pushes changes to remote and sets the current branch to track an upstream (`-u`) branch with the same name.

## Release Workflow
1. Release discission is made.
1. `git fetch && git checkout -b release/{major}.{minor}.{patch} origin/main && git push -u origin release/{major}.{minor}.{patch}`.  Fetch latest `main`, create a release branch with the desired version, push that to remote.  Ex: `git fetch && git checkout -b release/14.0.1 && git push -u origin release/14.0.1`.
1. Version released.
1. Tag the version and merge to production.
    - `git fetch` (get latest)
    - `git checkout production` (get on production)
    - `git reset --hard origin/production` (force your local to match remote)
    - `git merge -s theirs origin/release/14.0.1`
    - `git tag -a v14.0.1 -m "Release version 14.0.1"` (tag the release merge commit)
    - `git push v14.0.1` (push commit)
    - `git push` (push production branch)

### Bugs Discovered in Release Version
1. Bugs discovered after release is frozen is common.  If there are more than 1-2, consider figuring out why.  Biggest thing is make sure the bug fix is applied to both the release branch and the main trunk branch.
1. We have found using `git cherry-pick` is the most painless way of doing this.
1. Pick the branch to apply the change to - either `main` or `release/*`, let's assume main.
1. `git checkout main && git pull && git checkout -b task/1234`.
1. Make the changes to fix the bug, push, and submit a pull request.
1. What you want to do is cherry-pick the commit of your squash merge onto the trunk.  You can use `git log -n` to list the last couple commits.
1. `git checkout release/14.0.1 && git pull` Get up to date with remote changes.
1. `git checkout -b task/1234-cp-on-14.0.1`
1. `git cherry-pick {commit#}` (resolve any conflicts)
1. `git push -u task/1234-cp-on-14.0.1`
1. Complete pull request into `release/14.0.1`.

**You can do this inside Azure DevOps by finding the button [Cherry Pick] at the top of a completed pull request**.  As long as there are no conflicts, this is by far the easiest way to go.  If there are conflicts, you will have to complete the process locally as described above.