# Orientation and setup

# Docker concepts
Docker is a platform for developers and sysadmins to build, run and share applications with containers. The use of containers to deploy applications is called containerization. Containers are not new, but their use for easily deploying application is.

Containerization is increasingly popular because containers are:
- **Flexible**: Even the most complex applications can be containerized.
- **Lightweight**: Containers leverage and share the host kernel, making them much more efficient in terms of system resources than virtual machines.
- **Portable**: You can build locally, deploy to the cloud, and run anywhere.
- **Loosely Coupled**: Containers are highly self sufficient and encapsulated, allowing you to replace or upgrade one without disrupting others.
- **Scalable** - You can increase and automatically distribute replicas across a datacenter.
- **Secure** - Container apply agressibe constraints and isolations to processes without any configuration required on the part of the user.

# Containers and virtual machines
A container run natively on Linux and shares the kernel of the host machines with other containers. It runs a discrete process, taking no more memory than any other executable, making it lightweight.

By contrast, a virtual machine(VM) runs a full-blown "guest" operating system with virtual access to host resources through a hypervisor. In general VM incur a lot of overhead beyond what is being consumed by your application logic.

| | |
|:-:|:-:|
|![docker](https://docs.docker.com/images/Container%402x.png)|![vm](https://docs.docker.com/images/VM%402x.png)|
| Docker| Virtual Machine |

# Build and run your image
Now that you've setup  your development environment, you can begin to develop containerized applications. In general, the development workflow look like this:
1. Create and test individual containers for each component of your application by first creating Docker image.
2. Assemble your containers and supporting infrastructure into a complete application.
3. Test, share, and deploy your complete containerized application.

Let's focus om step 1 of this workflow: creating the images that your container will be based on. Remember, a Docker image captures the private filesystem that your containerized process will run in; you need to create an image that contains just what your application needs to run.

## Set up
Let us downloaded the `node-bullrtin-board` example project. This is a simple bulletin board application written in Node.js
```bash
git clone https://github.com/dockersamples/node-bulletin-board
cd node-bulletin-board/bulletin-board-app
```
## Define a container with Dockerfile
After downloading the project, take a look at the file called `Dockerfile` in the bulletin board application. Dockerfiles describe how to assemble a private filesystem for a container, and also contain some metadata describing how to run a container based on this image.

## Build and test your image
Now that you have some source code and a Dockerfile, it's time to build your first image, and make sure the containers launched from it works as expected.

Make sure you're in the directory `node-bulletin-board/bulletin-board-app` in a terminal or Powershell using the `cd` command. Run the following command to build your bulletin board image:
```bash
docker build --tag bulletinboard:1.0 .
```
You'll see Docker step through each instruction in your Dockerfile, building up your image as it goes. If successful, the build process should end with message `Successfully tagged bulletinboard:1.0`.

## Run your image as a container
1. Run the following command to start a container based on your new image:
    ```bash
    docker run --publish 8000:8080 --detach --name bb bulletinboard:1.0
    ```
    There are couple of common flag here:
    - `--publish` ask Docker to forward traffic incoming on the host's port 8000 to the container's port 8080. Containers have their own private set of ports, so if you want to reach one from the network, you have to forward traffic to it in this way. Otherwise, firewall rules will prevent all network traffic from reaching your container, as a default security posture.
    - `--detach` ask Docker to run this container in the background.
    - `--name` specifies a name with which you can refer to your container in subsequenct commands in this case `bb`.
2. Visit your application in a browser at `localhost:8000`. You should see your bulletin board application up and running. At this step, you would normally do everything you could to ensure your container works the way you expected; now would be the time to run unit tests, for example.
3. Once your statisfied that bulletin board container works correctly, you can delete it.   
    ```bash
    docker rm --force bb
    ```
    The `--force` option stops a running container, so it can be removed. If you stop the container running with `docker stop bb` first, then you do not need to use `--force` to remove it.


# Sample Dockerfile
Writing a Dockerfile is the first step to containerizing an application. You can think of these Dockerfile commands as a step-by-step recipe on how to build up your image. The Dockerfile in the bulleting board app looks like this:

```bash 
# Use the official image as a parent image.
FROM node:current-slim

# Set the working directory.
WORKDIR /usr/src/app

# Copy the file from your host to your current location.
COPY package.json .

# Run the command inside your image filesystem.
RUN npm install

# Add metadata to the image to describe which port the container is listening on at runtime.
EXPOSE 8080

# Run the specified command within the container.
CMD [ "npm", "start" ]

# Copy the rest of your app's source code from your host to your image filesystem.
COPY . .
```
The dockerfile defined in this example takes the following steps:
- Start `FROM` the pre-existing `node:current-slim` image. This is an _offical image_, built by the node.js vendors and validated by Docker to be a high-quality image containing the Node.js Long Term Support(LTS) interpreter and basic dependencies.
- Use `WORKDIR` to specify that all subsequent action should be taken from the directory `/usr/src/app` in your image filesystem(never the host's filesystem).
- `COPY` the file `package.json` from your host to the present location`(.)` in your image(so in this case, to `/usr/src/app/package.json`)
- `RUN` the command `npm install` inside your image filesystem(which will read `package.json` to determine your app's node dependencies, and install them).
- `COPY` in the rest your app's source code from your hist to your image file system.

You can see that these are much the same steps you might have taken to set up and install your app on your host. However, capturing these as a Dockerfile allows you to do the same thing inside a portable, isolated Docker image.
 
The steps above built up the filesystem of our image, but there are other lines in your Dockerfile.

The `CMD` directive is the first example of specifying some metadata in your image that describes how to run a container based on this image. In this case it's saying that the containerized process that this image is meant to support is `npm start`.

The `EXPOSE 8080` informs Docker that the container is listening on port 8080 at runtime.

# Share images on Docker Hub
At this point, you've built a containerized application described above on your local development machine. 
The final step in developing a containerized application is to share your images on a registry like `Docker Hub`, so they can be easily downloaded and run on any destination machine.
