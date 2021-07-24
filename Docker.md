# Docker 🐋

⌚ Last Updated on: 07/24/2021

***

- Run applications in an isolated environment(same advantages as running an app in a virtual machine).
  - Acts the same way since it is in its "development" environment.
  - Allows you to isolate different projects(sandboxing).
  - Use somebody else's project -> just run Docker container and it contains all the dependencies and what not.
  - Solves the "it works on my machine" problem.
  - Can also remove app and all its dependences in one go! (easy clean up)
- Easier to work with than virtual machine -> Docker uses containers (not a full virtual machine).
  - Run a virtual machine, each machine gets it own kernel and full OS (resource heavy on the host machine). A hypervisor like VirtualBox or VMWare is used to create/manage the VM.
  - In containers, the kernel (core part of OS) is shared(and the host machine's kernel is used) but everything on top of that is separate (e.g bins/, libraries, app environments) -> you get your isolated environments but less resource heavy.
- Docker Architecture is based on the Client-Server model(connected via a RESTful API).
  - The Docker engine sits in the background and takes care of building and running containers. 
  - Containers are technically a regular process like other processes on your computer. They share the OS of the host machine. (e.g you can only run Windows and Linux containers on a Windows host but Docker on Mac uses a lightweight Linux VM).
- A container is a running instance of an image(which is the template for the environment you want). The image has all the OS,libraries/software, and application code all bundled together.
- Docker workflow:
  - Make the application first -> then "Dockerize" it (make the `Dockerfile`)
  - Once we have this image we can push it to a registry (Docker Hub)[like GitHub for Docker images].
  - Then this image can be pulled by anyone else(e.g into the test/production environment).
  - **Your container will need an OS, your apps dependencies, app files, and you need to run your app in the container as well.**
-  Images are defined by `Dockerfiles` ==> All the configuration goes here.
  - Build the `Dockerfile` ->build to get an image(snapshot) -> run to get a container.
  - Start with an existing image in a `Dockerfile` (go to hub.docker.com) -> Use official ones  (look at given instructions)
  - `FROM [BASE IMAGE]` -> start from base image and add your own things to it. Get the image names from hub.docker.com
  - `EXPOSE [PORT]` -> When run this image and you get a container, the container will listen in on the specified port.
  - `WORKDIR [DIR]` -> Change your working directory to `[DIR]`
    - Make sure you know what your start directory is when you use official images.
  - `COPY ./[PATH IN HOST] /[PATH IN CONTAINER] ` -> copy your files into a new directory in the container.
  - `CMD [OS/TERMINAL COMMAND]` -> Run the command in the OS (useful when telling docker to run/start your app). The last `CMD` is the one that becomes the default command that specifies the live of the container. This is the command that gets run when you run the container.
    - `RUN [[OS/TERMINAL COMMAND]]` -> Run the command in the OS but actually commit it(during Build time). Lots of these will be used and are used to build up environment.
  - `docker build -t [NAME] [DIR LOCATION OF DOCKERFILE]` -> Build your image
  - `docker run -p [PORT OF HOST]:[PORT OF CONTAINER] [IMAGE NAME]` -> Run image into container and forward ports from host to container
  - Want to make a change? Rebuild and re-run. => Alternative: volumes.
    - Can share files between host and container and can also share and persist files between containers.
    - To share files between host and container -> `docker run -p [PORT OF HOST]:[PORT OF CONTAINER] -v [PATH OF DIR YOU WANT TO MOUNT/SHARE WITH CONTAINER]:[DIR IN CONTAINER YOU WANT IT TO BE COPIED/MOUNTED TO] [IMAGE NAME]`  => Changes will be reflected right away. Also the paths must be **absolute**!
    - Life of a container is tied to the life of the main  process of that container(be aware of that). Once it ends, the container stops running. -> Try to have containers with only one process(run multiple containers if need be).
  - Images are not single files -> how Docker stores these images is complex and is beyond the scope of a regular developer :) 
  - To see all images on host: `docker image ls `(You'll get a swanky table on the terminal with metadata for each image).
  - `play with docker` website -> Can create a VM instance (with Linux and Docker). You can pull and run the image you deployed on Docker Hub here. (Use `docker pull [image name]` to get the image from Docker Hub) or just use `docker run [image name]` -> will check to see if you have this image locally, if not then it does downloads it like `pull`.
  - To create a Docker container you can interact with: `docker run -it [image name]`(e.g running a Ubuntu image will give you a Linux root shell)
  - Use `docker export [container id] > result.tar` and then `tar -xvf result.tar` to get the container's files on your host/system.
  - To create a container you can detach/attach to later -> Add the `-t -i` when you `run` the container. Then enter `Ctrl+p+q` to detach -> to reattach run: `docker attach [CONTAINER ID]`
  - To get the stdout/logs of a container: `docker logs [container ID]` (Need specific drivers - so check before you start your container See __Resources Used__ section )

***

### 🥽🥼 Resources Used:

- https://www.youtube.com/watch?v=YFl2mCHdv24&list=RDQMCCdRNOc4KIE&start_radio=1 (*Learn Docker in 12 Minutes 🐳* by `Jake Wright`)
- https://www.youtube.com/watch?v=pTFZFxd4hOI (*Docker Tutorial for Beginners [2021]* by `Programming with Mosh`) [⚠ Note ⚠: half of this tutorial is a Linux tutorial...so just focus on the first half if you are already comfortable with Linux Commands and/or Linux terminal usage]
- https://stackoverflow.com/questions/19688314/how-do-you-attach-and-detach-from-dockers-process#:~:text=To%20detach%20from%20the%20container,Ctrl%20and%20press%20P%20%2B%20Q%20. (*How do you attach and detach from Docker's process?* by `https://stackoverflow.com/users/356788/ken-cochrane`)
- https://docs.docker.com/engine/reference/commandline/logs/ (*docker logs* by `Docker Inc`)
- https://stackoverflow.com/questions/37461868/difference-between-run-and-cmd-in-a-dockerfile (*Difference between RUN and CMD in a Dockerfile* by `https://stackoverflow.com/users/6386350/takesoup`)

