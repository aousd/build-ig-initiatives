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


# Implementation Leads

- David Aguilar (Walt Disney Animation Studios) - @davvid


# Decisions

## cmake package name transition strategy - (pxr to openusd if and how?)

Before changing the name of the cmake package, we need to decide if we wish to do so,
and if so, how we proceed. Roughly speaking, we have 3 options:

👎 : Don't Change
* Just keep the package called pxr

👍: Staged Deprecation
* Have a period where we create BOTH (or support creating both) a pxr package AND an openusd package
* Go through a standard deprecation cycle (though we could potentially skip some stages - TBD):
    * Experimental / alpha: openusd is introduced as a new/unstable package, and pxr is
    still the default intended primary package
    * Beta: openusd is somewhat stabilized, but pxr is still the default + intended
    primary package
    * Deprecation: pxr is still default, but is marked deprecated and will trigger
    warnings, and pxr becomes the intended primary package
    * New Default: openusd becomes the default, pxr remains as a deprecated package
    * Removal: support for pxr is removed entirely

🚀: Hard Switch
* Switch to a new package name, with NO period where both pxr and openusd are maintained / supported
* ...or perhaps just an extremely abbreviated period

### Voting Results

The votes cast in issue #24 went as follows:

* 1 vote for "Don't change"
* 6 votes for "Staged Deprecation"
* 4 votes for "Hard Switch"

While there is no official decision yet, given that many expressed the desire for a
staged deprecation, it would not hurt to explore the the Staged Deprecation route
initially since it does not preclude moving forward with a Hard Switch later.


# Implementation Plan (Staged Deprecation)

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

* Declare that `pxrConfig.cmake` and its related `PXR_LIBRARIES`, `PXR_INCLUDE_PATH` and
other pxr-specific cmake variables are officially deprecated.

* After enough time (TBD) passes, remove `pxrConfig.cmake` and any related cmake scaffolding.


# Pull Requests and Issues

* https://github.com/aousd/build-ig-initiatives/issues/24

* https://github.com/aousd/build-ig-initiatives/issues/25


# Action items

* Discuss the deprecation plan with Pixar.
