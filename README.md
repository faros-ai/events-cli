# Faros Events CLI

CLI for reporting events to Faros platform

## `faros_event.sh`

The purpose of this script is to abstract away the schema structure of the various CI/CD Faros canonical models. Now when attempting to send a deployment or build event to Faros, only the field values need to be specified and the script takes care of structuring and sending the request.

### Usage

Download and invoke the script in one line:

```text
curl https://raw.githubusercontent.com/faros-ai/faros-events-cli/faros_events.sh | bash -s help
```

### Arguments passing

There are two ways that arguments can be passed into the script. The first is via flags. The second, is via environment variables. If both are set, flags will take precedence over environment variables. By convention, you can switch between using a flag or an environment variable by simple capitalizing the argument name and prefixing it with `FAROS_`. E.g. `--commit_sha` becomes `FAROS_COMMIT_SHA`, `--vcs_org` becomes `FAROS_VCS_ORG`.

Example with mixed argument input:

```sh
FAROS_CI_ORG="<ci_org>" \
FAROS_COMMIT_SHA="<commit_sha>" \
FAROS_REPO="<vcs_repo>" \
./faros_event.sh build -k "<api_key>" \
    --app "<app_name>" \
    --build_status "<build_status>" \
    --pipeline "<ci_pipeline>" \
    --vcs_source "<vcs_source>" \
    --vcs_org "<vcs_organization>"
```

### Arguments

#### Required Arguments

| Flag                          | Environment Variable    | Required By |
| ----------------------------- | ----------------------- | ----------- |
| -k / --api_key \<api_key>     | FAROS_API_KEY           | all         |
| --app \<app>                  | FAROS_APP               | all         |
| --commit_sha \<commit_sha>    | FAROS_COMMIT_SHA        | all         |
| --pipeline \<pipeline>        | FAROS_PIPELINE          | all         |
| --ci_org \<ci_org>            | FAROS_CI_ORG            | all         |
| --deployment_env \<env>       | FAROS_DEPLOYMENT_ENV    | deployment  |
| --deployment_status \<status> | FAROS_DEPLOYMENT_STATUS | deployment  |
| --build \<build>              | FAROS_BUILD             | deployment  |
| --build_status \<status>      | FAROS_BUILD_STATUS      | build       |
| --repo \<repo>                | FAROS_REPO              | build       |
| --vcs_org \<vcs_org>          | FAROS_VCS_ORG           | build       |
| --vcs_source \<vcs_source>    | FAROS_VCS_SOURCE        | build       |

#### Optional Arguments

| Flag                                   | Environment Variable            | Default                   | Used By    |
| -------------------------------------- | ------------------------------- | ------------------------- | ---------- |
| -u / --url \<url>                      | FAROS_URL                       | https://prod.api.faros.ai | all        |
| -g / --graph \<graph>                  | FAROS_GRAPH                     | "default"                 | all        |
| --origin \<origin>                     | FAROS_ORIGIN                    | Faros_Script_Event        | all        |
| --source \<source>                     | FAROS_SOURCE                    | Faros_Script              | all        |
| --start_time \<start>                  | FAROS_START_TIME                | Now                       | all        |
| --end_time \<end>                      | FAROS_END_TIME                  | Now                       | all        |
| --app_platform \<platform>             | FAROS_APP_PLATFORM              | "NA"                      | all        |
| --deployment \<deployment>             | FAROS_DEPLOYMENT                | Random UUID               | deployment |
| --deployment_env_details \<details>    | FAROS_DEPLOYMENT_ENV_DETAILS    | ""                        | deployment |
| --deployment_status_details \<details> | FAROS_DEPLOYMENT_STATUS_DETAILS | ""                        | deployment |
| --deployment_start_time \<start>       | FAROS_DEPLOYMENT_START_TIME     | FAROS_START_TIME          | deployment |
| --deployment_end_time \<end>           | FAROS_DEPLOYMENT_END_TIME       | FAROS_END_TIME            | deployment |
| --build \<build>                       | FAROS_BUILD                     | FAROS_COMMIT_SHA          | build      |
| --build_status_details \<details>      | FAROS_BUILD_STATUS_DETAILS      | ""                        | build      |
| --build_start_time \<start>            | FAROS_BUILD_START_TIME          | FAROS_START_TIME          | build      |
| --build_end_time \<end>                | FAROS_BUILD_END_TIME            | FAROS_END_TIME            | build      |

