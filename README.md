# ghes-private-package-gem-on-ghes-instance
This repository host a simple hello world gem and will act as a private packages hosted on GHES instance to test a particular GHES QA signoff usecase: A version update can be completed on GHES instance from private packages hosted on GHES instance.

Manul Steps:

1. Create a new repository in GHES Instance
2. Add the below Gemfile.lock to the new repository at root directory
```ruby
GIT
  remote: https://github.com/<GHES-instance-name>/org-name/ghes-private-package-gem-on-ghes-instance
  revision: 63db43ab77623bd3d0d9d31db381a3670585fc25
  branch: main
  tag: v1.0.0
  specs:
    ghes-private-package-gem-on-ghes-instance (1.0.0)

GEM
  remote: https://rubygems.org/
  specs:

PLATFORMS
  ruby

DEPENDENCIES
  ghes-private-package-gem-on-ghes-instance!

BUNDLED WITH
   2.1.2
```

3. Add the below Gemfile to the new repository at root directory
```ruby
# frozen_string_literal: true
source 'https://rubygems.org'

gem 'ghes-private-package-gem-on-ghes-instance', git: 'https://github.com/<GHES Instance name>/org-name/ghes-private-package-gem-on-ghes-instance', branch: "main"
```

4. Add the below `dependabot.yaml` file at `.github/dependabot.yml` in the new repository.

```yaml
# To get started with Dependabot version updates, you'll need to specify which
# package ecosystems to update and where the package manifests are located.
# Please see the documentation for all configuration options:
# https://help.github.com/github/administering-a-repository/configuration-options-for-dependency-updates

version: 2
registries:
  github-repo:
    type: git
    url: https://<GHES-instance-name>
    username: "honeyankit"
    password: ${{secrets.MY_GHES_INSTANCE_TOKEN}}

updates:
  - package-ecosystem: "bundler"
    directory: "/" # Location of package manifests
    registries:
       - github-repo # Allow version updates for dependencies in this registry
    schedule:
      interval: "daily"
    insecure-external-code-execution: allow
```

5. We need to add PAT token (from [https://<GHES-instance-name>](https://<GHES-instance-name>/settings/tokens)) to the `Dependabot Secrets` of the new repository in the GHES instance. Below are the steps.

   Go to repository's `Settings` --> `Secrets` --> `Dependabot` --> click on `New repository secret` --> enter `MY_GHES_INSTANCE_TOKEN` in the `Name` section and paste the PAT token in the `Secret` section and click on `Add secret`.

6. The dependaobt should be able to bump the version of the `ghes-private-package-gem-on-ghes-instance` gem.

