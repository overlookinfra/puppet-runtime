# Mirrored repository

This repository is synced from upstream on a regular basis.
This default `plumbing` branch contains code to run the sync, this `README`, and various tasks for building this project with Overlook InfraTech modified tooling.

The other branches hold the actual code, and regular snapshots are taken and stored in the `backup/*` path.

Change to the `main` or `master` branch to see the repo you probably expected.

## Tagging
When `puppetlabs` tags their repo, run `bundle exec rake overlookinfra:tag['<puppetlabs tag>']`. This will create a new branch called `overlookinfra/<tag>` which contains everything from the puppetlabs tag, and then with the overlookinfra.patch applied. This patch commit is then tagged with `<puppetlabs tag>-overlookinfra` and branch and tag are pushed to this repo.

## Building
Run `bundle exec rake overlookinfra:build['<project>','<overlook tag>',<num threads>]`. This will use Docker and the overlookinfra-modified version of vanagon to build this project for all platforms in parallel.  For example, `bundle exec rake overlookinfra:build['agent-runtime-main', '202410140-overlookinfra', 8]`. For a system with 32 hardware threads and 32GB of RAM, 8 threads is a reasonable number. Ensure you have swap enabled in case the build takes more than the available RAM on your system.

The build script `build-vanagon.rb` is used to do the build in parallel. It copies the repo to temporary directories, volume mounts the repo into a container of the appropriate type, and runs the build. It then copies the output artifacts back to this repo's `output` directory.

This script is basically spike code at the moment. This will be made much nicer, cleaner, centralized across repos, and more flexible in the future. If you'd like to contribute to this effort, please do!
