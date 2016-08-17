# Intermodal

Intermodal is a collection of guest containers for use within the Harbor platform. Like intermodal shipping container these are standardized to provide a consistent base; with common entry-points supporting both [Cloud-init](https://cloudinit.readthedocs.io/en/latest/) (utilizing OpenStack/EC2 metadata) and [Systemd](https://github.com/systemd).

## Builds / Release

The built images from this repository may be found at the Port.direct [docker hub](https://hub.docker.com/u/port/). Releases are split into two streams: 'latest' and 'stable'. 'Latest' builds for the Port.direct continuous release, while stable tracks the upstream OpenStack Release schedule and will only receive critical security updates following a release. It is strongly recommend that unless you have very specific requirements to use the the 'latest' release stream for the best experience.