#### Additional Settings Flags

| Flag          | Description                            |
| ------------- | -------------------------------------- |
| --dry_run     | Print the event instead of sending.    |
| -s / --silent | Unexceptional output will be silenced. |
| --debug       | Helpful information will be printed.   |

### Usage Examples

#### Sending a Build Event

```sh
# Using flags
./faros_event.sh build -k "<api_key>" \
    --app "<app_name>" \
    --build_status "<build_status>" \
    --ci_org "<ci_organization>" \
    --commit_sha "<commit_sha>" \
    --repo "<vcs_repo>" \
    --pipeline "<ci_pipeline>" \
    --vcs_source "<vcs_source>" \
    --vcs_org "<vcs_organization>"

# Using environment variables
FAROS_API_KEY="<api_key>" \
FAROS_APP="<app_name>" \
FAROS_BUILD_STATUS="<build_status>" \
FAROS_CI_ORG="<ci_org>" \
FAROS_COMMIT_SHA="<commit_sha>" \
FAROS_REPO="<vcs_repo>" \
FAROS_PIPELINE="<ci_pipeline>" \
FAROS_VCS_SOURCE="<vcs_source>" \
FAROS_VCS_ORG="<vcs_org>" \
./faros_event.sh build
```

#### Sending a Deployment Event

```sh
# Using flags
./faros_event.sh deployment -k "<api_key>" \
    --app "<app_name>" \
    --ci_org "<ci_organization>" \
    --commit_sha "<commit_sha>" \
    --deployment_status "<deploy_status>" \
    --deployment_env "<environment>" \
    --pipeline "<ci_pipeline>" \
    --build "<build>"

# Using environment variables
FAROS_API_KEY="<api_key>" \
FAROS_APP="<app_name>" \
FAROS_CI_ORG="<ci_org>" \
FAROS_COMMIT_SHA="<commit_sha>" \
FAROS_DEPLOYMENT_STATUS="<deploy_status>" \
FAROS_DEPLOYMENT_ENV="<environment>" \
FAROS_PIPELINE="<pipeline>" \
FAROS_BUILD="<build>" \
./faros_event.sh deployment
```

#### Sending a Full (Build + Deployment) Event

```sh
# Using flags
./faros_event.sh full -k "<api_key>" \
    --app "<app_name>" \
    --build_status "<build_status>" \
    --ci_org "<ci_organization>" \
    --commit_sha "<commit_sha>" \
    --deployment_status "<deploy_status>" \
    --deployment_env "<environment>" \
    --pipeline "<ci_pipeline>" \
    --repo "<vcs_repo>" \
    --vcs_source "<vcs_source>" \
    --vcs_org "<vcs_organization>"

# Using environment variables
FAROS_API_KEY="<api_key>" \
FAROS_APP="<app_name>" \
FAROS_BUILD_STATUS="<build_status>" \
FAROS_CI_ORG="<ci_org>" \
FAROS_COMMIT_SHA="<commit_sha>" \
FAROS_DEPLOYMENT_STATUS="<deploy_status>" \
FAROS_DEPLOYMENT_ENV="<environment>" \
FAROS_REPO="<vcs_repo>" \
FAROS_PIPELINE="<pipeline>" \
FAROS_VCS_SOURCE="<vcs_source>" \
FAROS_VCS_ORG="<vcs_org>" \
./faros_event.sh full
```

## Testing

We use [shellspec](https://github.com/shellspec/shellspec) to test our scripts.

### Install using Homebrew

```sh
brew install shellspec
```

### Running the tests

Move to the `/test` directory and execute `shellspec`

```sh
cd test
shellspec
```
