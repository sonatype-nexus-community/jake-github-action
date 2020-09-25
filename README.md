<p align="center">
    <img src="https://github.com/sonatype-nexus-community/jake/blob/main/docs/images/jake.png" width="350"/>
</p>

<p align="center">
    <a href="https://gitter.im/sonatype-nexus-community/jake?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge"><img src="https://badges.gitter.im/sonatype-nexus-community/jake.svg" alt="Gitter"></img></a>
</p>

# Jake for GitHub Actions

Run [Sonatype Jake](https://github.com/sonatype-nexus-community/jake) as part of your GitHub Actions workflow.

## Inputs

### `packagesListFile`

**Default** : `packages.list` 

Optional command providing the path to a file containing the list of packages, typically the output from a "conda list" command.

### `jakeCommand`

**Default** : `ddt` 

You can customize this input with other commands and flags recognized by `jake`. 
 
For example: `ddt --quiet`

## Example Usage

The example below shows the use of a custom package list file.
```
name: Jake Github Action

on: [push]

jobs:
  build:
    runs-on: python:3.8-slim-buster
    steps:
    - name: Check out code
      uses: actions/checkout@v2

    - name: Run Jake
      uses: sonatype-nexus-community/jake-github-action@main
      with:
        packagesListFile: myPackages.list
```

## Development

I found it useful to leverage the [act](https://github.com/nektos/act) project while developing
this github action. This project allows you to push a branch to the github action repo, and use a commit hash to test the behavior
of that branch. For example, a [test project](https://github.com/bhamail/jake-gh-action-test) that uses the `jake-github-action` 
could have the following `.github/workflows/jake.yml` file. 
Notice the commit hash `950a8965cd37d8e14aaa6aebd6c0d71b4da71fa3` used below in the `Scan` step to run the 
development branch. 

```
name: Jake

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:

  build:
    name: Build
    runs-on: python:3.8-slim-buster
    steps:
    - name: Check out code
      uses: actions/checkout@v2

    - name: Scan
      uses: sonatype-nexus-community/jake-github-action@950a8965cd37d8e14aaa6aebd6c0d71b4da71fa3
      with:
        jakeCommand: ddt --quiet
```
 
  * Gotchya - As of go v1.15, there is an issue using `act` related to how docker handles http `identity`
  connections. Due to this issue, I had to run `act` in a Linux Virtual Machine when running go 1.15. The error 
  you see running `act` resulting from this issue looks similar to this:
    ```
    $ act 
    [Go/Build] 🚀  Start image=node:12.6-buster-slim
    [Go/Build]   🐳  docker run image=node:12.6-buster-slim entrypoint=["/usr/bin/tail" "-f" "/dev/null"] cmd=[]
    [Go/Build]   🐳  docker cp src=/Users/bhamail/sonatype/community/go/gh-action-test/. dst=/github/workspace
    Error: error during connect: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.40/exec/9f2eb3f2ea59b7e41c32efe56a90c2919fe4b459b3f1e763dd02686f797839da/start": net/http: HTTP/1.x transport connection broken: unsupported transfer encoding: "identity"
    ```

## The Fine Print

It is worth noting that this is **NOT SUPPORTED** by Sonatype, and is a contribution of ours
to the open source community (read: you!)

Remember:

* Use this contribution at the risk tolerance that you have
* Do NOT file Sonatype support tickets related to `Nancy for GitHub Actions` support in regard to this project
* DO file issues here on GitHub, so that the community can pitch in

Phew, that was easier than I thought. Last but not least of all:

Have fun creating and using Nancy for GitHub Actions, we are glad to have you here!

## Getting help

Looking to contribute to our code but need some help? There's a few ways to get information:

* Chat with us on [Gitter](https://gitter.im/sonatype-nexus-community/nancy)
