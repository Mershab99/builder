## Build builder of python version 3.9

### Build py39 stack

```shell
bazel run //builders/py39/stack:build
```

This command creates two images:

```shell
openfunctiondev/buildpacks-py39-run:v1
openfunctiondev/buildpacks-py39-build:v1
```

### Build py39 builder

```shell
bazel build //builders/py39:builder.image
```

This command creates one image:

```shell
of/py39
```

Tag and push:

```shell
docker tag of/py39 <your container registry>/py39:v1
docker push <your container registry>/py39:v1
```

### Test

```shell
bazel test //builders/py39/acceptance/...
```

Output example:

```shell
INFO: Analyzed 2 targets (1 packages loaded, 5 targets configured).
INFO: Found 1 target and 1 test target...
INFO: Elapsed time: 31.606s, Critical Path: 31.33s
INFO: 2 processes: 1 internal, 1 local.
INFO: Build completed successfully, 2 total actions
//builders/py39/acceptance:python_fn_test                            PASSED in 31.3s

Executed 1 out of 1 test: 1 test passes.
INFO: Build completed successfully, 2 total actions
```

### Run

Download gcp samples:

```shell
git clone https://github.com/GoogleCloudPlatform/buildpack-samples.git
```

Build the function:

> Add `--network host` to pack and docker command if they cannot reach internet.

```shell
cd buildpack-samples/sample-functions-framework-python/
pack build function-python --builder of/py39 --env FUNC_NAME="hello"
docker run --rm -p8080:8080 function-python
```

Visit the function:

```shell
curl http://localhost:8080
```

Output example:

```shell
hello, world
```