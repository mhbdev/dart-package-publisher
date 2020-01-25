[![Build Status][ci-badge]][ci-badge-url]

## The Challenge

Usually, when you are trying to `pub lish` your package and your `pubspec.yaml` has *the same* version as already published, you'll have a non zero exit code from `pub` tool. 
So you cannot directly put this command as an re-action `on: push` event. 

This Action is intended to solve this challenge by continuously Test & Publish Dart packages only **IF** local `pubspec.yml` has **different** version than already published on [Pub.dev](http://pub.dev) site.

It's handy to hook it up on `push, pull_request` [events][3].
```yaml
on: [push, pull_request]
```
and forget about it. When you need to publish a package, just bump the version in `pubspec.yaml`.

## Inputs

### `accessToken`

**Required** Google Account token from `~/.pub-cache/credentials.json` 
Put it as `secrets.OAUTH_ACCESS_TOKEN` on your repo [secrets section][1]

You can find the credentials.json within `.pub-cache` in the User's home directory.
You can use `open ~/.pub-cache`.

### `refreshToken`

**Required** Google Account token from `~/.pub-cache/credentials.json` 
Put it as `secrets.OAUTH_REFRESH_TOKEN` on your repo [secrets section][1]

You can find the credentials.json within `.pub-cache` in the User's home directory.
You can use `open ~/.pub-cache`.

### `relativePath`

**Optional** Path to your package root in your repository. In case you have a mono-repo, like this [one][2]

### `flutter`

**Optional** Declares a package as a Flutter package. Default: `false`

### `dryRunOnly`

**Optional** Perform dry run only, no real publishing. Default: `false`

### `skipTests`

**Optional** Skip unit tests run. Default: `false`

### `suppressBuildRunner`

**Optional** Suppress using `build_runner` for unit tests run. Default: `false`


## Outputs

### `package`

Package name from pubspec

### `localVersion`

Package local version from pubspec

### `remoteVersion`

Package remote version from pub.dev

### `dartVersion`

Dart SDK version which is being used to run tests & publish

### `flutterVersion`

Flutter SDK version which is being used to run tests & publish


## Dart package example usage

```yaml
name: Publish to Pub.dev

on: push

jobs:
  publishing:
    runs-on: ubuntu-latest
    steps:
      - name: 'Checkout'
        uses: actions/checkout@v2 # required!
        
      - name: '>> Dart package <<'
        uses: k-paxian/dart-package-publisher@master
        with:
          accessToken: ${{ secrets.OAUTH_ACCESS_TOKEN }}
          refreshToken: ${{ secrets.OAUTH_REFRESH_TOKEN }}
```

## Flutter package example usage

```yaml
name: Publish to Pub.dev

on: push

jobs:
  publishing:
    runs-on: ubuntu-latest
    steps:
      - name: 'Checkout'
        uses: actions/checkout@v2 # required!

      - name: '>> Flutter package <<'
        uses: k-paxian/dart-package-publisher@master
        with:
          accessToken: ${{ secrets.OAUTH_ACCESS_TOKEN }}
          refreshToken: ${{ secrets.OAUTH_REFRESH_TOKEN }}
          flutter: true
```

[ci-badge]: https://github.com/k-paxian/dart-package-publisher/workflows/Workflow%20test/badge.svg
[ci-badge-url]: https://github.com/k-paxian/dart-package-publisher/actions
[1]: https://help.github.com/en/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets
[2]: https://github.com/k-paxian/dart-json-mapper
[3]: https://help.github.com/en/actions/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#example-using-a-list-of-events
