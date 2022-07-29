Things to do before this becomes a real charm (woefully incomplete)

- figure out an appropriate license for the charm (Apache 2 might be
  a sensible choice, as some other StorPool OpenStack software is
  already licensed under that, but take a look at other things in
  the charm ecosystem and consider theirs if different)
- drop most of the configuration options that do not apply to us
- maybe keep the "protocol" config option with "iscsi" and "block" as
  valid values, and then, for the present, go to the "maintenance"
  state with a "not yet supported" kind of status message for "block"
- add a "storpool\_conf" multiline string config option with no default
  and write its contents atomically to `/etc/storpool.conf` (only if
  different); some of that logic may be copied from the old charm
- add a "storpool\_template" multistring config option with something
  like "hybrid" or "ssd" as the default and add it to the Cinder config
  section passed on to the "cinder" charm
- either get rid of the per-release main() dispatch logic completely or
  implement it correctly by invoking a "get the current OpenStack release"
  function (only if it turns out that we need to do different things for
  different OpenStack releases)
- (probably Peter) prepare the infrastructure for installing
  StorPool-specific dependencies from a Ubuntu package:
  - finish the storpool-rollup tool that packages up
    storpool-openstack-integration (if needed) and the storpool and
    storpool.spopenstack Python libraries into a single Ubuntu package
  - create a StorPool charmed-openstack PPA (or maybe a better name)
  - set up some infrastructure/processes for uploading packages there,
    probably based on repopush as Peter already does for the sp-contrib
    and sp-infra repositories
  - create an e.g. "charm-cinder-storpool" metapackage that depends on
    everything we need if there is more than what would be rolled up
  - tell the charm about the StorPool PPA, its signing key, and
    the need to install the metapackage
- write some unit tests for the charm's option handling and storpool.conf
  installation
- write some user-oriented documentation for the charm
- write some developer-oriented (HACKING.md-style) documentation for the charm
- set up a trivial CI job that downloads the charm from a central repo and
  runs the Python source unit tests
- set up a less-than-trivial CI job that deploys a charmed OpenStack
  environment, installs the charm, and performs some end-to-end tests
- possibly set up a less-than-trivial CI job that deploys a charmed OpenStack
  environment, installs the charm, then upgrades the environment along one or
  more of the orthogonal upgrade axes (OpenStack release, Ubuntu release,
  possibly even StorPool version, etc)
- add other tasks that will inevitably come up during development
