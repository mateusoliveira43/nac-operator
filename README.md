# Non Admin Controller Operator for OADP

A Non Admin Controller (NAC) Operator for [OADP operator](https://github.com/openshift/oadp-operator).

## Requirements

To run the project, it is necessary the following tools:

- [Go](https://go.dev/dl/) 1.20 or higher
- [Make](https://www.gnu.org/software/make/)

## Development

TODO

### Operator SDK

The project was generated using Operator SDK version `v1.33.0`, running the following commands
```
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
```
> The information about plugin and project version, as well as project name, repo and domain, is stored in [PROJECT](PROJECT) file

TODO how to upgrade Operator SDK version

## Quality

The quality metrics of the project are reproduced by the continuos integration (CI) pipeline of the project. CI configuration in [`.github/workflows/ci.yml`](.github/workflows/ci.yml) file.

### Tests

To run tests and coverage report, run
```
TODO
```

TODO report, coverage and tests information

### Linters and code formatters

To run Go linters and check Go code format, run
```
TODO
```

To fix Go linters issues and format Go code, run
```
TODO
```

Go linters and Go code formatters configuration in [`.golangci.yaml`](.golangci.yaml) file.

To check all repository's files format, run
```
TODO
```

## License

This repository is licensed under the terms of [Apache License Version 2.0](LICENSE).
