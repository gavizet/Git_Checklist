# Git Checklist

## Initialization Git

### Create a develop branch (one time)

```bash
git branch develop
git push -u origin develop
```

This branch will contain the complete history of the project, whereas `master` will contain an abridged version. Other developers should now clone the central repository and create a tracking branch for develop:

## Development

### Development branch

```bash
git clone ssh://user@host/path/to/repo.git
git checkout develop
```

Everybody now has a local copy of the historical branches set up.

### New features

```bash
git checkout -b some-feature develop
```

Add commits to the feature branch in the usual fashion: edit, stage, commit:

```bash
git status
git add <some-file>
git commit
git push

[...]

git status
git add <some-other-file>
git commit
git push
```

### Finished feature

```bash
# Push the CHANGELOG.md (latest commit before merge)
git pull origin develop
git checkout develop
git merge some-feature
git push
git branch -d some-feature
git push -d origin some-feature
```

The first command makes sure the develop branch is up to date before trying to merge in the feature. Note that features should never be merged directly into `master`.

## Release

### Prepare a release

```bash
git checkout -b release-0.1 develop
```

This branch is a place to clean up the release, test everything, update the documentation, and do any other kind of preparation for the upcoming release. It’s like a feature branch dedicated to polishing the release.

### Finishes the release

Once the release is ready to ship, Merges it into `master` and `develop`, then deletes the `release` branch.

```bash
git checkout master
git merge release-0.1
git push
git checkout develop
git merge release-0.1
git push
git branch -d release-0.1
```

Release branches act as a buffer between feature development (develop) and public releases (master). Whenever you merge something into master, you should tag the commit for easy reference:

```bash
git tag -a 0.1 -m "Initial public release" master
git push --tags
