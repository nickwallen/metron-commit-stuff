# Apache Metron Committer Tools

## Prepare Commit

This script automates the process of preparing a pull request to be merged into `apache/master`.  The script will ask for the pull request number and most of the remaining information is extracted from Github or Apache JIRA automatically.

In the following example, I enter the pull request number (`80`) when prompted.   From the pull request the script can extract most of the remaining required information.

When prompted the `[value in brackets]` will be used if you simply press enter at the prompt.  If you would like to change the default value, simply type it in and hit enter when done.

```
$ cd metron-commit-stuff
$ ./prepare-commit
  pull request: 80
  your github username [nickwallen]:
  origin repo [http://github.com/apache/incubator-metron]:
  github contributor's username [nickwallen]:
  github contributor's email [nick@nickallen.org]:
  github contributor's branch [METRON-111]:
  issue identifier in jira [METRON-111]:
  issue description ['Start Metron UI' Fails When Already Started]:
  commit message [METRON-111 'Start Metron UI' fails when already started (nickwallen) closes apache/incubator-metron#80]:

Merge 'nickwallen/METRON-111' and use 'PR#80' to resolve 'METRON-111'? [N/y] y
```

After reviewing the information, simply enter `y` at the prompt.  This will begin the process of preparing the commit.

```
----> clone origin and fetch upstream <----
Cloning into 'incubator-metron'...
remote: Counting objects: 7021, done.
remote: Compressing objects: 100% (52/52), done.
remote: Total 7021 (delta 12), reused 0 (delta 0), pack-reused 6957
Receiving objects: 100% (7021/7021), 39.69 MiB | 9.18 MiB/s, done.
Resolving deltas: 100% (2472/2472), done.
Checking connectivity... done.
From https://git-wip-us.apache.org/repos/asf/incubator-metron
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
From http://github.com/nickwallen/incubator-metron
 * branch            METRON-111 -> FETCH_HEAD
Squash commit -- not updating HEAD
Automatic merge went well; stopped before committing as requested
[master 8c901cb] METRON-111 'Start Metron UI' fails when already started (nickwallen) closes apache/incubator-metron#80
 Author: nickwallen <nick@nickallen.org>
 1 file changed, 1 insertion(+), 1 deletion(-)

 ----> commit complete <----
```

Finally the script will output a summary of the changes that were made and also the last two log messages.

```
  deployment/roles/metron_ui/tasks/main.yml | 2 +-
  1 file changed, 1 insertion(+), 1 deletion(-)

 commit 8c901cb3455ecb4623ea75b89679bbe26d7d7062
 Author: nickwallen <nick@nickallen.org>
 Date:   Mon Apr 18 15:11:51 2016 -0400

     METRON-111 'Start Metron UI' fails when already started (nickwallen) closes apache/incubator-metron#80

 commit 3bc804fa227ff3152cbe5454736d8e84bc7329ce
 Author: nickwallen <nick@nickallen.org>
 Date:   Fri Apr 15 15:29:38 2016 -0400

     METRON-83 Create sensor test mode (nickwallen) closes apache/incubator-metron#58

 Review commit carefully then run...
     cd /Users/nallen/tmp/incubator-metron
     git push upstream master
```

To this point nothing has been committed or merged to `apache/master`.  If you are happy with the result, then simply follow the instructions.  If you are not happy, simply re-run the script.

```
cd /Users/nallen/tmp/incubator-metron
git push upstream master
```
