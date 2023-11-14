> [!SUMMARY]+ Table of Contents
>- [Git Introduction](Git%20&%20Github.md#Git%20Introduction)
>    - [Git Commit](Git%20&%20Github.md#Git%20Commit)
>    - [Git Branch](Git%20&%20Github.md#Git%20Branch)
>    - [Git Merge](Git%20&%20Github.md#Git%20Merge)
>    - [Git Rebase](Git%20&%20Github.md#Git%20Rebase)
>    - [Summary 1](Git%20&%20Github.md#Summary%201)
>- [Moving Around in Git](Git%20&%20Github.md#Moving%20Around%20in%20Git)
>    - [HEAD](Git%20&%20Github.md#HEAD)
>    - [Relative Refs ( ^ )](Git%20&%20Github.md#Relative%20Refs%20(%20^%20))
>    - [Relative Refs ( ~ )](Git%20&%20Github.md#Relative%20Refs%20(%20~%20))
>    - [Reset & Revert](Git%20&%20Github.md#Reset%20&%20Revert)
>    - [Summary 2](Git%20&%20Github.md#Summary%202)
>- [Another Git Command](Git%20&%20Github.md#Another%20Git%20Command)
>    - [Git Cherry-pick](Git%20&%20Github.md#Git%20Cherry-pick)
>    - [Git Tags](Git%20&%20Github.md#Git%20Tags)
>    - [Git Describe](Git%20&%20Github.md#Git%20Describe)
# Git Introduction
## Git Commit
- A commit in a git repository records a snapshot of all the (tracked) files in your directory. It's like a giant copy and paste, but even better!
- Git also maintains a history of which commits were made when. That's why most commits have ancestor commits above them -- we designate this with arrows in our visualization. Maintaining history is great for everyone working on the project!
- Let's see what this looks like in practice. Below we have a visualization of a (small) git repository. There are two commits right now -- the first initial commit, `C0`, and one commit after that `C1` that might have some meaningful changes.
- Command : `git commit`

![[Pasted image 20230902084541.png | 500]]

![[Pasted image 20230902084753.png | 500]]

- There we go! Awesome. We just made changes to the repository and saved them as a commit. The commit we just made has a parent, `C1`, which references which commit it was based off of.e

## Git Branch
- Branches in Git are incredibly lightweight as well. They are simply pointers to a specific commit -- nothing more. This is why many Git enthusiasts chant the mantra:
> branch early, and branch often
- Because there is no storage / memory overhead with making many branches, it's easier to logically divide up your work than have big beefy branches.
- When we start mixing branches and commits, we will see how these two features combine. For now though, just remember that a branch essentially says "I want to include the work of this commit and all parent commits."
- Here we will create a new branch named `newImage`.
- Command : `git branch newImage`

![[Pasted image 20230902085236.png | 500]]

![[Pasted image 20230902085300.png | 500]]

- There, that's all there is to branching! The branch `newImage` now refers to commit `C1`.
- Let's try to put some work on this new branch. Hit the button below.
- Command : `git commit`

![[Pasted image 20230902085343.png | 500]]

![[Pasted image 20230902085405.png | 500]]

- Oh no! The `main` branch moved but the `newImage` branch didn't! That's because we weren't "on" the new branch, which is why the asterisk (*) was on `main`.
- Let's tell git we want to checkout the `newImage` branch and make commit.
- Command : `git checkout newImage`
- Command : `git commit`

![[Pasted image 20230902085648.png | 500]]

![[Pasted image 20230902085714.png | 500]]

- There we go! Our changes were recorded on the new branch.
> _Note: In Git version 2.23, a new command called `git switch` was introduced to eventually replace `git checkout`, which is somewhat overloaded (it does a bunch of different things depending on the arguments). The lessons here will still use `checkout` instead of `switch` because the `switch` command is still considered experimental and the syntax may change in the future._
- By the way, here's a shortcut: if you want to create a new branch AND check it out at the same time, you can simply type `git checkout -b [yourbranchname]`.
## Git Merge
- The first method to combine work that we will examine is `git merge`. 
- Merging in Git creates a special commit that has two unique parents.
- Here we have two branches; each has one commit that's unique. 
- This means that neither branch includes the entire set of "work" in the repository that we have done. 
- Let's fix that with merge. 
- We will `merge` the branch `bugFix` into `main`.

![[Pasted image 20230907184321.png | 500]]

- Command : 
- `git merge bugFix`

![[Pasted image 20230907184408.png | 500]]

- First of all, `main` now points to a commit that has two parents. 
- If you follow the arrows up the commit tree from `main`, you will hit every commit along the way to the root. 
- This means that `main` contains all the work in the repository now.
- Let's merge `main` into `bugFix`

![[Pasted image 20230907184525.png | 500]]

- Command : `git checkout bugFix`
- Command : `git merge main`

![[Pasted image 20230907184631.png | 500]]

- Since `bugFix` was an ancestor of `main`, git didn't have to do any work; it simply just moved `bugFix` to the same commit `main` was attached to.

## Git Rebase
- The second way of combining work between branches is _rebasing._ 
- Rebasing essentially takes a set of commits, "copies" them, and plops them down somewhere else.
- The advantage of rebasing is that it can be used to make a nice linear sequence of commits. 
- The commit log / history of the repository will be a lot cleaner if only rebasing is allowed.
- Here we have two branches yet again; note that the `bugFix` branch is currently selected (note the asterisk)
- We would like to move our work from bugFix directly onto the work from main. 
- That way it would look like these two features were developed sequentially, when in reality they were developed in parallel.

![[Pasted image 20230907185016.png | 500]]

- Command : `git rebase main`

![[Pasted image 20230907185051.png | 500]]

- Now the work from our bugFix branch is right on top of main and we have a nice linear sequence of commits.
- Note that the commit C3 still exists somewhere (it has a faded appearance in the tree), and C3' is the "copy" that we rebased onto main.
- Now we are checked out on the `main` branch. 
- Let's go ahead and rebase onto `bugFix`

![[Pasted image 20230907185152.png | 500]]

- Command : `git rebase bugFix`

![[Pasted image 20230907185234.png | 500]]

- Since `main` was an ancestor of `bugFix`, git simply moved the `main` branch reference forward in history.

## Summary 1
- List of commands : 
	- `git commit` - to make commit
	- `git branch <branch_name>` - to make a new branch 
	- `git checkout <branch_name>` - to switch branch
	- `git checkout -b <branch_name>` - to create and switch new branch at the same time
	- `git merge <branch_name>` - to merge current branch into another branch
	- `git rebase <branch_name>` - to rebase current branch into another branch

# Moving Around in Git
## HEAD
- HEAD is the symbolic name for the currently checked out commit -- it's essentially what commit you're working on top of.
- HEAD always points to the most recent commit which is reflected in the working tree. 
- Most git commands which make changes to the working tree will start by changing HEAD.
- Normally HEAD points to a branch name (like bugFix). When you commit, the status of bugFix is altered and this change is visible through HEAD.

![[Pasted image 20230907194130.png | 500]]

- Command : `git commit`
- Command : `git checkout c2`

![[Pasted image 20230907194152.png | 500]]

- HEAD was hiding underneath our `main` branch all along.
- Detaching HEAD just means attaching it to a commit instead of a branch.

![[Pasted image 20230907194330.png | 500]]

- Command : `git checkout c1`

![[Pasted image 20230907194357.png | 500]]

## Relative Refs ( ^ )
- Moving around in Git by specifying commit hashes can get a bit tedious. 
- In the real world you won't have a nice commit tree visualization next to your terminal, so you'll have to use `git log` to see hashes.
- Furthermore, hashes are usually a lot longer in the real Git world as well. 
- For instance, the hash of a commit can be `fed2da64c0efc5293610bdd892f82a58e8cbc5d8`.
- The upside is that Git is smart about hashes. 
- It only requires you to specify enough characters of the hash until it uniquely identifies the commit. 
- So I can type `fed2` instead of the long string above.
- With relative refs, you can start somewhere memorable (like the branch `bugFix` or `HEAD`) and work from there.
- Relative commits are powerful, but we will introduce two simple ones here:
	- Moving upwards one commit at a time with `^`
	- Moving upwards a number of times with `~<num>`
- Let's look at the Caret (^) operator first. 
- Each time you append that to a ref name, you are telling Git to find the parent of the specified commit.
- So saying `main^` is equivalent to "the first parent of `main`".
- `main^^` is the grandparent (second-generation ancestor) of `main`

![[Pasted image 20230907194842.png | 500]]

- Command : `git checkout main^`

![[Pasted image 20230907194919.png | 500]]

- You can also reference `HEAD` as a relative ref. 
- Let's use that a couple of times to move upwards in the commit tree.

![[Pasted image 20230907194959.png | 500]]

- Command : `git checkout c3`
- Command : `git checkout HEAD^`
- Command : `git checkout HEAD^`
- Command : `git checkout HEAD^`

![[Pasted image 20230907195056.png | 500]]

- The `^` modifier accepts an optional number after it.
- The modifier on `^` specifies which parent reference to follow from a merge commit. 
- Remember that merge commits have multiple parents, so the path to choose is ambiguous.
- Git will normally follow the "first" parent upwards from a merge commit, but specifying a number with `^` changes this default behavior.

![[Pasted image 20230907204417.png | 500]]

- Command : `git checkout main^2`

![[Pasted image 20230907204504.png | 500]]
( The "first" parent is directly above main)

## Relative Refs ( ~ )
- If you want to move a lot of levels up in the commit tree, it might be tedious to type `^` several times, so Git also has the tilde (~) operator.
- The tilde operator (optionally) takes in a trailing number that specifies the number of parents you would like to ascend.

![[Pasted image 20230907195248.png | 500]]

- Command : `git checkout HEAD~4`

![[Pasted image 20230907195327.png | 500]]

- One of the most common ways to use relative refs is to move branches around. 
- You can directly reassign a branch to a commit with the `-f` option.

![[Pasted image 20230907195447.png | 500]]

- Command : `git branch -f main HEAD~3`

![[Pasted image 20230907195533.png | 500]]

- Relative refs gave us a concise way to refer to `C1` and branch forcing (`-f`) gave us a way to quickly move a branch to that location.
- Relative refs can also be chained together

![[Pasted image 20230907204657.png | 500]]

- Command : `git checkout HEAD~^2~2`

![[Pasted image 20230907204742.png | 500]]

## Reset & Revert
- There are two primary ways to undo changes in Git -- one is using `git reset` and the other is using `git revert`.
- `git reset` reverses changes by moving a branch reference backwards in time to an older commit. 
- In this sense you can think of it as "rewriting history;" `git reset` will move a branch backwards as if the commit had never been made in the first place.

![[Pasted image 20230907195802.png | 500]]

- Command : `git reset HEAD~1`

![[Pasted image 20230907195844.png | 500]]

- Git moved the main branch reference back to `C1`; now our local repository is in a state as if `C2` had never happened.
- While resetting works great for local branches on your own machine, its method of "rewriting history" doesn't work for remote branches that others are using.
- In order to reverse changes and _share_ those reversed changes with others, we need to use `git revert`.

![[Pasted image 20230907195933.png | 500]]

- Command : `git revert HEAD`

![[Pasted image 20230907200008.png | 500]]

- Weird, a new commit plopped down below the commit we wanted to reverse. 
- That's because this new commit `C2'` introduces _changes_ -- it just happens to introduce changes that exactly reverses the commit of `C2`.
- With reverting, you can push out your changes to share with others.
## Summary 2
- List of commands :
	- `git checkout <commit_name(hash)>` - to detach HEAD ( to point to a commit )
	- `git checkout <branch_name>^<number>` - to point to commit using relative refs ( parent )
	- `git checkout <branch_name>~<number>` - to point to commit using relative refs 
	- `git branch -f <branch_name> <another_branch_name/relative_refs>` - to force move a branch
	- `git reset <current_branch/commit_parent>` - to reset commit ( to parent )
	- `git revert <current_branch/commit>` - to reset remotely

# Another Git Command
## Git Cherry-pick
- The command is called `git cherry-pick`. 
- It takes on the following form:
	- `git cherry-pick <Commit1> <Commit2> <...>`
- It's a very straightforward way of saying that you would like to copy a series of commits below your current location (`HEAD`).
- Here's a repository where we have some work in branch `side` that we want to copy to `main`. 
- This could be accomplished through a rebase, but let's see how cherry-pick performs.

![[Pasted image 20230907205140.png | 500]]

- Command : `git cherry-pick C2 C4`

![[Pasted image 20230907205223.png | 500]]
## Git Tags
- Git tags permanently mark certain commits as "milestones" that you can then reference like a branch.
- More importantly though, they never move as more commits are created. 
- You can't "check out" a tag and then complete work on that tag -- tags exist as anchors in the commit tree that designate certain spots.
- Let's try making a tag at `C1` which is our version 1 prototype.

![[Pasted image 20230907205407.png | 500]]

- Command : `git tag v1 C1`

![[Pasted image 20230907205447.png | 500]]

- We named the tag `v1` and referenced the commit `C1` explicitly. 
- If you leave the commit off, git will just use whatever `HEAD` is at.
## Git Describe
- Because tags serve as such great "anchors" in the codebase, git has a command to _describe_ where you are relative to the closest "anchor" (aka tag). 
- That command is called `git describe`
- Git describe takes the form of:
	- `git describe <ref>`
- Where `<ref>` is anything git can resolve into a commit. 
- If you don't specify a ref, git just uses where you're checked out right now (`HEAD`).
- The output of the command looks like:
	- `<tag>_<numCommits>_g<hash>`
- Where `tag` is the closest ancestor tag in history, `numCommits` is how many commits away that tag is, and `<hash>` is the hash of the commit being described.

![[Pasted image 20230907205726.png | 500]]

- The command `git describe main` would output:
	- `v1_2_gC2`
- Whereas `git describe side` would output:
	- `v2_1_gC4`