# Pantheon Introduction

Pantheon provides a robust three-tier environment that is tied directly to the status of the `master` branch of the git repo provisioned when the project is created. Additionally, up to 10 multidev environments can be created based off of any of the main three environments. These environments exist as separate branches of the repository, but they have their own separate files and database storage. Pantheon provides one-click ways to move the code, files, and database between environments.

# Technical Information: How does Pantheon power three or more environments from the same repo?

One of the more confusing aspects for developers first starting out in Pantheon is that the three environments made available: `dev`, `test`, and `live`, all run off the `master` branch in git. Traditionally, when setting up a three-tier environment manually, one might create a "dev" environment based off the `dev` branch of the repo; a "test" environment based off the `test` or `stage` branch; and a "prod" environment based off of `master`. In the Pantheon world, however, everything runs off of `master`.

## The Dev Environment

The simplest of the three environments is the "dev" environment:

![](/assets/pantheon-dev.png)

The dev branch simply reflects the `HEAD` commit of the `master` branch at all times. When code is pushed to the repo on that branch, within seconds the Pantheon dev environment will reflect those changes.

## The Test and Live Environments

The "test" and "live" environments both work the same way. When a Pantheon project is created, the only environment available is the "dev" environment. Clicking on either "Test" or "Live" in the dashboard will show something like this:

![](/assets/pantheon-unitialized-test.png)

Once the "test" environment has been initialized, a tag will be created on the `master` branch at the current `HEAD` commit, using the format `pantheon_test_nn`. The same is true for the "live" environment: a new tag will be created on the `master` branch at the current `HEAD` commit, using the format `pantheon_live_nn`.

![](/assets/pantheon-tag-example.png)

The "test" and "live" environments are frozen at the highest tag ending in `nn`. It's important to note that this is a numeric sorting, not a string sorting, so while `2` sorts higher than `20` when you're looking at an alphabetical list of tags, Pantheon will load the tag ending in `20` rather than `2`.

## Multidev Environments

Multidev environments operate under the same principle as the "dev" environment: the environment reflects the `HEAD` commit of the branch that was set up for the environment. Multidev environments have an 11-character restriction on the name of the branch and **MUST** use alpha-numeric characters and dashes, not underscores. It is generally best to create a multidev environment using the Pantheon dashboard, though it is also possible to create a git branch outside of Pantheon, as long as it follows the naming restrictions, and later convert it into a multidev environment. Multidev environments will live at the url `https://[branch-name]-[site-name].pantheonsite.io`.

### Merging environment/branches

Since Pantheon is built on top of vanilla git, it is perfectly acceptable to work on a feature branch and later merge into another feature branch or into `master` using your git tool of choice. In some cases this is actually preferable, especially if there are merge conflicts that need to be resolved. In other cases, Pantheon keeps track of which environments and branches are available and allows you to merge them together in one click:

![](/assets/pantheon-merge.png)

In a normal git workflow, you must first check out the `master` branch before merging into it; similarly, in Pantheon, you must have the target environment open on your screen in order to merge into it.

Merging code in this way will simply merge the branch that powers the selected multidev environment into the environment shown on the screen. Note that it is also possible to merge one multidev environment into another using the same process.

# Initial Git Workflow

At the initial creation of the project, a repo will be provisioned. Developers should check this repo out to begin their work. At the start of a project, all work can be done directly on the `master` branch since the initial work is generally things like setting up modules, features, configuration, etc.

Once multiple developers are working on the project, developers should switch to using feature branches and merging into `master` at the conclusion of the work on that branch. This feature branch workflow minimizes the risk of merge conflicts directly on the `master` branch. It is recommended that developers keep their feature branches small to minimize the risk of divergance.

# Git workflow from Beta Handoff/Content Entry to Pre-Launch

Once the client is allowed into the site, a "test" environment should be created. The client should do all of the content entry in the "test" environment. Developers can create new multidev environments either based off of the current "test" environment (which will create a multidev environment with the codebase of the `master` branch but the files and database of the "test" environment) or from the "dev" environment. Once the client has started content entry, fields **must not** be deleted and databases **must not** be copied into the "test" environment. Only configuration changes, preferably using the [Features](https://www.drupal.org/project/features) module, should be pushed into the "test" environment.

Feature branches can be merged back into the `master` branch in preparation for a push to the "test" environment; however, be aware that everything on the `master` branch will be deployed; it is not possible to pick and choose which commits will be sent to the "test" environment. For this reason, some projects have a dedicated multidev environment called `release` (or something similar) where things can be cherry-picked from feature branches, and which is the only branch to be merged into `master`.

To deploy the changes in the `master` branch to the "test" environment, open the "Test" tab, enter a "Deploy Log Message", and click "Deploy Code from Development to Test Environment." Note that no other branches can be merged directly into the "test" environment.