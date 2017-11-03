# Git

## Dispaly Commits
  * Use `git log --oneline` to review the short version of git logs.
  * Use `git log --stat` to get more information such as edited files' names.
  * `git log -p`: display the actual changes made to a file, this command palys the same role as `git show`. The flag is `--patch` which can be shortened to just `-p`
  * `git log -p --stat` delivers more details.
  * `git -log -w`: show patch(edited/distingct) information by ignoring whitespace changes.
  * `git -log -p SHA_code`: supply a SHA code without scolling in the log history.

## Navigating The Log
  * to scroll **down**, press
    * `j` or `↓` to move _down_ one line at a time
    * `d` to move by half the page screen
    * `f` to move by a whole page screen
  * to scroll **up**, press
    * `k` or `↑` to move _up_ one line at a time
    * `u` to move by half the page screen
    * `b` to move by a whole page screen

## Commit
  The `git diff` command can be used to see changes that have been made but haven't been committed, yet. `git diff` is similar to `git log -p` that is because `git log -p` uses `git diff` under the hood.


## Tagging
#### Annoted tag & Lightweit tag
  **`git tag -a v1.0` `git tag v1.0`**

  In the command above (`git tag -a v1.0`) the `-a` flag is used. This flag tells Git to create an _annotated_flag. If you don't provide the flag (i.e. `git tag v1.0`) then it'll create what's called a _lightweight_ tag.

  Annotated tags are recommended because they include a lot of extra information such as:

  * the person who made the tag
  * the date the tag was made
  * a message for the tag

  Because of this, you should always use annotated tags.

  The `--decorate` flag will show us some details that are hidden from the default view. `git log --oneline --decorate`

  A Git tag can be deleted with the `-d` flag (for _delete_!) and the name of the tag: **`git tag -d v1.0`**. `git tag -d` stands for `git tag --delete`.

#### Tagging a older commit
  `git tag -a v1.0 b325e35` Append the commit's SHA code at the end of the command.

## Branch
  * `git branch`: Check branch information.
  * `git branch branch_name`: Create a new branch named 'branch_name' on the current commit.
  * `git branch name b325e35`: Create a new branch on the commit with the SHA code of 'b325e35'.
  * `git branch -d name`: Delete a branch according to the branch name. Git not allows user to delete the branch which is currently checkout. The branch is not going to be deleted if there is unique/new code on that branch. To break this restriction, use `git branch -D branch_name`
  * The `git checkout` command can actually create a new branch, too. If you provide the `-b`flag, you can create a branch _and_ switch to it all in one command: **`git checkout -b richards-branch-for-awesome-changes`**
  * **See All Branches At Once: `git log --oneline --decorate --graph --all`**

