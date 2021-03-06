title: Development Dependencies in Docker

# Problem

Gems, npm modules, etc. get added, updated, and removed frequently in development. Baking the 
dependencies into the container is less than ideal.


# Solutions

1. `bundle install` or `npm install` every time the container runs
  - works, but could be slow due to rubygems.org or npmjs.org
2. bind mount in the gems folder
  - works, but leaves artifacts in the host

## Workflow

Developers are used to running `bundle install` on their dev machines.
To support this workflow, the gem folder needs to be bind mounted into Docker,
effectively sharing the same folder on dev machine and container.

