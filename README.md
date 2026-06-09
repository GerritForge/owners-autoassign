# Gerrit Owners-Autoassign Plugin

The `owners-autoassign` plugin has the ability to parse the OWNERS file in the repository for
defining who is the owner of the components in a repository. The plugin find the owners
of every change and automatically add them as reviewers or CC.

For details on how to configure either plugin, please refer to the docs in the
specific plugin's folder.

> **NOTE**: A comprehensive introduction to both `owners` and `owners-autoassign` has been
> given as part of the [GerritMeets series in Jan 2025](https://www.youtube.com/watch?v=NfIHnJF30Wo).
> While a further deep dive specifically into newer features of this plugin was
> give at the [GerritMeets in April 2026](https://www.youtube.com/watch?v=DfKYduVKJEY)

## owners-autoassign

This plugin parses the same OWNERS file format as the owners plugin. It will
automatically assign all of the owners as reviewers to newly created or updated
changes. It also allows for completely custom management of the attention set,
i.e. allows, via custom integrations, to not add people on holiday to the
attention set, or that the same user is not added to too many changes at the
same time, etc...

## Building the plugins

This plugin is built with Bazel in Gerrit tree

Create symbolic links of the owners and owners-autoassign folders to the Gerrit
source code /plugins directory.

Create a symbolic link of the owners-common-api plugin to the Gerrit source code
/plugins directory.

Then build the owners and owners-autoassign plugins with the usual Gerrit
plugin compile command.

Example:

```
  git clone https://gerrit.googlesource.com/plugins/owners
  git clone https://gerrit.googlesource.com/plugins/owners-common-api
  git clone --recurse-submodules https://gerrit.googlesource.com/gerrit
  cd gerrit/plugins
  ln -s ../../owners .
  ln -s ../../owners-common-api .
  ln -sf owners-common-api/external_plugin_deps.bzl .
  cd ..
  bazel build plugins/owners-autoassign
```

NOTE: the owners-common folder is producing shared artifacts for the two plugins
and does not need to be built separately being a direct dependency of the build
process. Its resulting .jar must not be installed in gerrit plugins directory.

The output is created in

```
  bazel-bin/plugins/owners-autoassign/owners-autoassign.jar
```

To execute the tests run:

```
  bazel test plugins/owners-autoassign/...
```

This project can be imported into the Eclipse IDE:

Add the plugin name to the `CUSTOM_PLUGINS` (and in case when you want to run
tests from the IDE to `CUSTOM_PLUGINS_TEST_DEPS`) in Gerrit core in
`tools/bzl/plugins.bzl` file and run:

```
  ./tools/eclipse/project.py
```

