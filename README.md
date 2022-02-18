# Gitlab CI Notes

## Stages
Are run sequentially, first stage, then next stage, etc. Running a stage means running all the jobs that belong to that stage in parallel.
```yaml
stages:
  - build
  - test
  - deploy
```

## Cache
Tells which files should be cached between jobs.
```yaml
cache:
  key: $CI_COMMIT_REF_SLUG		# slugized version of the branch name/tag
  paths:
    - node_modules/
```

## Variables
Variables scoped to the CI pipeline you are executing.
```yaml
variables:
  COMPONENT_NAME: folkvangur
  BUILD_NUMBER: printf %04d $CI_PIPELINE_IID
```
Note: The printf line will be passed unevaluated to whoever is using BUILD_NUMBER.

## Jobs
The heavy lifters. Always belong to stages.
```yaml
Do some testing:  # The job name
  stage: test
  tags:
    - docker    # Tags which type of gitlab ci runner we want to run this job
  only:         # Only run this job if conditions within are met
    changes:
      - src/
  script:       # Script to run inside the runner
    - echo "Hello world"
    - dotnet test
  artifacts:    # Outputs of the job
    paths:
      - MyProject/bin/Release/net6.0
    reports:
      junit:
        - src/My.UnitTests/unit_test_report.xml
```
