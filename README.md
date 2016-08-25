git_undo
========

create an alias for "git undo"  (as described by Enrico Campidoglio [http://megakemp.com/2016/08/25/git-undo/])


```shell 
git config --global alias.undo '!f() { \
  git reset --hard $(git rev-parse --abbrev-ref HEAD)@{${1-1}}; \
; f'
``` 
 

Explanation
=============

!f() { ... } f
--------------

Here, we’re defining the alias as a shell function named f which is then invoked immediately.

$(git rev-parse --abbrev-ref HEAD)@{...}
----------------------------


We use the git rev-parse command followed by the --abbrev-ref option to get the name of the current branch reference, which we then concatenate with @{...} to form the reference to a previous position in the reflog (e.g. master@{1}).

${1-1}
------

We specify the position in the reflog as the first parameter $1 with a default value of 1. This is the whole reason why we defined the alias as a shell function: to be able to provide a default value for the parameter using the standard Bash syntax.

The beauty of using an optional parameter like this, is that it allows us to undo any number of operations. At the same time, if we don’t specify anything, it’s going to undo the just latest one.

