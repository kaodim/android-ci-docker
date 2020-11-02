# Docker base images for Android apps compiled with Java 8
[![CircleCI](https://circleci.com/gh/kaodim/android-ci-docker/tree/master.svg?style=svg&circle-token=686b83ae12d1ada7e1000ebc29b08d827bcb7698)](https://circleci.com/gh/kaodim/android-ci-docker/tree/master)

Android-ci-docker is a [Docker](https://www.docker.com) images meant to serve as good bases for **Android** mobile apps build via CircleCI.

---------------------------------------

<a name="whats_included"></a>
### What's included?

*Android-ci-docker is built on top of circleci images: [Circle-Ci_Android](https://github.com/CircleCI-Public/circleci-dockerfiles/tree/master/android/images).

<a name="how_to_use"></a>
### How to use?

1. Fork/clone the project and setup a circle ci project.
2. Add `DOCKER_USERNAME` and `DOCKER_PASSWORD` of your [Docker Hub](https://hub.docker.com) in your Circle CI Environment Variables.
3. Push the repo to build the custom image 

<a name="how_to_upgrade_image"></a>
### How to upgrade image?

If you need to maintain different images for your project, i.e target for different Android APIs,
1. Create a new folder `android-api-{version-number}`
2. Create a new Dockerfile inside your new folder. You can copy the Dockerfile from existing folders or from [Circle-Ci_Android](https://github.com/CircleCI-Public/circleci-dockerfiles/tree/master/android/images)
3. Make sure you edit the java version to `openjdk:8-jdk-slim` and build-tools version for your targeted SDK.
4. Edit your circle ci config to targetyour new folder. 

```
...
      - checkout
      - setup_remote_docker
      - run:
          name: Setup custom environment variables
          command: |
            echo 'export ANDROID_REPO="$DOCKER_USERNAME/android-ci-api-{version-number}"' >> $BASH_ENV
      - run:
          name: Build Docker image
          command: |
            source $BASH_ENV
            export TAG=`if [ "$CIRCLE_BRANCH" == "master" ]; then echo "latest"; else echo $CIRCLE_BRANCH ; fi`
            docker build -t $ANDROID_REPO:$CIRCLE_BUILD_NUM -t $ANDROID_REPO:$TAG ./android-api-{version-number}
...
   
```
5. Commit and push your changes. A new docker image will be created via Circle-Ci

