# Git Branching & Merging

## Why Branch and Merge?
Branching allows developers to quickly create new instances or copies of a current set of files either locally or remotely on a GitHub repository; this helps separate working/production-ready code from code currently under development/testing.

Merging is very important in the software development lifecycle, it helps implement new features or changes to code in a non-obstructive way; it is the foundation of expanding and building upon previously developed code.

## Terminology & Commands
Listed below are a few key terminologies and commands most commonly used during development:

#### remote
Oftentimes, remote is set to `origin` which is essentially the original repository on GitHub that the project was initially cloned from onto a local machine; in some cases, a repository may have different “clones” of itself on GitHub in which case different remotes can be specified and pushed to instead of `origin`.

`git remote add [remote-name] [remote-url]` — adds a new remote to the repo from a specified url; this command is often used during the initialization of a new git project

#### checkout/branching
Branching creates a snapshot of files from another branch with modifications only pertaining to its respective branch. Checking out a branch switches between different branches.

`git checkout [branch-name]` — switches to specified branch from current branch  
`git checkout -b [branch-name]` — creates a new branch with files identical to the current branch this command is run in

#### merging
Combines changes in files and code from two separate branches into one branch.

`git merge [branch-name]` — merges code from specified branch into current branch

#### fetching
Retrieves a list of changes (ex. new commits, new branches, etc.) made to a remote repository on GitHub, but *does not* overwrite any local code in the current branch (unlike a `git pull`).

`git fetch` — synchronizes changes from remote repository on GitHub with local repository

#### pulling
At its core, a pull in git is a combination of `git fetch` and `git merge` where changes to the remote branch are fetched first and then merged into the current local branch.
  
`git pull` — adds in any changes made to the same remote branch on GitHub into current local branch  
`git pull [remote-name] [branch-name]` — adds in changes from another remote branch on GitHub into current local branch (remember, *remote-name* is often `origin`)

#### commit
A single commit is a recorded set of changes to one or more files at a period of time; a branch consists of a history of multiple commits.

`git commit -am “[commit-message]”` — adds all modified/deleted files and creates a commit with the specified message

#### HEAD
For brevity, the HEAD points to the latest commit in a branch otherwise known as the tip of a branch. 

- - - -

## Types of Merges
Merging two branches is essentially combining their respective commits into one single commit history. There are two types of merges that can occur when combining two different branches, they are fast forward merge and three-way merge:

