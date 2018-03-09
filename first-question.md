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

![](/assets/Screen Shot 2018-03-09 at 08.15.06.png)Once the environment has been initialized, a tag will be created on the `master` branch at the current `HEAD` commit, using the format `pantheon\test\nn`. 

