sync-git-from-svn
=================

Perl script that git-adds svn-tracked files to git.

I use this script to hold all my svn checkouts needed for some project in one local git repository, and hold svn checkouts in sync (please inform me if git-svn can do this for multiple svn repositories).

Here is an example usage (please note: this is my first git usage, so not everything might be idiomatic):

```bash
$ # Make git repository.
$ mkdir gitrepo && cd gitrepo
$ git init
$ 
$ # Checkout svn repositories.
$ svn checkout http://svn.repo.url1/foo/trunk dir/foo
$ svn checkout http://svn.repo.url2/bar/trunk bar
$ 
$ # Add contents of svn repositories to git.
$ echo dir/foo >> .svndirs
$ echo bar >> .svndirs
$ sync-git-from-svn --all # or sync-git-from-svn dir/foo && sync-git-from-svn bar
$ git commit -m "Synchronized with svn."
$ 
$ # Work on a feature.
$ git checkout -b feature
$ # Implement your feature and commit to feature branch.
$ ...
$ 
$ # Commit your feature, but in the mean time, new svn revisions were 
$ # added to remote svn repositories.
$ # Synchronize master branch first.
$ git checkout master
$ svn update dir/foo
$ svn update bar
$ sync-git-from-svn --all
$ git commit -m "Synchronized with svn."
$ 
$ # Merge changes to feature branch.
$ git checkout feature
$ git rebase master
$ 
$ # Merge feature branch to master.
$ git checkout master
$ git merge feature
$ 
$ # Commit your feature to svn repository. Added files for feature are
$ # not tracked for svn, so you have to add them first.
$ svn add dir/foo/file1 dir/foo/file2 bar/file3 ...
$ svn commit -m 'Implemented feature' dir/foo
$ svn commit -m 'Implemented feature' bar
```