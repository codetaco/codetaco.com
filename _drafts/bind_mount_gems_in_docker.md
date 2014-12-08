title: Using Docker to Develop Ruby Apps

# Problem

Gems get added, updated, and removed frequently in development. Baking the gems 
into the container is less than ideal.


# Solutions

1. `bundle install` every time the container runs
  - works, but could be slow due to rubygems.org or Internet issues
2. bind mount in the gems folder
  - works, but leaves artifacts in the host
