# **Git notes**

## Git commit style

*Commit style:* (PROJECT?)-(TYPO): TITLE... DESCRIPTION...
*Example:* JS-PROJECTS-FIX: TITLE... DESCRIPTION...

## Most used commands

- git push, pull, fork
- git blame, revert, rebase, log, shortlog 
- git show, git diff, git log -p
- git log --oneline
- git shortlog
- git merge
- git reflog
- git commit --amend
- git show --quiet --pretty=fuller
- git commit -C ORIG_HEAD --reset-author
- git commit --amend --no-edit --date=now

## Helpful tricks

- ls -a; (show the list of files in project)
- git show(+ --pretty=fuller) (show all about commit)
- git commit --author='...' --date='..' (changing author/commiter)
- git add -p index.html (to see which changes in the file must be in commit)
- git commit -am "msg"(without git add)
- git commit .gitignore(or any other file, that must be commited right now, without adding other files)
- git config --global alias.commitall '!git add -A; git commit' ----> git commitall -m ...(creation of the alias, if u want to commit everything at the moment)
- git add -A(add every change, from the root of the project)
- git rm (path)/(-r src)(removing src directory, if add after "-r" --cached, then file'll be translated into untracked category)
- git mv index.html hello.html(rename the file and add to commit)
- git diff --cached

### Branches 

1. Creating a branch


- git branch(-v) branch-name (shows all the branches in rep / create the branch)

> **Note:** HEAD -> master -> identifier of commit (HEAD is just a link of the current branch/commit)
new commit(peak of master branch) <- master <- HEAD 
new branch - creation of new link to the latest commit
technically branch is just a link to one specific commit

- git checkout branch-name(relinking to the new branch)
- git checkout -b feature (branch creation + momental relinking)

> **Note:** if we need to change branch from newest commit to old, then "git checkout" will change link in HEAD and return files in working directory to the oldest commit version in other branch

2. Checkout with uncommited changes

> **Note:** if change were uncommitted: 1)you can erase them "checkout -f"; 2)use "git stash (+pop - to return unsaved changes)", but if you do that in other branch, then it can make error

- git checkout -f (HEAD)(it will move u to the latest commit and erase all changes)
- git stash(saves all changed files without committing)
- git stash pop(returns all unsaved changes)

> **Note:** if files were the same in latest commits in other branches and you changed one of them, then tried to move to another branch - it will give u a message that file was modified and changes won't be erased

3. Moving uncommited changes

> **Note:** if u have to commit changes immediately, but u cant do it in main branch, then use "git checkout -b branch-name" and create a temporary branch where you'll save all the code and finish it later

4. Moving branches "manually"

> **Note:** It can be used when u've done a unnecessary commit and want to move it to another branch

- 1)git branch fix; 2)git branch -f master(main) 1234(code of the commit) 
> **Attention:** you need to be on another
branch to change place of master(main) branch) *)if u want to reset changes, then type "git branch -f master(main) fix(/code of the commit)"

> **Note:** For the same thing it can be easy to use "git checkout -b master 1234(code of the commit)"

> **Interesting:** if u'll type "-B" and we already have that branch, then it'll move and relink on it 

5. HEAD detached status

> **Note:** U can use "git checkout 1234(code of the commit)" to move to the current commit instantly, that' when HEAD is detached. If u've commited smth in that state, it'll create an unnamed branch and it may be difficult to move to those commits in future, also Git can delete them in time, SO TO AVOID IT "git branch <`new-branch-name`> <`commit-code-in-head-detached-state`>"

> **Note:** Or there is another way - you can move that commit to the top of selected branch(git cherry-pick)
- git cherry-pick <`commit-code`>(it will move changes to the top of previously selected branch)

6. Recovery of the previous file versions(only FILES, not entire PROJECT)

> **Note:** If u need to recover prev. file ver. to current commit - use git checkout(it automatically adds them into index also)
- git checkout <`prev-commit-code`> <`recovery-path`> (ex: git checkout main index.html)

