Cloud Ops:

Regarding the builder-system(renaming to trailblazer) I have drafted a rough plan outline looks good to me you guys can review it and add 
remarks about it.

For the first part, to make sure we have something working, we’ll build a simple product that detects changes in GitHub code, copies the 
updated code locally into a Docker image, builds it, and then fakes the artifact upload. Since artifact uploading is mostly a plug-and-play 
process, we won’t focus on it initially.

In the second iteration, we’ll implement a DAG-based build system. Users will provide a BUILD file, which we’ll read to plot the dependency 
graph and generate the build instructions. They will also provide a Dockerfile containing all the tools and dependencies required for the 
build. This tool artifact will be built first, after which the required number of instances will be created. Each build step that is fully 
dependent will be executed in its own separate container. Once all steps are complete, the final container will spin up, link everything 
together, and output the final files into a common storage location.

At this stage, we won’t consider cross-dependencies. Completely independent threads will run in separate containers and be built 
independently to keep the logic simple.

Once we have confidence in the build system, we’ll implement caching. Initially, this will be a simple cache that hashes .o files, stores 
them, and clears them after a set time. From there, we’ll evaluate the results and decide on the next direction for the team.
