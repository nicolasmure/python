# Python docker image

This repository has the same structure than the
[docker-library/python](https://github.com/docker-library/python)
repository.

It aims to create python docker images from other flavors than the ones that
are proposed by the above repository (c.f.
[python](https://hub.docker.com/_/python) images on docker hub).

## Flavors

- `centos7.6`

## Build

```bash
$ docker build -t nicolasmure/python:3.7.4-centos7.6 3.7/centos7.6/
```
