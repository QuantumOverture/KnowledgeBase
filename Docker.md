# Docker

- Run applications in an isolated environment(same advantages as running an app in a virtual machine).
  - Acts the same way since it is in its "development" environment.
  - Allows you to isolate different projects(sandboxing).
  - Use somebody else's project -> just run Docker container and it contains all the dependencies and what not.
- Easier to work with than virtual machine -> Docker uses containers (not a full virtual machine).
  - Run a virtual machine, each machine gets it own kernel and full OS (resource heavy on the host machine).
  - In containers, the kernel (core part of OS) is shared(and the host machine's kernel is used) but everything on top of that is separate (e.g bins/, libraries, app environments) -> you get your isolated environments but less resource heavy.
- A container is a running instance of an image(which is the template for the environment you want). The image has all the OS,libraries/software, and application code all bundled together.
-  Images are defined by `Dockerfiles` ==> All the configuration goes here.
  - Build the `Dockerfile` ->build to get an image(snapshot) -> run to get a container.
  - Start with an existing image in a `Dockerfile` (go to hub.docker.com) -> Use official ones  (look at given instructions)
  - `EXPOSE [PORT]` -> When run this image and you get a container, the container will listen in on the specified port.
  - `docker build -t [NAME] [DIR LOCATION OF DOCKERFILE]` -> Build your image
  - `docker run -p [PORT OF HOST]:[PORT OF CONTAINER] [IMAGE NAME]` -> Run image into container and forward ports from host to container
  - Want to make a change? Rebuild and re-run. => Alternative: volumes.
    - Can share files between host and container and can also share and persist files between containers.
    - To share files between host and container -> `docker run -p [PORT OF HOST]:[PORT OF CONTAINER] -v [PATH OF DIR YOU WANT TO MOUNT/SHARE WITH CONTAINER]:[DIR IN CONTAINER YOU WANT IT TO BE COPIED/MOUNTED TO] [IMAGE NAME]`  => Changes will be reflected right away.
    - Life of a container is tied to the life of the main  process of that container(be aware of that). -> Try to have containers with only one process(run multiple containers if need be).



***

### Resources Used

- https://www.youtube.com/watch?v=YFl2mCHdv24&list=RDQMCCdRNOc4KIE&start_radio=1 (*Learn Docker in 12 Minutes 🐳* by `Jake Wright`)
- https://www.youtube.com/watch?v=pTFZFxd4hOI (*Docker Tutorial for Beginners [2021]* by `Programming with Mosh`)

