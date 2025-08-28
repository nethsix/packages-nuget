# Hello.Buildkite example NuGet package

This Github repo is configured to use Buildkite to build a NuGet packet and publish it to a NuGet registry on Buildkite Packages.
The Buildkite setup is in '.buildkite' folder.

## '.buildkite/pipline.yml' explanation

## Files in this repo and what they do

## Additional setup

* Create base image with Dockerfile
  * 'dotnet' required to build NuGet package
  * 'python3' required by Buildkite 'publish to packages' plugin
```
FROM docker.io/buildkite/hosted-agent-base:ubuntu-v1.0.1@sha256:f1378abd34fccb2b7b661aaf3b06394509a4f7b5bb8c2f8ad431e7eaa1cabc9c

FROM mcr.microsoft.com/dotnet/sdk:latest
RUN apt-get update && apt-get install -y python-is-python3
```

* Create packages NuGet registry
  * Add 'OIDC' policy
```
- iss: https://agent.buildkite.com
  scopes:
    - read_packages
    - write_packages
  claims:
    organization_slug:
      equals: anothertest
    pipeline_slug:
      in:
        - packages-nuget
    build_branch:
      matches:
        - main
        - feature/*
```

## TODOs

Is there are way to create pipeline, base iamges, etc., all through API?

* References
  * Buildkite 'publish to packages' plugin
    * Plugin Page: https://buildkite.com/resources/plugins/buildkite-plugins/publish-to-packages-buildkite-plugin/
    * Github Repo: https://github.com/buildkite-plugins/publish-to-packages-buildkite-plugin
  * Add 'OIDC' policy
    * Reference: https://buildkite.com/docs/package-registries/security/oidc
