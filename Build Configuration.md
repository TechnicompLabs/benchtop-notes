# TechniComp Benchtop Linux — Build Configuration

TechniComp Benchtop Linux is an immutable, openSUSE Slowroll-based desktop image, built with kiwi on the openSUSE Build Service (OBS). Each package's source is held in a GitHub repository, and OBS retrieves those sources automatically through scmsync. The image follows the current Slowroll release rather than a fixed snapshot. OBS rebuilds it automatically whenever Slowroll changes, and it produces an image only when the entire package set resolves. When Slowroll is temporarily inconsistent, no new image is produced, and the most recent successful image remains available. The build uses Slowroll's packages without any local overrides.

## Source repositories

The build is composed of three public repositories under github.com/TechnicompLabs, each mapping to a single OBS package.

- `benchtop-settings` builds the OBS package `tc-benchtop-settings`. This is a configuration package whose files are installed directly from `Source0` onward, without a tarball.
- `benchtop-patterns` builds `patterns-tc-benchtop`, which produces the metapackage `patterns-tc-benchtop-base`. That metapackage lists approximately 340 `Requires`, and the image depends on it.
- `benchtop-image` builds `tc-benchtop-image`, the kiwi image description. It uses an oem, btrfs read-only-snapshot layout and was derived from Aeon.

Each OBS package is connected to its repository by an scmsync entry in its `_meta`:

```
<scmsync>https://github.com/TechnicompLabs/<repo>#main</scmsync>
```

## Build Service project structure

```
home:technicomp:benchtop           # base project; repository "openSUSE_Slowroll" -> path openSUSE:Slowroll/standard
  tc-benchtop-settings
  patterns-tc-benchtop
home:technicomp:benchtop:images    # Type: kiwi; paths to home:technicomp:benchtop/openSUSE_Slowroll and openSUSE:Slowroll/standard
  tc-benchtop-image                # requires patterns-tc-benchtop-base
```

The image resides in its own subproject because a kiwi build requires `Type: kiwi` in the project configuration, and applying that setting to the base project would break the RPM builds there.

## Automatic rebuilds

A push to any repository rebuilds its package immediately, rather than waiting for the scmsync poll. This is configured with one OBS workflow token and one organization-level GitHub webhook.

The token is created as follows. The GitHub token it requires needs only the `repo:status` scope, so that OBS can report build results back onto the commits.

```
osc token --create --operation workflow --scm-token <GITHUB_PAT>   # prints an id and a secret
```

A single webhook is then added under TechnicompLabs -> Settings -> Webhooks, using the id and secret from that command:

- Payload URL: `https://build.opensuse.org/trigger/workflow?id=<id>`
- Content type: `application/json`
- Secret: `<secret>`
- Event: push

Each repository contains a `.obs/workflows.yml` that names its own project and package:

```yaml
rebuild_on_push:
  steps:
    - rebuild_package:
        project: <project>
        package: <package>
  filters:
    event: push
    branches:
      only:
        - main
```

## Common commands

```
osc results home:technicomp:benchtop:images tc-benchtop-image                 # image status; any value other than "unresolvable" indicates it is building
osc buildinfo home:technicomp:benchtop:images tc-benchtop-image openSUSE_Slowroll x86_64 \
  2>&1 | grep -iE "nothing provides|unresolvable"                             # lists every unmet dependency in a single pass
osc service remoterun home:technicomp:benchtop <package>                      # forces scmsync to retrieve the source again if it does not update on its own
osc rebuild <project> <package>                                              # forces a rebuild
osc cat <project> <package> <file>                                          # displays the source file that OBS currently holds
```

## Notes on build behavior

OBS rebuilds automatically both when Slowroll updates and when a repository is pushed, so a build rarely needs to be triggered manually.

Because the image is built as a single unit, the entire package set must resolve at once. A single missing or mismatched Slowroll package prevents the whole image from building until Slowroll provides the correct version.

A package that shows a `disabled` build in Slowroll is not necessarily missing. Slowroll disables the build for certain packages and instead ships a binary imported from openSUSE Factory. To determine whether a package is actually available, use `osc buildinfo` rather than the `disabled` flag.

Slowroll occasionally ships a newer package before its dependency is available. Recent instances were `cockpit-ws` requiring a newer `selinux-policy`, and `qemu` requiring a newer `xen` (`libxenctrl`). These resolve on their own once Slowroll updates the dependency, and a large package such as `xen` should not be built locally to compensate.

If a package that Slowroll cannot provide is genuinely required, it can be linked from Factory with `osc linkpac openSUSE:Factory <package> home:technicomp:benchtop`, and removed afterward with `osc rdelete`. None are linked at present.

Commit messages contain no co-author or tooling attribution.

## Current state

The build resolves cleanly, apart from transient inconsistencies within Slowroll itself. It is currently waiting on Slowroll, and OBS will build the image once Slowroll is consistent again.
