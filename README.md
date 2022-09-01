# jfr-runtime-action

This is a brief introduction of `jfr-runtime-action`.
It aims at running the Jenkins pipeline inside the host machine directly.
If you want to learn more about the usage of this action,
you can check the [central documentation page](https://jenkinsci.github.io/jfr-action-doc).

## Inputs

| Name | Type | Default Value | Description |
| ----------- | ----------- | ----------- | ----------- |
| `command` | String | run | The command to run the jenkinsfile-runner. The supported commands are `run` and `lint`. |
| `jenkinsfile` | String | Jenkinsfile | The relative path to Jenkinsfile. |
| `jcasc` | String | N/A | The relative path to Jenkins Configuration as Code YAML file. |

## Example

`jfr-runtime-action` will run the pipeline in the host machine directly.
The users need to use `jfr-setup-action` in advance.
If the users want to install extra plugins, they can use `jfr-plugin-installation-action`.
The advantage of `jfr-runtime-action` is that it can run in all kinds of runners provided by GitHub Actions.
The users can call this action by using `jenkinsci/jfr-runtime-action@master`.

```yaml
name: CI
on: [push]
jobs:
  jfr-runtime-action-pipeline:
    strategy:
      matrix:
        os: [ ubuntu-latest, macOS-latest, windows-latest ]
    runs-on: ${{matrix.os}}
    name: jfr-runtime-action-pipeline
    steps:
      - uses: actions/checkout@v2
      - name : Setup Jenkins
        uses:
          jenkinsci/jfr-runtime-action@master
      - name: Jenkins plugins download
        uses:
          jenkinsci/jfr-plugin-installation-action@master
        with:
          pluginstxt: plugins.txt
      - name: Run Jenkins pipeline
        uses:
          jenkinsci/jfr-runtime-action@master
        with:
          command: run
          jenkinsfile: Jenkinsfile
```