### Fast Forward Merge
Let’s say `branch-2` is created by branching off `branch-1`. Changes are made to `branch-2` and pushed back up to GitHub. No changes are made or pushed to `branch-1`. Merging `branch-2` back into `branch-1` will require a fast forward merge since the commits made to `branch-2` can simply be stacked onto `branch-1` which contains an identical commit history to `branch-2` up until the point it was branched off of. Thus, we are “fast forwarding” the HEAD of `branch-1` to the HEAD of `branch-2`. For a more visual explanation see [here](https://www.atlassian.com/git/tutorials/using-branches/git-merge).

### Three-Way Merge
In the case where changes were made to `branch-1`, a three-way merge would occur. This is because `branch-1` and `branch-2` both contain identical commit histories up until their branch point plus additional commits to their respective branches. Since `branch-1` contains commits *not* in `branch-2` and vice versa, git can not simply stack their commits on top each other. In a three-way merge an extra commit is used to combine the two branches. For a more visual explanation see [here](https://www.atlassian.com/git/tutorials/using-branches/git-merge).

## Merge Conflicts
Merge conflicts arise when two branches have changed the same part of a file in which case git does not know which version to accept. For example, both `branch-1` and `branch-2` changed line 9 of `index.html`; from git’s perspective, there’s no clear distinction of whether to accept line 9 of `index.html` from `branch-1` or `branch-2` subsequently causing a merge conflict requiring manual resolution.

#### Merge Conflict Syntax:
`<<<<<<<` — indicates the beginning of conflicting code that needs to be resolved  
`=======` — divides incoming code changes (below line) from current code changes (above line)  
`>>>>>>>` — indicates the end of conflicting code that needs to be resolved  

After a merge conflict has been manually resolved, changes must be added and committed for full resolution of the conflict on the branch. Be sure to remove `<<<<<<<`, `=======`, and `>>>>>>>` from the file to prevent any errors.

*Note: in VSCode merge conflicts are automatically highlighted in a different color where you then have the option to click “Accept Current Change” or “Accept Incoming Change” and VSCode will automatically cleanup the conflict* see [here](https://code.visualstudio.com/docs/editor/versioncontrol#_merge-conflicts)

## Deleting Merged Branches
Branches are usually created with the intention of adding code for a new feature, a bug patch, etc. once it is merged, the branch has practically little use in the whole scope of the project since its objective has already been merged; therefore, it is normally best practice to delete a branch that has already been merged and branch off the merged branch to continue further development under a different branch name with a separate purpose.

`git branch -d [branch-name]` — deletes a local branch  
`git push -d [remote-name] [branch-name]` — deletes a branch in remote repository (be extra careful)

*Always be cautious when deleting branches, the last thing anyone wants is for the wrong branch to be deleted from a repository*

## Pull Requests (PRs)
PRs provide the opportunity to:
- review code changes before merging
- discuss best practices
- suggest code changes
- merge code to a protected branch like `master`

*Pull requests may also require a certain number of approvals before it can be merged; this ensures that code is indeed reviewed before being merged*

Opening a PR is usually done with the following steps:
1. On the branch to be merged perform `git pull origin [branch-receiving-merge]`
2. Resolve any merge conflicts
3. Push merged code back up to remote branch
4. On GitHub GUI open up a new PR

*By performing a git pull first and resolving all conflicts locally, it prevents any merge conflicts from appearing in the PR when opened, thereby allowing immediate merging upon review*

## Best Practices
- Always ensure that the two branches being merged are up-to-date with the latest code from their remote repository (GitHub) before merging locally:
  1. Perform a `git fetch` on the branch getting merged
  2. Use `git checkout` to navigate to the branch that will receive the merge
  3. Perform a `git pull` on this branch to get latest changes
  4. Execute `git merge` and resolve any merge conflicts before pushing back up
- Normally, a branch is deleted once it has been merged in order to keep the amount of active branches relatively small
- `master` branch is often protected and will only allows changes through pull requests in order to merge code; this ensures that `master` always has the latest, production-ready, working code
- On large merge requests, commits are usually *squashed* so that the merge appears as one single commit rather than a history of all commits that occurred on the branch being merged; this can be done using the `--squash` option on `git merge`

- - - -

## Takeaways
Understanding how branching and merging works is very powerful for both team development and local development. Below are some practical scenarios and examples where branching and merging are often used.

#### Local Development Scenario
Say you want to test a different solution to a problem you are having with your local code, but do not want to overwrite the solution you have currently implemented; in this case, you can simply create another local branch (`branch-2`) with your second solution implemented and then compare with the original branch (`branch-1`) which solution is ideal. If solution 1 is ideal, you can simply delete `branch-2` without any repercussions; on the other hand, if solution 2 is ideal, you can merge `branch-2` into `branch-1` where merge conflicts may or may not occur. In either case, all branching and merging takes place locally with only the final implemented solution being pushed to GitHub.

```shell
# in both cases, create branch-2 from branch-1
$ git checkout branch-1
$ git checkout -b branch-2
# ...changes are made to branch-2...

# CASE 1: Solution 1 is ideal
# switch back to branch-1
$ git checkout branch-1
# delete local branch-2
$ git branch -d branch-2

# CASE 2: Solution 2 is ideal
# switch back to branch-1
$ git checkout branch-1
# merge branch-2 changes into branch-1
$ git merge branch-2
# ...resolve merge conflicts if needed...
# delete local branch-2
$ git branch -d branch-2
```

#### Team Development Scenario
Consider a team developing a large app already used by millions of people. The current application version that all users are currently using is located on the `master` branch. The team is looking to implement a new feature, but it first needs to be developed with adequate testing performed before integration. For this, the team decides to create a new branch called `new-feature` so that any new changes do not break the functioning application located on `master`. Over the next few weeks multiple developers branch off `new-feature` with their new changes also being merged or pushed into `new feature`. At some point, `new-feature` is ready to be merged into `master` so a new pull request is opened and reviewed by all team members. In this scenario, branching and merging takes place both locally and remotely on GitHub with the feature branch finally being merged into `master` via a pull request.

```shell
# branch off master and create initial new-feature branch
$ git checkout master
$ git checkout -b new-feature

# branch off new-feature to create branch for writing test cases
$ git checkout -b new-feature-tests
# ...write tests and push new-feature-test to remote repo...

# prepare new-feature-tests branch for merge into new-feature
# synchronize any changes team may have made to remote repo
$ git fetch
# switch to new-feature branch so any new changes can be pulled down
$ git checkout new-feature
# pull down any new changes to new-feature branch
$ git pull
# merge new-feature-tests into new-feature
$ git merge new-feature-tests
# ...resolve merge conflicts if needed...
# git add/commit merged files then push back to remote repo
$ git push origin new-feature
# delete new-feature-tests on remote repo
$ git push -d origin new-feature-tests
```

## Extra Resources
- [GitHub Git Cheat Sheet - GitHub Cheatsheets](https://github.github.com/training-kit/downloads/github-git-cheat-sheet/)  
- [What is the difference between ‘git pull’ and ‘git fetch’?](https://stackoverflow.com/questions/292357/what-is-the-difference-between-git-pull-and-git-fetch)  
- [Git Merge: How to Use Git Merge the Correct Way](https://dev.to/neshaz/how-to-use-git-merge-the-correctway-25pd)  
- [Git Merge](https://www.atlassian.com/git/tutorials/using-branches/git-merge)  
- [How To Delete a Local and Remote Git Branch](https://linuxize.com/post/how-to-delete-local-and-remote-git-branch/)  
