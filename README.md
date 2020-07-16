# 7RewritingHistoryRysGitTutorial

7 Rewriting History Rys Git Tutorial

# 7. [Rewriting History](https://github.com/c4arl0s/RysGitTutorial#7-rewriting-history)
 * [Create the Red Page](https://github.com/c4arl0s/RysGitTutorial#-create-the-red-page)
 * [Create the Yellow Page](https://github.com/c4arl0s/RysGitTutorial#-create-the-yellow-page)
 * [Link and Commit the New Pages](https://github.com/c4arl0s/RysGitTutorial#-link-and-commit-the-new-pages)
 * [Create and commit the Green Page](https://github.com/c4arl0s/RysGitTutorial#-create-and-commit-the-green-page)
 * [Begin an Interactive Rebase](https://github.com/c4arl0s/RysGitTutorial#-begin-an-interactive-rebase)
 * [Undo the Generic Commit](https://github.com/c4arl0s/RysGitTutorial#-undo-the-generic-commit)
 * [Split the Generic Commit](https://github.com/c4arl0s/RysGitTutorial#-split-the-generic-commit)
 * [Remove the last Commit](https://github.com/c4arl0s/RysGitTutorial#-remove-the-last-commit)
 * [Open the Reflog](https://github.com/c4arl0s/RysGitTutorial#-open-the-reflog)
 * [Revive the Lost Commit](https://github.com/c4arl0s/RysGitTutorial#-revive-the-lost-commit)
 * [Filter the Log History](https://github.com/c4arl0s/RysGitTutorial#-filter-the-log-history)
 * [Merge in the Revived Branch](https://github.com/c4arl0s/RysGitTutorial#-merge-in-the-revived-branch)
 * [Conclusion](https://github.com/c4arl0s/RysGitTutorial#-conclusion-5)
 * [Quick Reference](https://github.com/c4arl0s/RysGitTutorial#-quick-reference-3)

# 7. [Rewriting History](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

The previuous module on rebasing taught us how to move commits around and perform some basic edits while doing so, but now we are going to really get our hands dirty We Will learn **how to split up commits, revive lost snapshots, and completely rewrite a repository's history to our exact specifications**.

Hopefully, this module will get you much more comfortable with the core Git components, as we will be inspecting and editing the internal makeup of our project.

# 	* [Create the Red Page](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

First, let's create a new branch and add a few more HTML pages.

```console
$ git checkout -b new-pages
Switched to a new branch 'new-pages'
```

```console
$ git branch
  master
```

Notice that we created a new branch and check it out in a single step by passing the -b flag to the **git checkout** command.

Next, create the file **red.html** and add the following content:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <title>The Red Page</title>
  <link rel="stylesheet" href="style.css" />
  <meta charset="utf-8" />
</head>
<body>
  <h1 style="color: #C00">The Red Page</h1>
  <p>Red is the color of <span style="color: #C00">passion</span>!</p>
    
  <p><a href="index.html">Return to home page</a></p>
</body>
</html>
```

We will hold off on committing this page for the moment.

# 	* [Create the Yellow Page](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

Create a file called **yellow.html**, which should look like the following.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <title>The Yellow Page</title>
  <link rel="stylesheet" href="style.css" />
  <meta charset="utf-8" />
</head>
<body>
  <h1 style="color: #FF0">The Yellow Page</h1>
  <p>Yellow is the color of <span style="color: #FF0">the sun</span>!</p>
    
  <p><a href="index.html">Return to home page</a></p>
</body>
</html>
```

# 	* [Link and Commit the New Pages](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

Next, We will link both new pages to the home page. Add the following items to the "Navigation" section in index.html

```html
<li style="color: #C00">
  <a href="red.html">The Red Page</a>
</li>
<li style="color: #FF0">
  <a href="yellow.html">The Yellow Page</a>
</li>
```

Then commit all of these changes in a single snapshot.

```console
$ git add red.html yellow.html index.html 
```

```console
$ git status
On branch new-pages
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   index.html
	new file:   red.html
	new file:   yellow.html
```

```console
$ git commit -m "add new HTML Pages"
[new-pages 4a5bfc9] add new HTML Pages
 3 files changed, 35 insertions(+), 1 deletion(-)
 create mode 100644 red.html
 create mode 100644 yellow.html
```

This is an example of a **bad commit**. It perform multiple unrelated tasks, and it has a relatively generic commit message. Thus far, we have not really specified when ti is appropriate to commit changes, but the general rules are essentially the same as for branch creation:

1. Commit a snapshot for each significant addition to your project.
2. Don't commit a snapshot if you cannot come up with a single, specific message for it.

This will ensure that your project has a meaningful commit history, which gives you the ability to see exactly when and where a feature was added or a piece of functionality was broken. However, in practice, you will often wind up committing several changes in a single snapshot, since you will not always know what constitutes a **"well-defined"** addition as you are developing a project. Fortunately, Git, lets us go back and fix up these problem commits after the fact.

# 	* [Create and commit the Green Page](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

Let's create one more page before splitting that "bad" commit: Add the following HTML to a file called **green.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <title>The Green Page</title>
  <link rel="stylesheet" href="style.css" />
  <meta charset="utf-8" />
</head>
<body>
  <h1 style="color: #0C0">The Green Page</h1>
  <p><span style="color: #0C0">Green</span> is the color of earth.</p>
    
  <p><a href="index.html">Return to home page</a></p>
</body>
</html>
```

Add a link to index.html in the "Navigation" section:

```html
<li style="color: #0C0">
  <a href="green.html">The Green Page</a>
</li>
```

And finally, stage the green page and commit the snapshot.

```console
$ git add green.html index.html 
```

```console
$ git status
On branch new-pages
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   green.html
	modified:   index.html
```

```console
$ git commit -m "Add green page"
[new-pages 49fd8bf] Add green page
 2 files changed, 17 insertions(+)
 create mode 100644 green.html
```

# 	* [Begin an Interactive Rebase](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

The commits introduced in our new-pages branch are:

```console
49fd8bf (HEAD -> new-pages) Add green page
4a5bfc9 add new HTML Pages
```

But, we want these commits to look more like:

```console
49fd8bf Add green page
XXXXXXX Add yellow page
XXXXXXX Add red page
```

To achieve this, we can use the same interactive rebasing method covered in the previous module, only this time we will actually **create** commits in the middle of the rebasing procedure.

```console
git rebase -i master
```

Change the rebase listing to the following, then save the file and exit the editor to begin the rebase.

```console
edit 4a5bfc9 add new HTML Pages
pick 49fd8bf Add green page
```

# 	* [Undo the Generic Commit](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

First, let's take a look at where we are with **git log --oneline**:

```console
4a5bfc9 (HEAD) add new HTML Pages
20b9d5d (master) Add link to about section in home page
```

When Git encountered the **edit** command in the rebase configuration, it stopped to let us edit the commit. As a result, the green page commit does not appear in our history yet. This should be familiar from the previous module. But instead of **amending** the current commit, we are going to completely remove it:

```console
$ git reset --mixed HEAD~1
Unstaged changes after reset:
M	index.html
```

The **git reset** command moves the checked out snapshot to a new commit, and the **HEAD~1** parameter tells it **to reset** to the commit that occurs immediately before the current HEAD (likewise, **HEAD~2** would refer to **second commit before HEAD**). In this particular case, **HEAD~1** happens to coincide with **master**. The effect on our repository can be visualized as:

![Screen Shot 2020-05-30 at 13 36 49](https://user-images.githubusercontent.com/24994818/83336663-d0bd5400-a27a-11ea-9bd6-28b787c51e19.png)

You may recall from [Undoing Changes]() that we used **git reset --hard** to undo uncommitted changes to our project. The **--hard** flag told Git to make the working directory look exactly like the most recent commit, giving us the intended effect of removing uncommitted changes.

But, this time we use the **--mixed** flag to preserve the working directory, which contains the changes we want to separate. That is to say, the **HEAD** moved, but the working directory  remained unchanged. Of course, this results in a repository with uncommitted modifications. We now have the opportunity to add the **red.html** and **yellow.html** files to distinct commits.

# 	* [Split the Generic Commit](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

Let's start with the red page. Since we only want to commit content that involves the red page, we will have to manually go in and remove the yellow page's link from the "Navigation" section. In index.html, change this section to match the following.

```html
<h2>Navigation</h2>
<ul>
  <li>
    <a href="about/index.html">About Us</a>
  </li>
  <li style="color: #F90">
    <a href="orange.html">The Orange Page</a>
  </li>
  <li style="color: #00F">
    <a href="blue.html">The Blue Page</a>
  </li>
  <li>
    <a href="rainbow.html">The Rainbow Page</a>
  </li>
  <li style="color: #C00">
    <a href="red.html">The Red Page</a>
  </li>
</ul>
``` 

Now, we can group the red page' updates into an independent commit.

```console
git add red.html index.html
```

```console
$ git status
interactive rebase in progress; onto 20b9d5d
Last command done (1 command done):
   edit 4a5bfc9 add new HTML Pages
Next command to do (1 remaining command):
   pick 49fd8bf Add green page
  (use "git rebase --edit-todo" to view and edit)
You are currently editing a commit while rebasing branch 'new-pages' on '20b9d5d'.
  (use "git commit --amend" to amend the current commit)
  (use "git rebase --continue" once you are satisfied with your changes)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   index.html
	new file:   red.html

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	yellow.html
```

```console
$ git commit -m "Add red page"
[detached HEAD cb8d72b] Add red page
 2 files changed, 18 insertions(+), 1 deletion(-)
 create mode 100644 red.html
```

Next up is the yellow page. Go ahead and add it back to the "Navigation" section in index.html

```console
$ vim index.html 
```

```html
<li style="color: #FF0">
  <a href="yellow.html">The Yellow Page</a>
</li>
```

And again, stage and commit the snapshot

```console
$ git add yellow.html index.html 
```

```console
interactive rebase in progress; onto 20b9d5d
Last command done (1 command done):
   edit 4a5bfc9 add new HTML Pages
Next command to do (1 remaining command):
   pick 49fd8bf Add green page
  (use "git rebase --edit-todo" to view and edit)
You are currently editing a commit while rebasing branch 'new-pages' on '20b9d5d'.
  (use "git commit --amend" to amend the current commit)
  (use "git rebase --continue" once you are satisfied with your changes)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   index.html
	new file:   yellow.html
```

```console
$ git commit -m "Add yellow page"
[detached HEAD 1b2f067] Add yellow page
 2 files changed, 17 insertions(+)
 create mode 100644 yellow.html
```

We have successfully split up the contents of a single commit into two new snapshots, as shown below.

![Screen Shot 2020-05-30 at 17 47 36](https://user-images.githubusercontent.com/24994818/83340445-acbf3a00-a29d-11ea-90c6-c8615da2042f.png)

But, do not forget that the rebase still needs to transfer the green page:

```console
$ git rebase --continue
Successfully rebased and updated refs/heads/new-pages.
```

To summarize, we removed the **"bad"** commit from the current branch with **git reset**, keeping the contained HTML files intact with the **--mixed** flag. Then, we committed them in separate snapshots with the usual **git add** and **git commit** commands. The point to remember is that during a rebase you can **add**, **delete**, and **edit** commits to your heart's content, and the entire result will be moved to the new base.

let's show

```console
$ git log --oneline
d417237 (HEAD -> new-pages) Add green page
1b2f067 Add yellow page
cb8d72b Add red page
20b9d5d (master) Add link to about section in home page
71153c2 Begin creating bio pages (added message to mary)
f4bb8c3 Create the about page
74afd90 Add article for 2nd news item
5142364 Add 2nd news item to index page
f79223d Merge branch 'crazy'
ebb4171 Add news item for rainbow
049c9d9 Add 1st news item
4310454 Link index.html to rainbow.html
6a43f42 add CSS stylesheet to rainbow.html
b9f2b14 Merge branch 'master' into crazy
1a27d0e link HTML pages to stylesheet
019e981 Add CSS stylesheet
95a36a7 Rename crazy.html to rainbow.html
e1bc771 add a rainbow to crazy.html
3553479 Revert "Add a crazzy experiment"
12e24f0 Add a crazzy experiment
453c8a4 (tag: v1.0) Add navigation links
1047951 t Add blue an orange html files
6a442fc Create index page for the message
```

# 	* [Remove the last Commit](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

Next, we are going to **"accidentally"** remove the green page commit so we can learn how to retrieve it via Git's **internal repository data**.

```console
$ git log --oneline
d417237 Add green page
1b2f067 Add yellow page
cb8d72b Add red page
20b9d5d Add link to about section in home page
71153c2 Begin creating bio pages (added message to mary)
f4bb8c3 Create the about page
74afd90 Add article for 2nd news item
5142364 Add 2nd news item to index page
f79223d Merge branch 'crazy'
ebb4171 Add news item for rainbow
049c9d9 Add 1st news item
4310454 Link index.html to rainbow.html
6a43f42 add CSS stylesheet to rainbow.html
b9f2b14 Merge branch 'master' into crazy
1a27d0e link HTML pages to stylesheet
019e981 Add CSS stylesheet
95a36a7 Rename crazy.html to rainbow.html
e1bc771 add a rainbow to crazy.html
3553479 Revert "Add a crazzy experiment"
12e24f0 Add a crazzy experiment
453c8a4 Add navigation links
1047951 t Add blue an orange html files
6a442fc Create index page for the message
```

```console
$ git reset --hard HEAD~
HEAD is now at 1b2f067 Add yellow page
```

```console
$ git status
On branch new-pages
nothing to commit, working tree clean
```

```console
$ git log --oneline
1b2f067 (HEAD -> new-pages) Add yellow page
cb8d72b Add red page
20b9d5d (master) Add link to about section in home page
71153c2 Begin creating bio pages (added message to mary)
f4bb8c3 Create the about page
74afd90 Add article for 2nd news item
5142364 Add 2nd news item to index page
f79223d Merge branch 'crazy'
ebb4171 Add news item for rainbow
049c9d9 Add 1st news item
4310454 Link index.html to rainbow.html
6a43f42 add CSS stylesheet to rainbow.html
b9f2b14 Merge branch 'master' into crazy
1a27d0e link HTML pages to stylesheet
019e981 Add CSS stylesheet
95a36a7 Rename crazy.html to rainbow.html
e1bc771 add a rainbow to crazy.html
3553479 Revert "Add a crazzy experiment"
12e24f0 Add a crazzy experiment
453c8a4 (tag: v1.0) Add navigation links
1047951 t Add blue an orange html files
6a442fc Create index page for the message
```

This moves the checked-out commit backward by one snapshot, along with the **new-pages** pointer. Note that **git status** tells us that we have nothing to commit, since the **--hard** flag obliterated (remove) any changes in the working directory. And of course, the **git log** output shows that the **new-pages** branch no longer contains the green commit.

This behavior is slightly different from the reset we used in the interactive rebase: this time the branch: **this time the branch moved with the new HEAD**. Since we were on (no branch) during the **rebase**, there was no branch tip to move. However, in general, **git reset** is used to move branch tips around and optionally alter the working directory via one of its many flags. (e.g. **--mixed** or **--hard**).

![Screen Shot 2020-05-31 at 18 57 25](https://user-images.githubusercontent.com/24994818/83365813-9afa9700-a370-11ea-8592-0ecb23015250.png)

The commit that we removed from the branch is now a **dangling commit**. Dangling commits are those that cannot be reached from any branch and are thus in danger of being lost forever.


# 	* [Open the Reflog](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

Git uses something called the **reflog** to record every change you make to your repository. Let's take a look at what it contains:

```console
$ git reflog
1b2f067 HEAD@{0}: reset: moving to HEAD~
d417237 HEAD@{1}: rebase -i (finish): returning to refs/heads/new-pages
d417237 HEAD@{2}: rebase -i (pick): Add green page
1b2f067 HEAD@{3}: commit: Add yellow page
cb8d72b HEAD@{4}: commit: Add red page
20b9d5d HEAD@{5}: reset: moving to HEAD~1
4a5bfc9 HEAD@{6}: rebase -i: fast-forward
20b9d5d HEAD@{7}: rebase -i (start): checkout master
49fd8bf HEAD@{8}: commit: Add green page
4a5bfc9 HEAD@{9}: commit: add new HTML Pages
20b9d5d HEAD@{10}: checkout: moving from master to new-pages
20b9d5d HEAD@{11}: merge about: Fast-forward
74afd90 HEAD@{12}: checkout: moving from about to master
20b9d5d HEAD@{13}: rebase -i (finish): returning to refs/heads/about
20b9d5d HEAD@{14}: rebase -i (pick): Add link to about section in home page
71153c2 HEAD@{15}: commit (amend): Begin creating bio pages (added message to mary)
c5c692e HEAD@{16}: rebase -i: fast-forward
f4bb8c3 HEAD@{17}: rebase -i (start): checkout master
def4be5 HEAD@{18}: rebase -i (finish): returning to refs/heads/about
def4be5 HEAD@{19}: rebase -i (pick): Add link to about section in home page
c5c692e HEAD@{20}: rebase -i (reword): Begin creating bio pages
c0ae323 HEAD@{21}: rebase -i (reword): Add HTML page for personal bio
f4bb8c3 HEAD@{22}: rebase -i (reword): Create the about page
a6d6855 HEAD@{23}: rebase -i: fast-forward
74afd90 HEAD@{24}: rebase -i (start): checkout master
7d4c122 HEAD@{25}: rebase -i (finish): returning to refs/heads/about
7d4c122 HEAD@{26}: rebase -i (pick): Add link to about section in home page
5a3bbf7 HEAD@{27}: rebase -i (reword): Add HTML page for personal bio
a6d6855 HEAD@{28}: rebase -i (reword): Add empty page in about section
97a4a22 HEAD@{29}: rebase -i: fast-forward
74afd90 HEAD@{30}: rebase -i (start): checkout master
a72282b HEAD@{31}: rebase -i (finish): returning to refs/heads/about
a72282b HEAD@{32}: rebase -i (start): checkout master
a72282b HEAD@{33}: checkout: moving from master to about
74afd90 HEAD@{34}: checkout: moving from about to master
a72282b HEAD@{35}: rebase -i (finish): returning to refs/heads/about
a72282b HEAD@{36}: rebase -i (start): checkout master
a72282b HEAD@{37}: reset: moving to HEAD
a72282b HEAD@{38}: rebase -i (finish): returning to refs/heads/about
a72282b HEAD@{39}: rebase -i (start): checkout master
a72282b HEAD@{40}: checkout: moving from master to about
74afd90 HEAD@{41}: checkout: moving from about to master
a72282b HEAD@{42}: rebase -i (finish): returning to refs/heads/about
a72282b HEAD@{43}: rebase -i (pick): Add link to about section in home page
1260437 HEAD@{44}: rebase -i (squash): Add HTML page for personal bio
8de18d7 HEAD@{45}: rebase -i (pick): Add HTML page for personal bio
97a4a22 HEAD@{46}: rebase -i (squash): Add empty page in about section
edff70e HEAD@{47}: rebase -i (start): checkout master
ce9d652 HEAD@{48}: checkout: moving from master to about
74afd90 HEAD@{49}: checkout: moving from about to master
ce9d652 HEAD@{50}: rebase -i (finish): returning to refs/heads/about
ce9d652 HEAD@{51}: rebase -i (start): checkout master
ce9d652 HEAD@{52}: rebase -i (finish): returning to refs/heads/about
ce9d652 HEAD@{53}: rebase -i (start): checkout master
ce9d652 HEAD@{54}: commit: Add link to about section in home page
5f022e1 HEAD@{55}: commit: Add empty HTML page for Mary's bio
fbe3d49 HEAD@{56}: commit: Add HTML page for personal bio
ebfbefc HEAD@{57}: rebase finished: returning to refs/heads/about
ebfbefc HEAD@{58}: rebase: Add contents to about page
edff70e HEAD@{59}: rebase: Add empty page in about section
74afd90 HEAD@{60}: rebase: checkout master
c94fb42 HEAD@{61}: checkout: moving from master to about
74afd90 HEAD@{62}: merge news-hotfix: Fast-forward
f79223d HEAD@{63}: checkout: moving from news-hotfix to master
74afd90 HEAD@{64}: commit: Add article for 2nd news item
5142364 HEAD@{65}: commit: Add 2nd news item to index page
f79223d HEAD@{66}: checkout: moving from master to news-hotfix
f79223d HEAD@{67}: checkout: moving from about to master
c94fb42 HEAD@{68}: commit: Add contents to about page
27b0924 HEAD@{69}: commit: Add empty page in about section
f79223d HEAD@{70}: checkout: moving from master to about
f79223d HEAD@{71}: commit (merge): Merge branch 'crazy'
049c9d9 HEAD@{72}: checkout: moving from crazy to master
ebb4171 HEAD@{73}: commit: Add news item for rainbow
4310454 HEAD@{74}: checkout: moving from master to crazy
049c9d9 HEAD@{75}: merge news-hotfix: Fast-forward
1a27d0e HEAD@{76}: checkout: moving from news-hotfix to master
049c9d9 HEAD@{77}: commit: Add 1st news item
1a27d0e HEAD@{78}: checkout: moving from master to news-hotfix
1a27d0e HEAD@{79}: checkout: moving from crazy-alt to master
d247771 HEAD@{80}: commit: Make a REAL rainbow
4310454 HEAD@{81}: checkout: moving from crazy to crazy-alt
4310454 HEAD@{82}: commit: Link index.html to rainbow.html
6a43f42 HEAD@{83}: commit: add CSS stylesheet to rainbow.html
b9f2b14 HEAD@{84}: merge master: Merge made by the 'recursive' strategy.
95a36a7 HEAD@{85}: checkout: moving from master to crazy
1a27d0e HEAD@{86}: merge css: Fast-forward
3553479 HEAD@{87}: checkout: moving from css to master
1a27d0e HEAD@{88}: commit: link HTML pages to stylesheet
019e981 HEAD@{89}: commit: Add CSS stylesheet
3553479 HEAD@{90}: checkout: moving from master to css
3553479 HEAD@{91}: checkout: moving from crazy to master
95a36a7 HEAD@{92}: commit: Rename crazy.html to rainbow.html
e1bc771 HEAD@{93}: reset: moving to HEAD
e1bc771 HEAD@{94}: commit: add a rainbow to crazy.html
12e24f0 HEAD@{95}: checkout: moving from 12e24f0c4e03b3c991b287230548d8bdad3882d7 to crazy
12e24f0 HEAD@{96}: checkout: moving from master to 12e24f0
3553479 HEAD@{97}: reset: moving to HEAD
3553479 HEAD@{98}: revert: Revert "Add a crazzy experiment"
12e24f0 HEAD@{99}: checkout: moving from 453c8a4db079c3d235e3470754a79a22ea0f0afd to master
453c8a4 HEAD@{100}: checkout: moving from master to v1.0
12e24f0 HEAD@{101}: commit: Add a crazzy experiment
453c8a4 HEAD@{102}: checkout: moving from 6a442fcc4ab51362713f09ed5eadc7af767db833 to master
6a442fc HEAD@{103}: checkout: moving from 1047951bab2636a3bc90682e48d9fb32644da036 to 6a442fc
1047951 HEAD@{104}: checkout: moving from master to 1047951
453c8a4 HEAD@{105}: commit: Add navigation links
1047951 HEAD@{106}: commit: t Add blue an orange html files
6a442fc HEAD@{107}: commit (initial): Create index page for the message
```

The resulting output should look something like the following. Depending on your version of Git, the message might be slightly different. You can press space to scroll through the content or q to exit.

The above listing reflects our last few actions. For example, the current HEAD, denoted by HEAD@{0}, resulted from reseting HEAD to HEAD~1. Four actions ago, the yellow page was applied during our rebase, as shown in HEAD@{3}.

The **reflog** is a **chronological** listing of our history, without regard for the repository's branch structure. This lets us find **dangling commits** that would otherwise be lost from the project history.

# 	* [Revive the Lost Commit](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

At the beginning of each **reflog** entry, you will find a commit ID representing, the HEAD~ after that action. Check out the commit at **HEAD@{2}**, which should be where the rebase added the green page (change the ID below to the ID from your reflog)

```console
$ git checkout d417237
Note: switching to 'd417237'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at d417237 Add green page
```

This puts us in a **detached HEAD state**, which means our HEAD is no longer on the tip of a branch. We are actually in the opposite situation as we were in [Undoing Changes]() when we checked our a commit **before** the branch tip. Now, we are looking at a commit **after** the tip of the branch, but we still have a detached **HEAD**:

![Screen Shot 2020-06-01 at 17 49 41](https://user-images.githubusercontent.com/24994818/83462250-58988f00-a430-11ea-8d1d-c476cb41dcf4.png)

To turn our dangling commit into a full-fledged branch, all we have to do is create one.

```console
$ git checkout -b green-page
Switched to a new branch 'green-page'
```

We now have a branch that can be merged back into the project:

![Screen Shot 2020-06-01 at 17 53 28](https://user-images.githubusercontent.com/24994818/83462430-d197e680-a430-11ea-815e-202f989519fc.png)

The above diagram makes it easy to see that the **green-page** branch is a extension of **new-pages**, but how would we figure this out if we weren't drawing out the state of our repository every step of the way ?

# 	* [Filter the Log History](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

To view the differences between branches, we can use Git's log-filtering syntax.

```console
$ git log new-pages..green-page
commit d41723777cff192f4b4453fe6115a997e3aeb1a5 (HEAD -> green-page)
Author: c4arl0s <c.santiago.cruz@gmail.com>
Date:   Sat May 30 13:19:55 2020 -0500

    Add green page
```

This will display all commits contained in **green-page** that are not in the **new-pages** branch. The above command tells us the **green-page** contains one more snapshot than **new-pages**: our dangling commit (although, it is not really dangling any more since we created a branch for it).

You can also use this syntax to limit the output of **git log**. For example, to display the ast 4 commits on the current branch, you could use.

```console
$ git log HEAD~4..HEAD
commit d41723777cff192f4b4453fe6115a997e3aeb1a5 (HEAD -> green-page)
Author: c4arl0s <c.santiago.cruz@gmail.com>
Date:   Sat May 30 13:19:55 2020 -0500

    Add green page

commit 1b2f067a2b67f40fc8a50b1d21d49469b3ff50d2 (new-pages)
Author: c4arl0s <c.santiago.cruz@gmail.com>
Date:   Sat May 30 17:46:40 2020 -0500

    Add yellow page

commit cb8d72bc066d0a4f60110a9cbd7351421cfe463c
Author: c4arl0s <c.santiago.cruz@gmail.com>
Date:   Sat May 30 17:43:00 2020 -0500

    Add red page

commit 20b9d5d6d3bdb09d440f9067bc9b3bbdfa308446 (master)
Author: c4arl0s <c.santiago.cruz@gmail.com>
Date:   Tue May 26 18:50:56 2020 -0500

    Add link to about section in home page
```

However, this is a bit verbose for such a common task, so Git developers added the -n flag as an easier way to limit output.

```console
$ git log -n 4
commit d41723777cff192f4b4453fe6115a997e3aeb1a5 (HEAD -> green-page)
Author: c4arl0s <c.santiago.cruz@gmail.com>
Date:   Sat May 30 13:19:55 2020 -0500

    Add green page

commit 1b2f067a2b67f40fc8a50b1d21d49469b3ff50d2 (new-pages)
Author: c4arl0s <c.santiago.cruz@gmail.com>
Date:   Sat May 30 17:46:40 2020 -0500

    Add yellow page

commit cb8d72bc066d0a4f60110a9cbd7351421cfe463c
Author: c4arl0s <c.santiago.cruz@gmail.com>
Date:   Sat May 30 17:43:00 2020 -0500

    Add red page

commit 20b9d5d6d3bdb09d440f9067bc9b3bbdfa308446 (master)
Author: c4arl0s <c.santiago.cruz@gmail.com>
Date:   Tue May 26 18:50:56 2020 -0500

    Add link to about section in home page
```

The -n 4 parameter tells Git to show only the last four commits from the current HEAD, making it the equivalent of the HEAD~4..HEAD syntax shown above. Similarly, -n 3, -n 2, and -n 1 would display three, two, and one commit, respectively. This feature becomes very useful once a repository grows beyond one screenful of history.

# 	* [Merge in the Revived Branch](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

We have revived our lost commit, and now we are ready to merge everything back into the **master** branch. Before we do the merge, let's see exactly what we are merging:

```console
$ git checkout master
Switched to branch 'master'
```

```console
$ git log HEAD..green-page --stat
commit d41723777cff192f4b4453fe6115a997e3aeb1a5
Author: c4arl0s <c.santiago.cruz@gmail.com>
Date:   Sat May 30 13:19:55 2020 -0500

    Add green page

 green.html | 14 ++++++++++++++
 index.html |  3 +++
 2 files changed, 17 insertions(+)

commit 1b2f067a2b67f40fc8a50b1d21d49469b3ff50d2
Author: c4arl0s <c.santiago.cruz@gmail.com>
Date:   Sat May 30 17:46:40 2020 -0500

    Add yellow page

 index.html  |  3 +++
 yellow.html | 14 ++++++++++++++
 2 files changed, 17 insertions(+)

commit cb8d72bc066d0a4f60110a9cbd7351421cfe463c
Author: c4arl0s <c.santiago.cruz@gmail.com>
Date:   Sat May 30 17:43:00 2020 -0500

    Add red page

 index.html |  5 ++++-
 red.html   | 14 ++++++++++++++
 2 files changed, 18 insertions(+), 1 deletion(-)
```

The **$ git log HEAD..green-page** command shows us only those commits in **green-page** that are not in **master** (since master is the current branch, we can refer to it as HEAD). The new **--stat** flag includes information about which files have been changed in each commit. For example, the most recent commit tells us that 14 lines were added to the **green.html** file and 3 lines were added to **index.html**:

```c
commit d41723777cff192f4b4453fe6115a997e3aeb1a5
Author: c4arl0s <c.santiago.cruz@gmail.com>
Date:   Sat May 30 13:19:55 2020 -0500

    Add green page

 green.html | 14 ++++++++++++++
 index.html |  3 +++
 2 files changed, 17 insertions(+)
```

If we didn't already know what was in this new commit, the log output would tell us which files we needed to look at. But, we authored all these changes, so we can skip right to the merge.

```console
$ git merge green-page
Updating 20b9d5d..d417237
Fast-forward
 green.html  | 14 ++++++++++++++
 index.html  | 11 ++++++++++-
 red.html    | 14 ++++++++++++++
 yellow.html | 14 ++++++++++++++
 4 files changed, 52 insertions(+), 1 deletion(-)
 create mode 100644 green.html
 create mode 100644 red.html
 create mode 100644 yellow.html
```

The following diagram shows us the state our repository after the merge.

![Screen Shot 2020-06-02 at 13 33 12](https://user-images.githubusercontent.com/24994818/83556525-af0bd900-a4d5-11ea-9407-23d61d12ff52.png)

```console
d417237 Add green page
1b2f067 Add yellow page
cb8d72b Add red page
20b9d5d Add link to about section in home page
71153c2 Begin creating bio pages (added message to mary)
f4bb8c3 Create the about page
74afd90 Add article for 2nd news item
5142364 Add 2nd news item to index page
f79223d Merge branch 'crazy'
ebb4171 Add news item for rainbow
049c9d9 Add 1st news item
4310454 Link index.html to rainbow.html
6a43f42 add CSS stylesheet to rainbow.html
b9f2b14 Merge branch 'master' into crazy
1a27d0e link HTML pages to stylesheet
019e981 Add CSS stylesheet
95a36a7 Rename crazy.html to rainbow.html
e1bc771 add a rainbow to crazy.html
3553479 Revert "Add a crazzy experiment"
12e24f0 Add a crazzy experiment
453c8a4 Add navigation links
1047951 t Add blue an orange html files
6a442fc Create index page for the message
```

Note that the green-page branch already contains all the history of **new-pages**, which is why we merged the former instead of the latter. It this was not the case, Git would complain when we try to run the following command:

```console
$ git branch -d new-pages
Deleted branch new-pages (was 1b2f067).
```

We can go ahead and delete the green 

```console
$ git branch -d green-page
Deleted branch green-page (was d417237).
```

# 	* [Conclusion](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

This module took an in-depth look at rebasing, resetting, and the reflog. We learned how to split old commits into one or more new commits, and how to revive "lost" commits. Hopefully, this has given you a better understanding of the interaction between the working directory, the stage, branches, and commited snapshots. We also explored some new options for displaying our commit history, which will become an important skill as our project grows.

WE did a lot of work with the branch tips this module. It is important to realize that Git uses the tip of a branch to represent **entire branch**. That is to say, a branch is actually a pointer to a single commit- not a containter for a series of commits. This idea has been implicitly reflected in our diagrams:

![Screen Shot 2020-06-02 at 13 45 55](https://user-images.githubusercontent.com/24994818/83557547-740aa500-a4d7-11ea-8458-145596a4191d.png)

The history is represented by the parent of each commit (designated by arrows), not the branch itself. So, to request a new branch, all Git has to do is create a reference to the current commit. And, to add a snapshot to a branch, it just has to move the branch reference to the new commit. An understanding of Git's branch representation should make it easier to wrap your head around merging, rebasing, and other kinds of branch manipulation.

We will revisit Git's internal representation of a repository in the final module of his tutorial. But now, we are finally ready to discuss multi-user development, which happens to rely entirely on Git branches.

# 	* [Quick Reference](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

```console
$ git reflog
```
```console
$ git reset --mixed HEAD~n
```
Move the HEAD backward n commits, but do not change the working directory

```console
$ git reset --hard HEAD~n
```
Move the HEAD backward n commits, and change the working directory to match.


```console
$ git log since..until
```
Display the commits reachable from until but not from since. These parameters can be either commit ID's o branch names.


```console
$ git log --stat
```
Include extra information about altered files in the log output.

# 8. [Remotes](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

Simply put, a **remote repository*** is one that is not your own. It can be another Git repository that is on your company's network, the internet, or even your local file system, but the point is that it is a repository distinct from your my-git-repository project.

We have already seen how branches can streamline a workflow within a single repository, but they also happen to be Git's mechanism for sharing commits between repositories. **Remote Brances** act just like the local branches that we have been using, only the represent a branch in someone else's repository.

![Screen Shot 2020-06-03 at 12 30 45](https://user-images.githubusercontent.com/24994818/83668761-1721f400-a596-11ea-9f23-26b12b25960e.png)

This means we can adapt our merging and rebasing skills to make Git fantastic collaboration tool. Over the next few modules, we will be exploring various multi-user workflows by pretending to be different developers woking on our example website.

For several parts of this module, we are going to pretend to be Mary, the graphic designer for our website. Mary's actions are clearly denoted by including her name in the heading of each step.

# 	* [Clone the Repository (Mary)](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

First, Mary needs her own copy of the repository to work with. The **Distributed Workflows** module will discuss network-based remotes, but right new we are just going to store them on the local filesystem.

```console
cd /Users/carlossantiagocruz/iOS/RysGitTutorialRepository
```

```console
cd ..
```

- Before we continue, I have to go back to fix the following command to pass the --local flag to do the correct cloning.

```console
$ git clone --local RysGitTutorialRepository/ RysGitTutorialMarysRepository
Cloning into 'RysGitTutorialMarysRepository'...
done.
```

```console
$ cd RysGitTutorialMarysRepository/
Wed Jun 03 ~/iOS/RysGitTutorialMarysRepository 
$ ls
about        green.html   news-1.html  orange.html  red.html     yellow.html
blue.html    index.html   news-2.html  rainbow.html style.css
```

The first two lines navigate the command shell to the directory **above** RysGitTutorialMarysRepository. Make sure to change to the actual path to your repository. The **git clone** command copies our repository into RysGitTutorialMarysRepository, which will reside in the same directory as my git repository. Then, we navigate to the Mary's repository so we can start pretending to be Mary.

First show all directories involved

```console
$ ls RysGitTutorial*
RysGitTutorialRepository:
blue.html    orange.html  style.css    news-1.html  rainbow.html news-2.html  about        green.html   index.html   red.html     yellow.html

RysGitTutorial:
LICENSE   README.md

RysGitTutorialMarysRepository:
about        blue.html    green.html   index.html   news-1.html  news-2.html  orange.html  rainbow.html red.html     style.css    yellow.html
```

Run **git log** verify that Mary's repository is in fact a copy of our original repository.

```console
$ git log --oneline
d417237 (HEAD -> master, origin/master, origin/HEAD) Add green page
1b2f067 Add yellow page
cb8d72b Add red page
20b9d5d Add link to about section in home page
71153c2 Begin creating bio pages (added message to mary)
f4bb8c3 Create the about page
74afd90 Add article for 2nd news item
5142364 Add 2nd news item to index page
f79223d Merge branch 'crazy'
ebb4171 Add news item for rainbow
049c9d9 Add 1st news item
4310454 Link index.html to rainbow.html
6a43f42 add CSS stylesheet to rainbow.html
b9f2b14 Merge branch 'master' into crazy
1a27d0e link HTML pages to stylesheet
019e981 Add CSS stylesheet
95a36a7 Rename crazy.html to rainbow.html
e1bc771 add a rainbow to crazy.html
3553479 Revert "Add a crazzy experiment"
12e24f0 Add a crazzy experiment
453c8a4 (tag: v1.0) Add navigation links
1047951 t Add blue an orange html files
6a442fc Create index page for the message
```
# 	* [Configure The Repository (Mary)](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

First off, Mary needs to configure her repository so that we know who contributed what to the project.

```console
$ git config user.name "Mary"
```

```console
$ git config user.mail mary.example@icloud.com
```

You may recall from the first module that we used a **--global** flag to set the configuration for the entire Git installation. But since Mary's repository is on the local **filesystem**, she needs a **local** configuration.

```console
$ ls .git
info        hooks       packed-refs HEAD        index
description objects     refs        logs        config
```

```console
$ cat .git/config 
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
	ignorecase = true
	precomposeunicode = true
[remote "origin"]
	url = /Users/carlossantiagocruz/iOS/RysGitTutorialRepository/
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
	remote = origin
	merge = refs/heads/master
[user]
	name = Mary
	mail = mary.example@icloud.com
```

Use a text editor to open up the file called **config** in the **.git** directory of Mary's project (you may  need to enable hidden files to see .git). This is where local configurations are stored, and we see Mary's information at the bottom of the file. Note that this overrides the global configuration that we set in **The basics**

# 	* [Start Mary's Day (Mary)](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

Today, Mary is going to be working on her bio page, which she should develop in a separate branch:

```console
git checkout -b bio-page
```

Mary can create and check out branches just like we did in our copy of the project. Her repository is a completely isolated development enviroment, and she can do whatever she wants in her without worrying about what's going on in my git repository. Just as branches are an abstraction for the working directory, the staged snapshot, and a commit history, a repository is an abstraction for branches.

# 	* [Create Mary's Bio Page](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

Let's complete Mary's biography page. In mary's repository, change about/mary.html to:

```console
<html lang="en">
<head>
  <title>About Mary</title>
  <link rel="stylesheet" href="../style.css" />
  <meta charset="utf-8" />
</head>
<body>
  <h1>About Mary</h1>
  <p>I'm a graphic designer.</p>

  <h2>Interests</h2>
  <ul>
    <li>Oil Painting</li>
    <li>Web Design</li>
  </ul>

  <p><a href="index.html">Return to about page</a></p>
</body>
</html>
```

Again, we are developing this in a branch as a best-practice step: our **master** branch is only for stable, tested code. Stage and commit the snapshot, then take a look at the result.

```console
$ git commit -a -m "Add bio page for mary"
[bio-pages 0d746d0] Add bio page for mary
 1 file changed, 21 insertions(+), 1 deletion(-)
```

```console
Author: Mary <c.santiago.cruz@gmail.com>
Date:   Fri Jun 5 11:09:51 2020 -0500

    Add bio page for mary
```

The **Author** field in the log output should reflect the local configurations we made for Mary's name and email. Remember that -n -1 flags limits history output to a single commit.

```console
$ git log -n 1
commit 0d746d0a6eb7b983e661e47f9347b196b2967a0d (HEAD -> bio-pages)
Author: Mary <c.santiago.cruz@gmail.com>
Date:   Fri Jun 5 11:09:51 2020 -0500

    Add bio page for mary
```

[Check why the email was not changed]()

# 	* [Publish the Bio Page (Mary)](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

Now, we can publish the bio page by merging into the **master** branch:

```console
$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
```

Then try to merge

Oh Crap !!!

```console
merge: bio-page - not something we can merge
```

I suppose the repository was not created correctly, reading more documentation it suggest to pass the --local flag when you cloning.

So do it again, and configure.

```console
$ git clone --local RysGitTutorialRepository/ RysGitTutorialMarysRepository
```

the configure:

```console
$ git config user.name "Mary"
```

```console
$ git config user.mail mary@repository.com
```

Then merge again


```console
$ git merge bio-page
Updating d417237..49baa6e
Fast-forward
 about/mary.html | 21 ++++++++++++++++++++-
 1 file changed, 20 insertions(+), 1 deletion(-)
```
Woala ! Now it works !!!

Of course, this results in a fast-forward merge. 

![Screen Shot 2020-06-05 at 11 43 05](https://user-images.githubusercontent.com/24994818/83902300-be7f6200-a721-11ea-880d-fb7689c54d45.png)

Notice that both repositories have normal, local branches - we have not had any interaction between the two repositories, so we don't see any remote branches yet. Before we switch back to my git repository, let's examine Mary's remote connections.

# 	* [View Remote Repositories (Mary)](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

Mary can list the connections she has to other repositories using the following command.

```console
Fri Jun 05 ~/iOS/RysGitTutorialMarysRepository 
$ git remote
origin
```

Apparently, she has a remote called **origin**. When you clone a repository, Git automatically adds an **origin**. When you clone a repository, Git automatically adds an **origin** remote pointing to the original repository, under the assumption that you will probably want to interact with it down to the road. We can request a little bit more information with -v (verbose) flag:

```cosole
Fri Jun 05 ~/iOS/RysGitTutorialMarysRepository 
$ git remote -v
origin	/Users/carlossantiagocruz/iOS/RysGitTutorialRepository/ (fetch)
origin	/Users/carlossantiagocruz/iOS/RysGitTutorialRepository/ (push)
```

This shows the full path to our original repository, verifying that **origin** is a remote connection to my git repository. The same path is designated as a **"fetch"** and a **"push"** location. We will see what these means in a moment.

# 	* [Return to Your Repository (you)](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

OK, we are done being Mary, and we can return to our own repository:

```console
$ cd ../RysGitTutorialRepository/
```

Notice that Mar's bio page is still empty. It is very important to understand that this repository and Mary's repository are completely separate. While she was altering her bio page, we could have been doing all sorts of other things in my git repository. We could have even changed her bio page, which would result in a merge conflict when we try to pull her changes in.

# 	* [Add Mary as a Remote (you)](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

Before we can get ahold of Mar's bio page, we need access to her repository. Let's look at our current list of remotes:

```console
$ git remote
```

We don't have any (**origin** was never created because we didn't clone from anywhere). So, let's add Mary as a remote repository

```console
$ git remote add mary ../RysGitTutorialMarysRepository/
```

```console
Fri Jun 05 ~/iOS/RysGitTutorialRepository 
$ git remote -v
mary	../RysGitTutorialMarysRepository/ (fetch)
mary	../RysGitTutorialMarysRepository/ (push)
```

We can now use **mary** to refer to Mar's repository, which is located at ../RysGitTutorialMarysRepository. The **git remote add** command is used to bookmark another Git repository for easy access, and our connections can be seen in the figure below.

![Screen Shot 2020-06-05 at 12 14 05](https://user-images.githubusercontent.com/24994818/83904777-1cae4400-a726-11ea-992e-9a47fb3bf53e.png)

Now that our remote **repositories are setup, we will spend the rest of the module discussing remote **branches**

# 	* [Fetch Mary's Branches (you)](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

As noted earlier, we can use remote branches to access snapshots from another repository. Let's take a look at our current remote branches with the - flag.]()

```console
$ git branch -r
```

Again, we don't have any. To populate our remote branch listing, we need to **fetch** the branches from Mary's repository:

```console
$ git fetch mary 
remote: Enumerating objects: 7, done.
remote: Counting objects: 100% (7/7), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 4 (delta 1), reused 0 (delta 0)
Unpacking objects: 100% (4/4), 604 bytes | 100.00 KiB/s, done.
From ../RysGitTutorialMarysRepository
 * [new branch]      bio-page   -> mary/bio-page
 * [new branch]      master     -> mary/master
```

```console
Fri Jun 05 ~/iOS/RysGitTutorialRepository 
$ git branch -r
  mary/bio-page
  mary/master
```

This will go to the **"fetch"** location shown in **git remote -v** and download all of the branches it finds there into our repository. The resulting branches are shown below.

```console
  mary/bio-page
  mary/master
```

Remote branches are always listed in the form remoteName/branchName so that they will never be mistaken for local branches. The above listing reflects the sate of Mar's repository at the time of the fetch, but they will not be automatically updated if Mary continues developing any of her branches.

That is to say, our remote branches are not **direct** links into Mary's repository -they are read-only copies of her branches, stored in our own repository. This means that we would have to do another fetch to access new updates.

![Screen Shot 2020-06-05 at 12 25 09](https://user-images.githubusercontent.com/24994818/83905628-a1e62880-a727-11ea-944a-77e60c9a6399.png)

The above figure shows the state of **our** repository. We have access to Mary's snapshot (represented by sqeares) and her branches, even though we don't have a real-time connection to Mary's repository.

# 	* [Check out a Remote Branch](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

Let's check out a remote branch to review Mary's changes.

```console
Fri Jun 05 ~/iOS/RysGitTutorialRepository 
$ git checkout mary/master
Note: switching to 'mary/master'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 49baa6e Add bio page for Mary
```

This puts us in a **detached** HEAD state, just like we were in when we checked out a dangling commit. This should not be that surprising, considering that our remote branches are **copies** of Mary's branches. Checking out a remote branch takes our HEAD off the tip of a local branch, illustrated by the following diagram.

![Screen Shot 2020-06-05 at 12 39 29](https://user-images.githubusercontent.com/24994818/83906672-a01d6480-a729-11ea-83ee-c616558f6d26.png)

We can't continue developing if we are not on a local branch. To build on **mary/master** we either need to merge it into our own local **master** or create another branch. We did the latter in Branches, Part I to build on an old commit and in the previous module to revive a "lost commit, but right now we are just looking at what Mary did, so the **detached** HEAD state does not really affect us.

# 	* [Find Mary's Changes](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

We can use the same log-filtering syntax from the previous module to view Mary's changes:

```console
Fri Jun 05 ~/iOS/RysGitTutorialRepository 
$ git log master..mary/master --stat
commit 49baa6e81a7ad87714540ec9ce9fceaaa12409c1 (HEAD, mary/master, mary/bio-page)
Author: Mary <c.santiago.cruz@gmail.com>
Date:   Fri Jun 5 11:34:45 2020 -0500

    Add bio page for Mary

 about/mary.html | 21 ++++++++++++++++++++-
 1 file changed, 20 insertions(+), 1 deletion(-)
```

This shows us what Mary has added to her master branch, but it is also a good idea to see if we have added any new changes that are not in Mary's repository

```console
Fri Jun 05 ~/iOS/RysGitTutorialRepository 
$ git log mary/master..master --stat
Fri Jun 05 ~/iOS/RysGitTutorialRepository 
$ 
```

This will not output anything, since we haven't altered our data-base since Mary cloned it, In other words, our history hasn't **diverged** - **we are just behind by a commit**.

# 	* [Merge Mary's Changes](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

Let's approve Mary's changes and integrate them into our own **master** branch.

```console
Sat Jun 06 ~/iOS/RysGitTutorialRepository 
$ git merge mary/master
Updating d417237..49baa6e
Fast-forward
 about/mary.html | 21 ++++++++++++++++++++-
 1 file changed, 20 insertions(+), 1 deletion(-)
Sat Jun 06 ~/iOS/RysGitTutorialRepository 
```

Even though **mary/master** is a remote branch, this still results in a **fast-forward** merge because there is a linear path from our **master** to the tip of **mary/master**

![Screen Shot 2020-06-06 at 12 44 47](https://user-images.githubusercontent.com/24994818/83950893-86e0ea80-a7f3-11ea-8a1a-aa3b15bfe9ee.png)

After the merge, the snapshots from Mary's remote branch become a part of a our local **master** branch. As a result, our **master** is now synchronized with Mary's:

![Screen Shot 2020-06-06 at 12 46 36](https://user-images.githubusercontent.com/24994818/83950922-c6a7d200-a7f3-11ea-9969-4f0a5bb9d842.png)

Notice that we only interacted with Mary's **master** branch, even though we had access to her **bio-page**. If we hadn't been pretending to be Mary, we wouldn't have known what this feature branch was for it or if it was ready to be merged. But, since we've designated **master** as a stable branch for the project, it was safe to integrate those updates (assuming Mary was also aware of this convention).

# 	* [Push a Dummy Branch](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

To complement our **git fetch** command, we will take a brief look at **pushing**. Fetching and pushing are **almost** opposites, in that fetching imports branches, while pushing exports branches to another repository. Let's take a look:

```console
$ git branch dummy
```

This creates a new branch called **dummy**.

```console
$ git push mary dummy
Total 0 (delta 0), reused 0 (delta 0)
To ../RysGitTutorialMarysRepository/
 * [new branch]      dummy -> dummy
```

This sends it to Mary. Switch into Mary's repository to see what we did:

```console
Mon Jun 08 ~/iOS/RysGitTutorialRepository 
$ cd ../RysGitTutorialMarysRepository/
```

```console
$ git branch
  bio-page
  dummy
* master
```

You should find a new **dummy** branch in her **local** branch listing. I said that **git fetch** and **git push** are **almost** opposites because **pushing creates a new local branch**, while **fetching imports commits into remote branches**.

Now put yourself in Mary's shoes. She was a developing in her own repository when, all of a sudden, a new **dummy** branch appeared out of nowhere. Obviously, pushing branches into other people's repositories can make for a chaotic workflow. So, as a general rule, **you should never push into another developer's repository. But then, why does **git push** even exist?

Over the next few modules, we will see that pushing is a necessary tool for maintaining public repositories. Until then, just remember to never, ever push into one of your friends's repositories. Let's get rid of these dummy branches and return to our repository.

```console
$ git branch -d dummy
Deleted branch dummy (was 49baa6e).
```

Go back to your repository

```console
Mon Jun 08 ~/iOS/RysGitTutorialMarysRepository 
$ cd ../RysGitTutorialRepository/
```

the delete the branch

```console
$ git branch -d dummy
Deleted branch dummy (was 49baa6e).
```

# 	* [Push a New Tag](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

An important property of **git push** is that it does not automatically push tag associated with a particular branch. Let's examine this by creating a new tag.

```cosole
Wed Jun 10 ~/iOS/RysGitTutorialRepository 
$ git tag -a v2.0 -m "An even stabler version of the website"
```

We now have a v2.0 tag in my git repository, which we can see by running the **git tag** command. Now, let's try pushing the branch to Mary's repository.

```
$ git push mary master
Everything up-to-date
```

Git will say her **master** branch is already up-to-date, and her repository will remain unchanged. Instead of pushing the branch that contains the tag. Git requires us to manually push the tag itself:

```console
$ git push mary v2.0
Enumerating objects: 1, done.
Counting objects: 100% (1/1), done.
Writing objects: 100% (1/1), 180 bytes | 180.00 KiB/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To ../RysGitTutorialMarysRepository/
 * [new tag]         v2.0 -> v2.0
```

You should now be able to see the v2.0 tag in Mary's repository with a quick **git tag**. It is very easy to forget to push new tags, so if it seems like your project has lost a tag or two, it is most likely because you didn't to push them to the remote repository.

```console
Wed Jun 10 ~/iOS/RysGitTutorialRepository 
$ cd ../RysGitTutorialMarysRepository/
Wed Jun 10 ~/iOS/RysGitTutorialMarysRepository 
$ git tag
v1.0
v2.0
```

# 	* [Conclusion](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

In this module, we learned how remote branches can be used to access content in someone else's repository. The remotes listed in **git remote** are merely bookmarks for a full path to another repository. We used a local path, but as we will soon see, Git can use the SSH protocol to access repositories on another computer.

The convention of **master** as a stable branch allowed us to pull changes without consulting Mary, but this doesn't necessarily have to be the case. When implementing your own workflow, Gif offers you a lot of flexibility about when and where you should pull from your team members. As long as you clearly define your project conventions, you can designate a special uses or privileges to **any** branch.

That said, it is important to note that remotes are for **people**, whereas branches are for **topics**. Do **not** create separate branches for each of your developers -give them separate repositories and bookmark them with **git remote add**. Branches should always be for project development, not user management.

Now that we know how Git shares information between repositories, we can add some more structure to our multi-user development environment. The next module will show you how to set up and access a shared central repository.

# 	* [Quick Reference](https://github.com/c4arl0s/RysGitTutorial#rysgittutorial)

```console
$ git -- clone remothePath
```
Create a copy of a remote Git repository

```console
$ git remote
```
List remote repositories.


```console
$ git remote add remoteName remotePath
```
Add a remote repository


```console
$ git fetch remoteName
```
Download remote branch information, but do not merge anything

```console
$ git merge remoteName/branchName
```
Merge a remote branch into the checked-out branch

```console
git branch -r
```
List remote branches

```console
git push remoteName branchName
```
Push a local branch to another repository

```console
$ git branch -r
```
List remote branches.

```console
$ git push remoteName branchName
```
Push a local branch to another repository

```consol
git push remoteName tagName
```
Push a tag to another repository


