## Scenario

A `docker/Dockerfile` for a leiningen-based clojure project.  Pushes are detected by Google Cloud Build ("./cloudbuild.yaml"), which builds the Dockerfile and pushes it to `gcr.io/personalsdm-216109/linking-test-gcr`. This image is based on the `java-atomist` base image that we use internally.

* run `./bump.clj` (executable - requires bb installation) to increment a
  version to increment a version and push.
* Google Cloud Build is setup to see this push and to build and push docker/Dockerfile (a multi-stage Dockerfile using `lein metajar`)
* resulting Image tagged with `latest` and pushed to
  `gcr.io/$PROJECT_ID/linking-test-gcr`.
* A subscription on the `gcr` topic is set up to notify Atomist when images are pushed to this registry.
* labels `org.opencontainers.image.revision` and
  `org.opencontainers.image.source` have been added to the builds (in "./cloudbuild.yaml")

