# Creating a release of mapbox.js

The majority of the release process is done on a release branch. Once all the artifacts are built and published, you will need to get your release reviewed and approved. Once approved the release branch is ready to be merged into the default `publisher-production` branch. After being merged the release can be tagged and Github release created.

Checklist:
- [ ] Make sure the changelog has been updated
- [ ] Do the release (part 1) in a release branch (using manual or automated process below)
- [ ] Create release PR and get it reviewed and approved
- [ ] Merge release branch into `publisher-production`
- [ ] Finalize the release (part 2)

## Release part 1: from release branch (version bump, cdn publish, npm publish, docs generation)

Update <MAJOR.MINOR.PATCH> to your version. i.e. 3.2.1

### Option 1: Automated release

```terminal
$ npm run release <MAJOR.MINOR.PATCH>
```

### Option 2: Manual release

```terminal
# Bump version
$ npm version --no-git-tag-version <MAJOR.MINOR.PATCH>
$ git add package*.json
$ git commit -m "Update package*.json: <MAJOR.MINOR.PATCH>"

# Publish to NPM
$ npm login
$ npm publish

# Publish to Mapbox CDN
$ ./deploy.sh v<MAJOR.MINOR.PATCH>

# Generate docs pages
$ ./deploy.sh v<MAJOR.MINOR.PATCH>

# Update mapboxjs version in `_config.yaml`, `_config.publisher-production.yml`, `_config.publisher-staging.yml`
$ find . -name '_config*.yml' -exec sed -i '' "s/^\\(\\s*mapboxjs\\s*:\\s*\\).*/\\1 <MAJOR.MINOR.PATCH>/" {} \;

# Commit configs and docs.
$ git add _config*.yml docs/*
$ git commit -m "Update docs/*: <MAJOR.MINOR.PATCH>"
```

### Release part 2: from publisher-production branch (tag and release)

```terminal
$ git checkout publisher-production
$ git pull origin publisher-production
$ git tag -a v<MAJOR.MINOR.PATCH> -m <MAJOR.MINOR.PATCH> release
$ git push origin "$tag" --tags
# login to Github and create release
```