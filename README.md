# Apache Metron Committer Tools

## Prepare Commit

This script automates the process of merging a pull request into `apache/master`.  The script will prompt for the pull request number.  Most of the remaining information is extracted from Github or JIRA.

In the following example, I enter the pull request number (`80`) when prompted.   Using the pull request number, the script can extract most of the remaining required information.

When prompted the `[value in brackets]` will be used by default.  To accept the default, simply press `enter`.  If you would like to change the default, type it in and hit `enter` when done.

(1) Execute the script and enter the pull request number when prompted.

```
$ ./prepare-commit
  pull request: 80
  your github username [nickwallen]:
  origin repo [http://github.com/apache/metron]:
  github contributor's username [nickwallen]:
  github contributor's email [nick@nickallen.org]:
  github contributor's branch [METRON-111]:
  issue identifier in jira [METRON-111]:
  issue description ['Start Metron UI' Fails When Already Started]:
  commit message [METRON-111 'Start Metron UI' fails when already started (nickwallen) closes apache/metron#80]:

Merge 'nickwallen/METRON-111' and use 'PR#80' to resolve 'METRON-111'? [N/y] y
```

(2) Review the summary and enter `y` at the prompt, if you are satisfied.  This will begin the process of preparing the commit.

```
----> clone origin and fetch upstream <----
Cloning into 'metron'...
remote: Counting objects: 7021, done.
remote: Compressing objects: 100% (52/52), done.
remote: Total 7021 (delta 12), reused 0 (delta 0), pack-reused 6957
Receiving objects: 100% (7021/7021), 39.69 MiB | 9.18 MiB/s, done.
Resolving deltas: 100% (2472/2472), done.
Checking connectivity... done.
From https://git-wip-us.apache.org/repos/asf/metron
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> upstream/master

----> merge upstream with origin <----
Already on 'master'
Your branch is up-to-date with 'origin/master'.
Already up-to-date.

----> ready to commit <----
remote: Counting objects: 7, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 7 (delta 1), reused 1 (delta 1), pack-reused 2
Unpacking objects: 100% (7/7), done.
From http://github.com/nickwallen/metron
 * branch            METRON-111 -> FETCH_HEAD
Squash commit -- not updating HEAD
Automatic merge went well; stopped before committing as requested
[master 8c901cb] METRON-111 'Start Metron UI' fails when already started (nickwallen) closes apache/metron#80
 Author: nickwallen <nick@nickallen.org>
 1 file changed, 1 insertion(+), 1 deletion(-)

 ----> commit complete <----
```

(3) After the merge is complete, the script will output a summary of the changes that were made and also the last two log messages.

```
  deployment/roles/metron_ui/tasks/main.yml | 2 +-
  1 file changed, 1 insertion(+), 1 deletion(-)

 commit 8c901cb3455ecb4623ea75b89679bbe26d7d7062
 Author: nickwallen <nick@nickallen.org>
 Date:   Mon Apr 18 15:11:51 2016 -0400

     METRON-111 'Start Metron UI' fails when already started (nickwallen) closes apache/metron#80

 commit 3bc804fa227ff3152cbe5454736d8e84bc7329ce
 Author: nickwallen <nick@nickallen.org>
 Date:   Fri Apr 15 15:29:38 2016 -0400

     METRON-83 Create sensor test mode (nickwallen) closes apache/metron#58

 Review commit carefully then run...
     cd /Users/nallen/tmp/metron
     git push upstream master
```

(4) To this point nothing has been committed or merged to `apache/master`.  If you are happy with the state of this local repo, then simply follow the instructions to push the changes to Apache.  If you are not happy, simply start over.  Nothing changes in Apache Land until you run the following command.

```
cd /Users/nallen/tmp/metron
git push upstream master
```

## Checkout PR

The `checkout-pr` script will checkout the branch for the specified pull request.  This makes it easy to checkout the code for a pull request and manually execute a test or validate the code.

```
$ ./checkout-pr 80
Cloning into 'metron-pr-80'...
remote: Counting objects: 7028, done.
remote: Compressing objects: 100% (56/56), done.
remote: Total 7028 (delta 15), reused 0 (delta 0), pack-reused 6960
Receiving objects: 100% (7028/7028), 39.69 MiB | 9.14 MiB/s, done.
Resolving deltas: 100% (2476/2476), done.
Checking connectivity... done.
remote: Counting objects: 4, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (4/4), done.
From http://github.com/apache/metron
 * [new ref]         refs/pull/80/head -> pr-80
Switched to branch 'pr-80'

$ cd metron-pr-80/

$ git branch
  master
* pr-80
```

If you execute the command within an existing repository, then the repository will not be cloned again.

```
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean

$ ../checkout-pr 81
remote: Counting objects: 77, done.
remote: Compressing objects: 100% (51/51), done.
remote: Total 77 (delta 10), reused 2 (delta 2), pack-reused 0
Unpacking objects: 100% (77/77), done.
From http://github.com/apache/metron
 * [new ref]         refs/pull/81/head -> pr-81
Switched to branch 'pr-81'

$ git status
On branch pr-81
nothing to commit, working directory clean
```
