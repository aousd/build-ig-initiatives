---
name: openusd-cmake-package
title: Change the name of the OpenUSD cmake package from pxr to openusd
about: Use openusd as the cmake package name to better align with standard conventions
labels: needs-discussion, needs-tac-approval
assignees: davvid
---
# Description

Currently, the OpenUSD cmake recipe creates a `pxrConfig.cmake`, which means that the
name of the cmake package for downstream consumers is `pxr`.

This is non-standard in the cmake ecosystem - the package name is generally not the
company name. For example, the name of the OpenEXR cmake package is `OpenEXR`, not
`ilm`.

However, changing the package name is fairly major - it's obviously a breaking change
that requires all downstream consumers to adapt their code.


# Champion

- David Aguilar (Walt Disney Animation Studios) - @davvid


# Decisions

## cmake package name transition strategy - (pxr to openusd if and how?)

We will follow the conventional deprecation strategy. This initiative will add the new
`openusd` cmake packaging alongside the existing `pxr` cmake packaging. A mechanism to
opt-out of providing the deprecated `pxr` packaging will be provided.

Typically, a deprecated feature is required to stay in the, "to be removed," state for a
single release before being removed in the next release.

Due to the high-impact nature of build system changes, we plan to keep the deprecated
functionality around for longer than required. We are aiming for at least two releases
(roughly 6 months or more) where we will continue supporting the `pxr` cmake packaging
alongside the proposed `openusd` cmake packaging.


# Implementation Plan

* Following a Staged Deprecation approach.

* Leave the existing pxrConfig.cmake and related cmake variables as-is.

* Adjust the top-level `project(...)` declaration to use `openusd` instead of `pxr`.

* Add a new `OPENUSD_INSTALL_CMAKEDIR` cmake option variable corresponding to the
location of the new packaging files. For Windows, this would correspond to a
`cmake/openusd` subdirectory and for Unix this would correspond to a
`${CMAKE_INSTALL_LIBDIR}/cmake/openusd` subdirectory relative to the
`${CMAKE_INSTALL_PREFIX}` installation prefix.

* Create a `cmake/openusd/openusd-config.cmake` template file in the repository to
contain the OpenUSD cmake packaging boilerplate.

* Update cmake build scripts to provide
`${OPENUSD_INSTALL_CMAKEDIR}/openusd-config.cmake` for use by cmake's `find_package()`.

* Update cmake build scripts to provide
`${OPENUSD_INSTALL_CMAKEDIR}/openusd-config-version.cmake`.

* Update cmake build scripts to export all OpenUSD cmake targets within the
`openusd::` cmake target namespace.

* Add a new `OPENUSD_INSTALL_DEPRECATED_PXR_CMAKE_CONFIG` (default `ON`) option variable
that will omit `pxrConfig.cmake` from the installation when set to `OFF`.

* Update the build system to print a `message(WARNING ...)` when
`OPENUSD_INSTALL_DEPRECATED_PXR_CMAKE_CONFIG` is enabled. This implies that most users
will start seeing a warning when configuring openusd.

* Declare that `pxrConfig.cmake` and its related `PXR_LIBRARIES`, `PXR_INCLUDE_PATH` and
other pxr-specific cmake variables are officially deprecated and will be removed in a
future release.

* After enough time (TBD) passes, remove `pxrConfig.cmake` and any related cmake scaffolding.
