Publish Workshop
================

This GitLab CI/CD component supports the following for an Educates workshop:

* Creating an OCI image artifact containing workshop content files and pushing
  it to the project's GitLab container registry.
* Creating a generic package artifact containing the workshop's generated `workshop.yaml`
  and pushing it to the project's GitLab package registry.
* Creating a release against the GitLab repository and attach as assets
  Kubernetes resource files for deploying the workshop to Educates.

Note that this GitLab CI/CD component can only publish a single workshop and will only
publish an OCI image artifact containing the workshop files.

The GitLab CI/CD component requires that it be triggered in response to a Git tag being
applied to the GitLab project.

GitLab Workflow
---------------

The name of the GitLab CI/CD component is:

```
$PATH/$TO/$GITLAB/$PROJECT/publish-workshop
```

To have an Educates workshop published upon the project being tagged, add the following
`.gitlab-ci.yml` to your project:

```yaml
include:
  - component: $CI_SERVER_FQDN/$PATH/$TO/$GITLAB/$PROJECT/publish-workshop@0.2.7
```

You can find the correct way to `include` the CI/CD component in your GitLab instance's
[CI/CD Catalog](https://docs.gitlab.com/ee/ci/components/#cicd-catalog). For more information
on how to publish this CI/CD component to your GitLab instance's catalog, read the
[repository's README.md](../../README.md).


Workshop Definition
-------------------

This GitLab CI/CD component makes use of the `educates publish-workshop` command. As
such, the workshop definition must include a `spec.publish` section which
defines where the image is to be published:

```
apiVersion: training.educates.dev/v1beta1
kind: Workshop
metadata:
  name: {name}
spec:
  publish:
    image: $(image_repository)/{name}-files:$(workshop_version)
```

The workshop definition may optionally specify what files should be included in
the OCI image artifact for the workshop.

```
apiVersion: training.educates.dev/v1beta1
kind: Workshop
metadata:
  name: {name}
spec:
  publish:
    image: $(image_repository)/{name}-files:$(workshop_version)
    files:
    - directory:
        path: .
      includePaths:
      - /workshop/**
      - /templates/**
      - /README.md
```

See the Educates documentation for more information.

Component Inputs
--------------------

Configuration parameters which can be set in the `inputs` object of the respective
project's `.gitlab-ci.yml`:

| Name                            | Default | Type     | Description                        |
|---------------------------------|----------|----------|------------------------------------|
| `path`                          | `.`    | String   | Relative directory path under `$GITHUB_WORKSPACE` to workshop files. Defaults to "`.`". |
| `educates-version`              | `2.6.0` | String | Version of the Educates CLI to be used for creating the workshop assets. |
| `trainingportal-resource-file`  | `resources/trainingportal.yaml`    | String   | Relative path under workshop directory to the `TrainingPortal` resource file. |
| `workshop-resource-file`        | `resources/workshop.yaml`    | String   | Relative path under workshop directory to the `Workshop` resource file. |

You would define inputs in your project's `.gitlab-ci.yml` like this:

```yaml
inputs:
  path: "./workshops/k8s-introduction"
  educates-version: "2.7.1"
include:
  - component: $CI_SERVER_FQDN/$PATH/$TO/$GITLAB/$PROJECT/publish-workshop@0.2.7
```
