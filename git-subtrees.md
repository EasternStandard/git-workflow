# Git Subtrees

In some situations it becomes necessary to operate two separate repositories:

* Contractors may not be granted access to a customer's Acquia/Pantheon repo but are still required to make code changes.
* Developers need to commit changes to a repository but are limited to creating PRs on a customer's repo.
* A platform or multisite-based site maintains separate dependency repositories that must be merged into the upstream or primary repository.

In these cases, and others, Eastern Standard uses a "Git subtree" workflow.

# Subtree Architecture

Using Git subtrees requires two separate repositories:

* The primary repository is the repository upon which the site is based.
* The secondary repository is any repository that contributes to the primary repository.

When viewed with a Git GUI client, the primary repository contains a history similar to the following:

![](/assets/acquia-subtree-merge.png)

When the secondary repository is added as a new remote, the component commits become obvious:

![](/assets/acquia-subtree-merge-details.png)

# Subtree Workflow

In a subtree workflow, code change commits are rarely, if ever, made to the primary repository, to ensure that everyone working in the secondary repository has all of the latest code available. Instead, code change commits are made to the secondary repository, using whatever feature branch workflow is required for that repository; and changes to be sent to the primary repository are collected in a single branch for inclusion in the primary repository.

> #### info::Note
>
> One of the most common ways this is done on Acquia is to create a secondary repository containing the contents of the `docroot` folder of an Acquia repo \(the primary repository\). Commits are made to the secondary repository like normal, and are then pulled into the primary Acquia repository via git subtree.

The primary repository is updated via a single command run from the root of the checked-out primary repository:

```
git subtree pull --prefix </path/to/destination/folder> <secondary repository Git URL> <secondary repository branch name> --squash -m "<commit message>"
```

The following items should be changed:

* `<path/to/destination/folder>`: This is the path, relative to the root of the project, where the secondary repository should be placed. The contents of the secondary repository will be created inside this folder.

  > #### info::Note
  >
  > If it helps to think of it this way, this part of the command works exactly like `git clone <remote URL> <destination folder>` does when it places the contents of the `<remote URL>` inside a newly-created `<destination folder>`.

* `<secondary repository Git URL>`: This is the URL you would use to clone the secondary repostory, eg. `git@github.com/org/project.git`.
* `<secondary repository branch name>`: This is the branch on the secondary repository to import into the primary repository.
* `<commit message>`: This is the commit message that should appear. If the `-m <commit message>` part of the command is omitted, the default message \(`Merge commit '<hash>'`\) will be used instead.