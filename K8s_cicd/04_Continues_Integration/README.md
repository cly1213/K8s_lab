# CI
Static file check

Shellcheck
 - entrypoint.sh

yamllint
 - yaml file

hadolint(Dockerfile lint)
 - Dockerfile

## CI Pipeline/Service

- Self-Hosted
  - CI server

- SaaS
  - HA

## Github Action(SaaS)
```
on:
  push:
    branches:
      - '*'
  pull_request:
    branches:
      - '*'

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2

    - name: Install required packages
      run: sudo apt-get install -f yamllint shellcheck

    - name: Run shell check
      run: |
        pushd cicd/application
        make shellcheck
        popd
    - name: Run docker check
      run: |
        pushd cicd/application
        make docker-lint
        popd
    - name: Run yaml check
      run: |
        pushd cicd/application
        make yaml-lint
        popd
```

```
on:
  push:
    branches:
      - '*'
  pull_request:
    branches:
      - '*'

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2

    - name: Install required packages
      run: sudo apt-get install -f yamllint shellcheck

    - name: Run shell check
      run: |
        pushd cicd/application
        make shellcheck
        popd
    - name: Run docker check
      run: |
        pushd cicd/application
        make docker-lint
        popd
    - name: Run yaml check
      run: |
        pushd cicd/application
        make yaml-lint
        popd
  build: 
    runs-on: ubuntu-latest
    needs: test
    steps:
    - uses: actions/checkout@v2

    - name: build docker-image
      run: |
        pushd cicd/application
        make build-image
        popd  
```

Ref:

https://docs.github.com/en/actions

## Jenkins(Self-Hosted)

