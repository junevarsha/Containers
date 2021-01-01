- Open Container Initiative : Docker, Buildah-Podman, runc
https://btholt.github.io/complete-intro-to-containers/buildah

- Docker: Docker builds, docker demon executes

- Buildah: builds, podman executes (some overlap exists)

- docker run -it --rm -p 3000:3000 --privileged --mount type=bind,source="$(pwd)",target=/src  --mount type="volume",src=podman-data,target=/var/lib/containers tomkukral/buildah bash

- buildah bud -f ./Dockerfile -t my-app-buildah . # instead of bud, you can use build-using-dockerfile : This accomplishes the same thing as docker build.

- buildah run --net host my-app-buildah-working-container -- bash
    - container inside container 

- podman run --cgroup-manager cgroupfs -p 3000:3000 localhost/my-app-buildah
