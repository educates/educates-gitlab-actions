Educates GitLab CI/CD Components
================================

This repository contains a collection of [GitLab CI/CD components](https://docs.gitlab.com/ee/ci/components/#cicd-catalog) supporting development and use of Educates, including user created workshop
content for Educates.

The GitLab CI/CD components included here are:

* [Publish Workshop](templates/publish-workshop/README.md) - Publishes a workshop to
  your GitLab project's container registry and creates a release with Kubernetes resource
  definitions for deploying the workshop as assets.

Note that versioning applies to the collection as a whole. This means that if a
breaking change is made to a single component, then the version is incremented on
all components, even though changes may not have been made to the other components.

Importing Components into the GitLab CI/CD Catalog
--------------------------------------------------

This collection is meant for **use on GitLab** - it is only hosted on GitHub for convenience.
In order to import the components contained in this collection into **your GitLab's CI/CD Catalog**,
[import the repository into your GitLab instance](https://docs.gitlab.com/ee/user/project/import/github.html).