## Merging
  Common command: **`git merge <other-branch-name>`**

  Undo merge: `git reset --hard HEAD^` (Make sure to include the `^` character! It's a known as a "Relative Commit Reference" and indicates "the parent commit".)

  When a merge happens, Git will:

  * look at the branches that it's going to merge
  * look back along the branch's history to find a single commit that _both_ branches have in their commit history
  * combine the lines of code that were changed on the separate branches together
  * makes a commit to record the merge

  **Fast-forward merge**: A Fast-forward merge will just move the currently checked out branch _forward_until it points to the same commit that the other branch is pointing to.

#### Merge Conflict Indicators Explanation
  The editor has the following merge conflict indicators:

  * `<<<<<<< HEAD` everything below this line (until the next indicator) shows you what's on the current branch
  * `||||||| merged common ancestors` everything below this line (until the next indicator) shows you what the original lines were
  * `=======` is the end of the original lines, everything that follows (until the next indicator) is what's on the branch that's being merged in
  * `>>>>>>> heading-update` is the ending indicator of what's on the branch that's being merged in (in this case, the `heading-update` branch)

## Modifying The Last Commit
  **Changing The Last Commit**

  > with the `--amend` flag, you can alter the _most-recent_ commit: `git commit --amend`

  **Add Forgotten Files To Commit**:
  >* edit the file(s)
   * save the file(s)
   * stage the file(s)
   * and run `git commit --amend`

## Reverting A Commit
  `git revert <SHA-of-commit-to-revert>`
  > When you tell Git to revert a specific commit, Git takes the changes that were made in commit and does the exact opposite of them. Let's break that down a bit. If a character is added in commit A, if Git reverts commit A, then Git will make a new commit where that character is deleted. It also works the other way where if a character/line is removed, then reverting that commit will add that content back!

## Resetting Commits
  `git reset <reference-to-commit>`

  It can be used to:

  * move the HEAD and current branch pointer to the referenced commit
  * erase commits with the `--hard` flag
  * moves committed changes to the staging index with the `--soft` flag
  * unstages committed changes `--mixed` flag

Typically, ancestry references are used to indicate previous commits. The ancestry references are:

  * `^` – indicates the parent commit
  * `~` – indicates the first parent commit

#### Reset VS Revert
  At first glance, _resetting_ might seem coincidentally close to _reverting_, but they are actually quite different. Reverting creates a new commit that reverts or undos a previous commit. Resetting, on the other hand, _erases_ commits!
  > ##### ⚠️ Resetting Is Dangerous ⚠️
  You've got to be careful with Git's resetting capabilities. This is one of the few commands that lets you erase commits from the repository. If a commit is no longer in the repository, then its content is gone.

  >To alleviate the stress a bit, Git _does_ keep track of everything for about 30 days before it completely erases anything. To access this content, you'll need to use the `git reflog` command. Check out these links for more info:

  >* [git-reflog](https://git-scm.com/docs/git-reflog)
  * [Rewriting History](https://www.atlassian.com/git/tutorials/rewriting-history)
  * [reflog, your safety net](http://gitready.com/intermediate/2009/02/09/reflog-your-safety-net.html)

  **`git reset <reference-to-commit>`**

  * move the HEAD and current branch pointer to the referenced commit
  * erase commits
  * move committed changes to the staging index
  * unstage committed changes

#### Git Reset's Flags

The way that Git determines if it erases, stages previously committed changes, or unstages previously committed changes is by the flag that's used. The flags are:

  * `--mixed` : As the same as wihtout this flag, which will put the most rencent commited (changed) files to the original workplace directory.
  * `--soft` : Put the most rencent commited (changed) files back to staging area/index.
  * `--hard` : Put the most rencent commited (changed) files to "Trash" directory of Git. In order to get access to the "Trash", use `git reflog` command.

> #### 💡 Backup Branch 💡

>Remember that using the `git reset` command will _erase_ commits from the current branch. So if you want to follow along with all the resetting stuff that's coming up, you'll need to create a branch on the current commit that you can use as a backup.

>Before I do any resetting, I usually create a `backup` branch on the most-recent commit so that I can get back to the commits if I make a mistake:

>    `git branch backup`

> #### 💡 Back To Normal 💡

>If you created the `backup` branch prior to resetting anything, then you can easily get back to having the `master`branch point to the same commit as the `backup` branch. You'll just need to:

  >1. remove the uncommitted changes from the working directory
  2. merge `backup` into `master` (which will cause a Fast-forward merge and move `master` up to the same point as `backup`)
>     * git checkout -- index.html
      * git merge backup

## Relative Commit References
  There are special characters called "Ancestry References" that we can use to tell Git about these relative references.

  * `^` – indicates the parent commit
  * `~` – indicates the _first_ parent commit

  Here's how we can refer to previous commits:

  * the parent commit – the following indicate the parent commit of the current commit
    * HEAD^
    * HEAD~
    * HEAD~1
  * the grandparent commit – the following indicate the grandparent commit of the current commit
    * HEAD^^
    * HEAD~2
  * the great-grandparent commit – the following indicate the great-grandparent commit of the current commit
    * HEAD^^^
    * HEAD~3

  The main difference between the `^` and the `~` is when a commit is created _from a merge_. A merge commit has _two_ parents. With a merge commit, the `^` reference is used to indicate the _first_ parent of the commit while `^2` indicates the _second_parent. The first parent is the branch you were on when you ran `git merge` while the second parent is the branch that was merged in.

  It's easier if we look at an example. This what my `git log` currently shows:

    * 9ec05ca (HEAD -> master) Revert "Set page heading to "Quests & Crusades""
    * db7e87a Set page heading to "Quests & Crusades"
    *   796ddb0 Merge branch 'heading-update'
    |\
    | * 4c9749e (heading-update) Set page heading to "Crusade"
    * | 0c5975a Set page heading to "Quest"
    |/
    *   1a56a81 Merge branch 'sidebar'
    |\
    | * f69811c (sidebar) Update sidebar with favorite movie
    | * e6c65a6 Add new sidebar content
    * | e014d91 (footer) Add links to social media
    * | 209752a Improve site heading for SEO
    * | 3772ab1 Set background color for page
    |/
    * 5bfe5e7 Add starting HTML structure
    * 6fa5f34 Add .gitignore file
    * a879849 Add header to blog
    * 94de470 Initial commit

Let's look at how we'd refer to some of the previous commits. Since `HEAD` points to the `9ec05ca` commt:

  * `HEAD^` is the `db7e87a` commit
  * `HEAD~1` is also the `db7e87a` commit
  * `HEAD^^` is the `796ddb0` commit
  * `HEAD~2` is also the `796ddb0` commit
  * `HEAD^^^` is the `0c5975a` commit
  * `HEAD~3` is also the `0c5975a` commit
  * `HEAD^^^2` is the `4c9749e` commit (this is the grandparent's (`HEAD^^`) **_second parent_ **(`^2`))

## Working with Remotes
  Show reomote path: `git remote -v`

  * `git add remote url`
  * `git push <remote-shortname> <branch>`
  * `git pull <remote-shortname> <branch>`

**`git pull origin master` vs `git fertch origin master`**

  Pull makes a fast forward merge. However, fertch pull the origin/master to local repository but not merge it with the local master, that keeps the local master HEAD at its original state. In order to merge them, use `git merge origin/master`

#### Review other's commits
  `git shortlog`: displays an alphabetical list of names and the commit messages that go along with them.

  If we just want to see just the number of commits that each developer has made, we can add a couple of flags: -s to show just the number of commits (rather than each commit's message) and -n to sort them numerically (rather than alphabetically by author name): `git shortlog -s -n`
#### Search someone's commit according to his/her name:
  `git log --author=Name`

  `git log --oneline --author=Name`

#### Filter Commits By Search
  **`git show SHA_code`**

  **`git log --grep=key_word`**
> If you were to run `git log --grep "fort"`, then Git will display only the commits that have the character `f` followed by the character `o` followed by `r` followed by `t`.

#### Renameing Remotes
`git remote rename original_name new_name`: It is applicable to both upstream and origin(own or forked repos).

#### Retrieving Upstream Changes
  **`git fetch upstream master`**

#### Squashing Commits
  Use `git rebase` to squash commmits.

  * `git rebase -i HEAD~commits_amount`: **`git rebase -i HEAD~3`** will suqashing the most 3 rencent commits into 1 commits, and this command also deletes those 3 commits without warning. The `~3` part means "three before".

  * In order to keep the squashed commits in the repo, it is a good habit to add a new branch auch as "backup" at the HEAD before squashing.

  * [Direction video](https://youtu.be/cL6ehKtJLUM)

#### Force Pushing
  **⚠️DANGER ⚠️**

  After squashing the commits, GitHub would reject the push request since it needs to deletes those squashed commits. To deal with this situation, use `git push -f` command.