> **Note:** If u want to give up changes in INDEX, type "git reset <`path-to-reset`>" (they'll stay in working directory)
- git reset index.html

> **Note:** If u want to reset current file in working dir. (without changing other files), use "git checkout HEAD <`path-to-reset`>" If path is typed it'll reset changes from repo, if not - latest changes from index
Typing "git checkout -- master" says that master is a PATH
- git checkout HEAD index.html

7. Look through history and old files, symbols ~, ^, @, search c :/

> **Note:** Use "git log" or "git log--oneline" to see the commits history, also "git log <`branch-name`> --oneline" If u want to see specific commit, then use "git show <`commit-code`>" or u can use "git show HEAD~" to see previous commit without remembering their codes. Number of '~' will show the lower commit u want to Usage of "git show HEAD~2 --quiet" will give short info on commit Also we can use '@' instead of 'HEAD'(In powershell use " '@' "), for example: git checkout @~ index.html

> **Important:** if u want to see specific file in prev. commits, type: "git show <`steps-to-prev-commit`>: <`path`>"
- git show @~:index.html (or: git show fix:index.hmtl) or even: git show :index.html(shows last indexed changes)

> **Note:** Also u can look for exact word to show the file: git show :/someFunc

8. Merging branches using "fast-forward"

> **Note:** To merge branches u need to relocate to the branch, to which u wanna merge another
- git merge <`branch-to-merge-with`>

> **Note:** To reverse the merge use "git branch -f <`branch-to-reverse`> <`commit-code`>", if u don't remember the code use "cat .git/ORIG_HEAD"
- git branch -f main 1234
- git branch -f main ORIG_HEAD (In case u don't remember the code)
- git checkout -B master fix (U can use that instead of "git merge")

9. Branch deletion

> **Note:** If u have already merged branches, then u need to delete the previous one, use "git branch -d <`branch-name`>"
- git branch -d fix

> **Note:** But if u want to delete unmerged branch, then use "git branch -D <`branch-name`>"
In case that deletion was an error, use "git branch <`deleted-branch-name`> <`deleted-branch-commit-code`>"
- git branch -D feature
- git branch feature 1234

10. History of branch switching(links log - reflog)(to see: cat .git/logs/HEAD(any other branch))

> **Note:** Reflogs are saved locally, on your machine only, they don't go to server

- git reflog <`branch-name`> (if not - it'll show history from HEAD's place)
- git log --oneline -g (the same)
- git reflog --date=iso

> **Note:** It can be used to recover needed branch:
- git branch <`branch-name`> <`commit-code`>
- git branch feature HEAD@{6}
- git reflog --no-decorate
- git checkout -(relink to prev branch)

11. Deletion of the non-important changes

> **Note:** These command do not affect on the untracked files:
- git checkout -f
- git reset --hard

> **Note:** That does(affects only on untracked files/dirs.):
- git clean
- git clean -d(files + dirs)
- git clean -x(plus files from .gitignore)
- git clean -dxf(altogether with force)

12. Hard reset, discard changes, commit deletion 

> **Note:** Making full reset to prev. commit
- git reset <`commit-to-reset-to`>
- git reset @~2
- git reset --hard @~ (returning to prev. commit)

> **Note:** If u need to reset changes back:
- git reset --hard <`commit-code`>
- git reset --hard ORIG_HEAD

13. Soft reset, changing and joining commits

> **Note:** It doesn't affect on Index and Working directory(saves files from upper commit)
- git reset --soft @~()
- git commit -c ORIG_HEAD (Takes description from last commit)
- git commit --amend(also usable, resets to the prev commit, commits new changes - 2 steps at a time instantly)

14. Mixed reset, it's comparison

> **Comparison:** () - changes
- work. dir + index + (curr. brnch) === reset --soft
- work. dir + (index) + (curr. brnch) === reset --mixed (default)
- (work. dir) + (index) + (curr. brnch) === reset --hard

> **_________________________________________________**

- git reset (this will delete changes from index)
- git reset <`commit-code`> <`path`> (this resets files only in index)
- git reset 1234 index.html

> **Useful:** tables with reset actions (git help reset)

### Commits

1. Commit(/branch) comparison

- git diff <`from-commit-code`> <`to-commit-code`>
- git diff master 23e4
- git diff master...feature(what was made in feature compare to master)
- git diff HEAD (compares your changes with ones in repository)
- git diff (compares your changes with ones in INDEX)
- git diff --cached (compares your added to index changes with repository files)
- git diff master feature index.html (difference only for file index.html)
- git diff commit1:path1 commit2:path2
- git diff --name-only master feature (files where changes are)
- git diff --no-index path1 path2 (compares two absolutely different files)

2. History output

> **Interesting:**
- abbrev-commit (shorten the commit code)
- no-decorate (turns of decoration of the links/branches)
- git config --global pretty.main format:'%C(yellow)%h %C(dim green)%ad %C(reset) | %C(cyan)%s%d %C(#667788)[%an]' --date=format:'%F %R'
- git config --global format.pretty main

> **__________________________________**

- git log --pretty=(format)
- git log --pretty=format:'%h %cd | %s%d [%an]' --date=short
- git log --pretty=format:'%C(yellow)%h %C(dim green)%ad %C(reset) | %C(cyan)%s%d %C(#667788)[%an]' --date=format:'%F %R'
- git log -p (shows detailed history of all commits, adds diff to every commit)

3. Commits range

- git log master (shows logs from master branch)
- git log master feature --graph (shows three of commits with typed branches)
- git log --all --graph (the same)
- git log feature ^master (shows all the commits, which were made aside from master in 'feature' branch)
- git log master..feature (the same)
- git log ..feature (taking log from the branch you're sitting on)
- git log feature.. --boundary (includes commit, on which branches have splitted)
- git log master...feature --boundary --graph (master&feature&boundary commits)

4. Files history

- git log index.html (seeing logs of index.html, commits in which it participated)
- git log -p script.js (seeing detailed changes in it)
- git log -p --follow script.js (if file was renamed)
- git log feature..master -- dir1 dir2 ...

5. Search in history, filters for 'git log'

- git log --grep Run <`branchname`> (shows commits where word 'Run' was used in the name of commit)
- git log --grep Run --grep `sayHi` --all-match (commit with only name 'Run sayHi')
- git log --grep '`say(Hi|Bye)`' -P (commits with '...sayHi...', '...sayBye...')
- git config --global grep.patternType perl (changes reg. exp to perl compatible)
- git log -G`sayHi` -p script.js(shows commits where this string/func was changed, only in string.js) - very usable!!!
- git log -L '/<`head`>/', '/<\/`head`>/':index.html(shows exact commits with changes in body of smth) - also usable
- git log -L '/^`function sayHi`/', '/^}/':script.js (shows the history of sayHi)
- git log --before(--after) '2017-09-13'

6. Usage of git blame

- git blame <`path`> (shows us line-to-line code of the file and author of that code)

7. Truly merge and conflicts solving

- git status (to check if there is smth to commit)
- git diff --name-only main feature (which files are different)
- git merge-base main feature (commit where branches have splitted up)
- git merge feature (try to auto merge 'feature' branch)
- git diff -U0 1234 master index.html (see the difference without useless lines of code)
- git checkout --ours index.html (saves changes in conflict only from our branch)
- git checkout --merge index.html (saves changes from both branches)
- git reset --hard (give up the merge)
- git reset --merge (saves files that were not in index and deletes the others)
- git merge --abort (the same as reset --merge)
- git checkout --conflict=diff3 --merge index.html (shows code which was before the spliting of branches)
- git add <`path`> (add files where u've solved the conflict)
- git merge --continue (the same as "git commit")

8. "Merge" commit, further actions

> **Note:** That commit has privileges: it has two parental commits from merging, also it shows differences in combined condensed diff(shows differences only in conflict files, how they were solved)

^ - needed only when working with "merge" commits, in defaults use '~'

- git log --oneline --all --graph(shows full history as a tree)
- git show (has 2 parental links)
- git show --first-parent (shows changes from the first commit)
- git show -m (shows differences from two parents in default format)
- git diff @^(first parent) or @^2(second parent)
- git diff @^2~2(shows the second parent of the second parent)
- git branch --merged (shows branches which were merged with current branch)
- git merge feature --log=2 (adds all the descriptions of the prev comms to the "merge" commit(current commit), limited by 2)
- git log master --oneline --first-parent(shows history of master without merged branches)

9. Merge cancelling, reverting

> **Attention:** git reset --hard @~ (this wont help bcs u will move to the parent of branch from which u did the merge) If u have done something like that use "git reflog" & "git reset --hard @{n-steps}"

10. Semantic troubles after merging and their solving

> **Note:** That happens when merge was "successful", but your prog. wont work because of semantic troubles, due to leaving in  branch only working commits(without bugs), we need to cancel merging and redo it again

- git reset --hard @~
- git merge <`branchname`> --no-commit
- git merge --continue (after fixing semantic troubles)

11. Useful trick, flag --no-ff

> **Useful:**  this cancels fast-forward merging(it can be very hard to differ branches when bug is found)
Use "git config merge.ff false" to do it globally(not necessary to write --no-ff)

- git merge --no-ff <`branchname`>

12. Creating commit from branch(merge --squash)

- git merge --squash <`branchname`> (takes changes from the latest commit of specific branch and moves it to our branch, without merging the branches, it just adds it to working dir and index)

13. Copying of the commits(git cherry-pick)

> **Note:** That command picks up the differences from the last commit and applies them to the needed branch. Use it from the branch to which you want to apply changes. If the error occured - do the same things when solving  merge problems

- git cherry-pick <`commit-code`>
- git cherry-pick -x (shows from where the copy was done)
- git cherry-pick master..feature(copy of the commits from 'feature' branch)
- git cherry-pick --quit (remembers the current state, if error occured during copying the commit)
- git cherry-pick --continue (to continue the copying)
- git reset --hard @~(n) (if you want to revert the changes)
- git cherry-pick --no-commit (if you want to add something during copying)

14. Commits moving (git rebase)

> **Note:** That picks up your branch on moves it on top of another branch, also that picks up changes from the branch to which you are adding yours branch. Use that from the branch you want to move. If the error occured - do the same things when solving  merge problems

- git rebase <`branch-to-rebase-to`> (or to rebase 'feature' on 'master': git rebase master feature)
- git rebase --abort (to return to the previous state)
- git rebase --skip (to skip the errored commit and start to rebasing the next one)

> **Note:** If rebasing was an error, do the next steps:
- git reflog feature
- git show --quiet feature@{1} (see the old peak of the chosen branch, see if it is the right one)
- git reset --hard feature@{1} (or use 'git reset --hard ORIG_HEAD')

15. Comparison of rebase & merge

> **Note:** See the video or read the articles
Use the automatic test when rebasing the branch (git rebase -x '...' master --> it can help to run automatic tests) rebase - can make easier the history of the project, but can break a lot of commits, so u need to be very attentive,  but it can be very easy to take changes in that way

16. Rebase with tag -x

> **Remember:** when (grammar) error in code happened commit is already saved, so you need to correct mistake and redo that commit

- git rebase -x 'node feature.js' master
- git commit --amend --no-edit (to change the current commit with bug)
- git rebase --continue (to finish the rebasing)

17. Rebasing part of the branch (git rebase --onto)

> **Note:** When u've created a branch in wrong place and want to rebase it to another branch, but u dont need to pick the whole branch, only part of it. Also u need to be on top of the branch, which u want to rebase.
Be VERY attentive with that command, because u can move wrong branch by mistake

- git rebase --onto <to-branch/commit> <from-commit-code/branch>
- git rebase --onto master feature (rebasing the 'fix' branch to the master branch)
- git rebase --onto master feature fix (checkout on 'fix' automatically)

> **Note:** If decided to move only commits use 'git cherry pick'

- git checkout feature
- git cherry-pick master~2..master (picks up two commits from 'master' to 'feature' branch)
- git branch -f master master~2 (dont forget to move the branch)

18. Rebasing of the merges (--rebase-merges)

> **Note:** By default rebase mises the 'merging' commit and moves branches without it, to avoid it:
(if mistake happened use "git reset --hard <`branch`>@{n-steps back}")

- git rebase --rebase-merges <`branch-to-rebase-to`>

> **Note:** Also if in 'merging' commit was an error during merging, it will occur again and u need to fix it(use the ability of git 'rerere')

19. Interactive rebasing (rebase -i)

> **Note:** IF rebase the whole branch - it will move to the top of another branch, but if you do the rebasing in borders of yours branch, then branch will stay on its current place

git config rebase.missingCommitsCheck warn/error

- pick = use commit
- reword = use commit, but edit the commit message
- edit = use commit, but stop for amending
- squash = use commit, but meld into previous commit
- fixup = like "squash", but discard this commit's log message
- exec = run command (the rest of the line) using shell
- drop = remove commit
> **____________________________**
- git rebase --edit-todo (use to change the actions with commits during the rebasing)

- git rebase -i @~3 (allows us to rewrite the history in the current branch) VERY useful!!

20. Fix the commit in the middle of the branch (autosquash)

git config --global rebase.autoSquash true

- git commit --fixup=<`bad-commit-code`>
- git rebase -i @~n-steps 

21. Commits discarding (revert)

> **Note:** It discards the changes made in specific commit(reverts them) and creates another commit without those changes. U need to be on the branch, where u want to revert the commit, to use this commit
If the error occured - do the same things when solving merge problems

- git revert <`commit-code`>
- git revert <`commit-code`>...<`commit-code-2`>(to select the range of commits, to revert the changes)

22. Merging discard (revert)

> **Attention:** if u are using that command, then in future u'll have problems with merging the branches, in which u've done the revert of 'merging' commit

- git revert <`commit-code`> -m 1(/2) (to revert the 'merging' commit of the first/second parent)

To avoid the merge problem, use this:

- first way: 1) git cherry-pick <`merging-commit`> -m 1(parent num)
- second way(more preferable): 2) git revert <`revert-commit-code`> (revert of the discarding commit)

Only after picking one of those ways u can do the branches merging

23. Repeat merging with 'git rebase'

> **Attention:** it will only rebase the commits, which main branch doesn't have as its 
ancestors, to avoid it use 'git rebase --onto'

To avoid repeating conflicts use 'rerere'(auto repeat-conflicts resolving): git config rerere.enabled true

- git rebase <`branch-to-rebase-to`> <`branch-to-rebase`>
- git rebase --onto master 1234/<`first-commit-of-rebasing-branch`> (make full branch rebase without restrictions)

Great way to delete the connection with 'merging' commit - make the rebase to the same place

- git rebase <`first-commit-in-branch`> --no-ff

### Additional Git Info

1. Tags in Git

Allows us to use tag instead of commit-code

- git tag <`tag-name`> <`commit-code`>
- git tag v1.0.0 1234 (Call example: git show --quiet v1.0.0) or (creation of branch with tag: git branch v1.x v1.0.0)
- git tag (shows all the tags)
- git tag --contains <`commit-code`> (shows if there's any tag on the commit)
- git tag -n (tags with messages)
- git tag -n -l 'v1.1*' (tags with mask)
- git tag -d <`tag-name`> (delete the tag)
- git tag -a -m 'version 1.0.0' v1.0.0 <`commit-code`> (tag with anotation, shows who did the tag)

2. Describe & archive

- git describe (shows info about commit with tag or its heir)
- git archive -o /tmp/<`file-name`>.zip HEAD (saves the archive of last commit in specific directory)

3. 'Modern' diff

> **Note:** For prog. languages do the next things ---> .gitattributes ---> *.html dif=html(or any other)

- git diff --word-diff (shows which words were changed)
- git diff --color-words (the same)

4. ReReRe (automatic solving of the same conflicts) (REuse REcorded REsolution)

> **Note:** It can be used when canceling rebase/merge with conflicts. When solve is recorded, rerere doesnt add it to index automatically,
you need to do it on your own

- git config rerere.enabled true (to activate that mode)
- git config rerere.autoUpdate true (to add solved files to index automatically)
- git rerere forget <`path`> (to abbandon old solving of the conflict, you need to use 'git checkout -merge <`path`>' before that)
- git commit -a --no-edit (to continue the merging)
- rm -rf .git/rr-cache (to forget all solvings)
- <`path`>/rerere-train.sh --all (to enable the script of auto detecting the previous conflict solving)
- <`path`>/rerere-train.sh feature@{n-steps} (to do it manually for specific commit)

5. Garbage collection

> **Main Rule:** if top commit is still alive - you can return the branch

- git gc (to init the garbage collection)
- git fsck --unreachable (to look for unreachable commits)
- git filter-branch (to filter the commits or redo them at all) OR use "BFG" (outer utility)
- git reflog expire --expire=now --all (to clean all reflogs)
- git gc --prune=now (to clean all unreachable commits)

6. Editor settings

- git config --global --edit (to open the config file in the editor)
- git config --global core.editor '<`path-to-editor`>' (check the net for different editors)
- git config --global core.autocrlf (true/input/false) (solves the problem of stroke ending) (only to text files)
