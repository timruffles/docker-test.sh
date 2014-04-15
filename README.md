# Docker Test

Set of bash programs to test a project that uses a Dockerfile to create a runnable image.

## Use

```sh
npm install docker-test.sh
```

Implement `bin/COMMAND_NAME-hook.sh` for each binary you'd like to use. You'll need to expose all non-optional callback functions - just define bash functions of the same name. You'll also need to define the non-optional variables - either set them as env vars at runtime, or define in the hook file.

The binaries are exposed via `npm`, so ensure your path includes local `node_modules/.bin`. Run the commands from a directory relative to your `bin` folder that contains the hooks.

## Binaries

### `docker-test-jenkins`

Runs a build for jenkins, where the repo has been checked out.

Hook file: `docker-test-jenkins.sh`

#### Variables

Either specify when running the bin, or in `callbacks.sh`.

```sh
# tag for image
IMAGE_TAG=auth-server

# (optional) the repo to tag built images on
DOCKER_REPO_URL=docker-registry.development.yourdomain.com:5000

# (optional) if you have a separate image-file for testing (e.g that installs with 
# testing dependencies) specify its path
TEST_DOCKERFILE=test/Dockerfile

# (optional) CSV of branches to deploy after a test that passes (see run_test_container)
DEPLOY_BRANCHES=dev,master,staging
```

#### Callbacks

##### `test_setup()` (optional)

Runs before test container, ensures test environment setup

##### `run_test_container()`

Executes test. Ensure exit status is `1` on test failures.

##### `test_teardown()` (optional)

Cleans up after test run.

##### `after_push()` (optional)

Run a functional after an image is pushed (and thus tests have passed).
