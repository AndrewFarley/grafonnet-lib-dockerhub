# Grafonnet Lib Docker Image
A simple Docker image that has jsonnet and grafana's grafonnet lib for CI/CD purposes, or to avoid having to install jsonnet and grafonnet on your computer (keep it in docker).

## Sample Usage
If you have a single dashboard called generateme.jsonnet in the current folder to compile you can...
```bash
docker run -v `pwd`:/here andrewfarley/jsonnet-bundler-grafonnet-lib:latest jsonnet /here/generateme.jsonnet
```
Or to send it to a file...
```bash
docker run -v `pwd`:/here andrewfarley/jsonnet-bundler-grafonnet-lib:latest jsonnet /here/generateme.jsonnet > generateme-output.json
```

If you have a folder full of dashboards in the folder `dashboards-jsonnet` to compile you can...
```bash
mkdir generated-dashboards
for file in dashboards-jsonnet/*; do name=$(basename $file); docker run -v `pwd`:/here andrewfarley/jsonnet-bundler-grafonnet-lib:latest jsonnet /here/$file > generated-dashboards/${name%.jsonnet}.json; done
```

Or if you want to build from within' a CI/CD builder (eg: Gitlab) here is a sample Gitlab-CI job....
```yaml
created-dashboards:
  stage: build-artifacts
  image: "andrewfarley/jsonnet-bundler-grafonnet-lib:latest"
  script:
    - mkdir generated-dashboards
    - for file in dashboards-jsonnet/*; do name=$(basename $file); jsonnet $file > generated-dashboards/${name%.jsonnet}.json; done
  artifacts:
    paths: ['generated-dashboards']
```

## Relevant Links
DockerHub: https://hub.docker.com/r/andrewfarley/jsonnet-bundler-grafonnet-lib
Github: https://github.com/AndrewFarley/grafonnet-lib-dockerhub

## Attribution
Original Dockerfile Author: https://github.com/mlushpenko
