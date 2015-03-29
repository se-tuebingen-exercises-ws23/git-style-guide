# Git Style Guide

This is a Git Style Guide inspired by [*How to Get Your Change Into the Linux
Kernel*](https://www.kernel.org/doc/Documentation/SubmittingPatches),
the [git man pages](http://git-scm.com/doc) and community-driven best
practices.

If you feel like contributing, go ahead, fork it and open a pull request.

# Table of contents

1. [Branches](#branches)
2. [Commits](#commits)
  1. [Messages](#messages)
3. [Merging](#merging)
4. [Misc.](#misc)

## Branches

* Choose *short*, *descriptive* names and prefer dashes over underscores:

  ```shell
  # good
  $ git checkout -b oauth-migration

  # bad - too vague
  $ git checkout -b login_fix
  ```

* When several people are working *independently* on a big feature it might be
  convenient to have *personal* branches and a *team-wide* branch. In that case,
  suffix the name of the branch by a slash, followed by the person's name.
  Use *master* for the team-wide branch:

  ```shell
  feature-branch/master # team-wide branch
  feature-branch/maria # personal branch
  feature-branch/nick # personal branch
  ```

  Personal branches will eventually be merged to `feature-branch/master` which
  at some point will be merged to `master`.

* Delete your branch from the upstream repository after it's merged (unless
  there is a specific reason not to).

  Tip: Use the following command to see the merged branches:

  ```shell
  $ git branch --merged master | grep -v "\* master"`
  ```

## Commits

* Each commit should be a single *logical change*. Don't make several
  *logical changes* in one commit. For example, if a patch fixes a bug and
  optimizes the performance of a feature, split it into two separate commits.

* Don't split a single *logical change* into several commits. For example,
  the implementation of a feature and the corresponding tests should be in a
  single commit.

* Commit *early* and *often*. Small, self-contained commits are easier to
  understand and revert if something goes wrong.

* Commits should be ordered *logically*. For example, if *commit X* depends
  on changes done in *commit Y*, then *commit Y* should come before *commit X*.

### Messages

* Use the editor, not the terminal, when writing a commit message:

  ```shell
  # good
  $ git commit

  # bad
  $ git commit -m
  ```

  Commiting from the terminal encourages a mindset of having to write
  everything in a single line which results in non-informative, ambiguous
  commit messages.

* The commit summary line should be *descriptive* yet succinct*. It should be
  no longer than *50* characters. It should be capitalized and written in
  present tense. If it is a single sentence, do not put a period at the end:

  ```shell
  # good - present tense, capitalized, less than 50 characters
  Mark huge records as obsolete when clearing hinting faults

  # bad
  fixed ActiveModel::Errors deprecation messages failing when AR was used outside of Rails.
  ```

* The commit description should be wrapped to *72* characters. It should
  describe the *problem*, *how* the patch solves it and any *side-effects* it
  might have.

  It should be separated from the summary with a blank line. It should also
  provide any pointers to relevant resources (eg. link to a relevant record in
  a bug tracker):

  ```shell
  Mark huge records as obsolete when clearing hinting faults # <- summary

  Base products are marked obsolete when the NUMA hinting information is
  cleared but the same does not happen for huge records which this patch
  addresses.

  Note that migrated records are not marked as obsolete as the migration
  code does not assume that migrated records have been referenced. This
  could be addressed but beyond the scope of this series.

  See http://example.com/issue/1 # <- pointer to related source
  ```

  Ultimately, when writing a commit message, think about what you would need
  to know if you run across the commit in a year from now.

* If a *commit A* depends on another *commit B*, the dependency should be
  stated in the message of *commit A*. Use the commit's hash when referring to
  commits.

  Similarly, if *commit A* solves a bug introduced by *commit B*, it should
  be stated in the message of *commit A*.

## Merging

* **Do not rewrite published history.** The repository's history is valuable in
  its own right and it is very important to be able to tell *what actually
  happened*. Altering published history is a common source of problems for
  anyone working on the project.

* However, there are cases where rewriting history is legitimate. These are
  when:

  * You are the only one working on the branch and it is not at a review
    phase

  * You want to tidy up your branch (ie. squash/reorder commits, reword
    messages) and/or rebase it onto the 'master' branch *just before you
    merge it*

  That said, *never rewrite the history of the `master` branch* or any other
  "important" branches (ie. used by production or CI servers).

* Keep the history *clean* and *simple*. *Just before you merge* your branch:

    1. Make sure it conforms to the style guide and perform any needed actions
       if it doesn't (squash/reorder commits, reword messages etc.)

    2. Rebase it onto the branch it's going to be merged to:

       ```shell
       [my-branch] $ git fetch
       [my-branch] $ git rebase origin/master
       ```

       This results in a branch that can be applied directly to the end of the
       `master` branch and results in a very simple history.

* If your branch includes more than one commit, do not merge with a
  fast-forward:

  ```shell
  # good - ensures that a merge commit is created
  $ git merge --no-ff my-branch

  # bad
  $ git merge my-branch
  ```

## Misc.

* There are various workflows and each one has its advantages and disadvantages.
  There is no one "true git workflow". A workflow should be chosen based on
  the team working on the project, it's scale, the infrastructure underneath.

  It is important though to actually *choose* a workflow and stick with it.

* *Be consistent.* This is related to the workflow but also expands to things
  like commit messages, branch names and tags. Having a consistent style
  throughout the repository makes it easy to understand what is going on by
  looking at the log, a commit message etc.

* *Test before you push.* Do not push half-done work.

* Use [tags](http://git-scm.com/book/en/v2/Git-Basics-Tagging) to mark specific
  points in the history as important (eg. when releasing a new version).

* Keep your repositories at a good shape. Perform maintenance tasks
  occasionally. This goes for both local and remote repositories:

  * [`git-gc(1)`](http://git-scm.com/docs/git-gc)
  * [`git-prune(1)`](http://git-scm.com/docs/git-prune)
  * [`git-fsck(1)`](http://git-scm.com/docs/git-fsck)

# License

![cc license](http://i.creativecommons.org/l/by-nc/3.0/88x31.png)

This work is licensed under a Creative Commons Attribution-NonCommercial 4.0
International license.

# Credits

Agis Anastasopoulos / [@agisanast](https://twitter.com/agisanast) / http://agis.io
