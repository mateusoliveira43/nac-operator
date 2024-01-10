# Non Admin Controller Operator for OADP

A Non Admin Controller (NAC) Operator for [OADP operator](https://github.com/openshift/oadp-operator).

## Development

### Requirements

To run the project, it is necessary the following tools:

- [Go](https://go.dev/dl/) 1.20 or higher
- [Make](https://www.gnu.org/software/make/)
- A container tool
- A Kubernetes cluster

### Quick start

For a quick start testing of the operator, create `nac-operator` namespace and run
```sh
make deploy-test
```

After finish testing, run
```sh
make undeploy-olm
```

> **NOTE:** To run operator in different namespace, run `NAMESPACE=<name> make <command>`.

### Development flow

After developing code changes, if `api/` folder was modified, run
```sh
make generate
```

If `api/` or `config/` folders were modified, run
```sh
make bundle
```

Also check [Quality section](#quality), for information about to enforce quality of proposed changes.

> **NOTE:** Run `make help` for more information on all potential `make` targets.

### Architecture

The project was generated using Operator SDK version `v1.33.0` (which uses kubebuilder version `v3.12.0`), running the following commands
```sh
operator-sdk \
    --plugins go.kubebuilder.io/v4 \
    --project-version 3 \
    init --project-name=nac-operator \
    --repo=github.com/mateusoliveira43/nac-operator \
    --domain=openshift.io
operator-sdk \
    --plugins go.kubebuilder.io/v4 \
    create api --group oadpnac \
    --version v1alpha1 \
    --kind NonAdminBackup \
    --resource --controller
operator-sdk \
    --plugins go.kubebuilder.io/v4 \
    create api --group oadpnac \
    --version v1alpha1 \
    --kind NonAdminRestore \
    --resource --controller
make bundle # fill in the questions
```
> **NOTE:** The information about plugin and project version, as well as project name, repo and domain, is stored in [PROJECT](PROJECT) file

To upgrade Operator SDK version, create Operator SDK structure using the current Operator SDK version and the upgrade version, using the same commands presented earlier, in two different folders. Then generate a `diff` file from the two folders and apply changes to project code.

## Quality

The quality checks of the project are reproduced by the continuos integration (CI) pipeline of the project. CI configuration in [`.github/workflows/ci.yml`](.github/workflows/ci.yml) file.

TODO create CI structure

TODO create CD structure : deploy new images

To run all checks locally, run `make ci`.

### Tests

To run tests, run
```sh
make test
```

TODO report, coverage and tests information

### Linters and code formatters

To run Go linters and check Go code format, run
```sh
make lint
```

To fix Go linters issues and format Go code, run
```sh
make lint-fix
```

Go linters and Go code formatters configuration in [`.golangci.yaml`](.golangci.yaml) file.

To check all files format, run
```sh
make ec
```

Files format configuration in [`.editorconfig`](.editorconfig) file.

## License

This repository is licensed under the terms of [Apache License Version 2.0](LICENSE).
