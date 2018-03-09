# Pantheon Introduction

Pantheon provides a robust three-tier environment that is tied directly to the status of the `master` branch of the git repo provisioned when the project is created. Additionally, an unlimited amount of multi-dev environments can be created based off of any of the main three environments. These environments exist as separate branches of the repository, but they have their own separate files and database storage. Pantheon provides one-click ways to move the code, files, and database between environments.

# Technical Information: How does Pantheon power three or more environments from the same repo?

One of the more confusing aspects for developers first starting out in Pantheon is that the three environments made available: `dev`, `test`, and `live`, all run off the `master` branch in git. Traditionally, when setting up a three-tier environment manually, one might create a "dev" environment based off the `dev` branch of the repo; a "test" environment based off the `test` or `stage` branch; and a "prod" environment based off of `master`. In the Pantheon world, however, everything runs off of `master`.

## The Dev Environment

The simplest of the three environments is the "dev" environment:

![](/assets/pantheon-dev.png)

The dev branch simply reflects the `HEAD` commit of the `master` branch at all times. When code is pushed to the repo on that branch, within seconds the Pantheon dev environment will reflect those changes.

## The Test and Live Environments

The "test" and "live" environments both work the same way. When a Pantheon project is created, the only environment available is the "dev" environment. Clicking on either "Test" or "Live" in the dashboard will show something like this:

![](/assets/Screen Shot 2018-03-09 at 08.15.06.png)

Once the "test" environment has been initialized, a tag will be created on the `master` branch at the current `HEAD` commit, using the format `pantheon_test_nn`. The same is true for the "live" environment: a new tag will be created on the `master` branch at the current `HEAD` commit, using the format `pantheon_live_nn`.

![](/assets/Screen Shot 2018-03-09 at 08.27.56.png)

The "test" and "live" environments are frozen at the highest tag ending in `nn`. It's important to note that this is a numeric sorting, not a string sorting, so while `2` sorts higher than `20` when you're looking at an alphabetical list of tags, Pantheon will load the tag ending in `20` rather than `2`.

## Multi-dev Environments

Multi-dev environments operate under the same principle as the "dev" environment: the environment reflects the `HEAD` commit of the branch that was set up for the environment. Multi-dev environments have an 11-character restriction on the name of the branch and **MUST** use alpha-numeric characters and dashes, not underscores. It is generally best to create a multi-dev environment using the Pantheon dashboard, though it is also possible to create a git branch outside of Pantheon, as long as it follows the naming restrictions, and later convert it into a multi-dev environment. Multi-dev environments will live at the url `https://[branch-name]-[site-name].pantheonsite.io`.

### Merging environment/branches

Since Pantheon is built on top of vanilla git, it is perfectly acceptable to work on a feature branch and later merge into another feature branch or into `master` using your git tool of choice. In some cases this is actually preferable, especially if there are merge conflicts that need to be resolved. In other cases, Pantheon keeps track of which environments and branches are available and allows you to merge them together in one click: