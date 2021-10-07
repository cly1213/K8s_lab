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

Install

```
docker network create jenkins
docker volume create jenkins-docker-certs
docker volume create jenkins-data

docker container run --name jenkins-docker --rm --detach \
  --privileged --network jenkins --network-alias docker \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume jenkins-docker-certs:/certs/client \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:2376  docker:dind

docker container run --name jenkins-blueocean --rm --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  --publish 8080:8080 --publish 50000:50000 jenkinsci/blueocean
```

Jenkins 安裝密碼

```
#透過 docker volume inspect 來觀察 jenkins-data 的實際位置
docker volume inspect jenkins-data

# 切換成 root 移動過去
sudo su
cd ..


# 根據網頁提示，獲取初始密碼 secrets/initialAdminPassword
cat secrets/initialAdminPassword
```