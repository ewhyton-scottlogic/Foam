# Containerize and Deploy FriendFace to Container Apps

## Static web app

Building the docker image `docker build .` when in the same directory as the Dockerfile. To add a tag you can name it before the `.` e.g. `docker build --tag hello .`

When doing the `docker login <name>.azurecr.io` command, the login is the login for the container registry! Which can be found in the access keys part of the container registry in the portal, so you can copy and paste these details when they ask (username and password) into the terminal.

Then you can use the `docker tag <name of image> <login server/name>` so for example using static web app, `docker tag friend-face-static friendfacestatic.azurecr.io/friendface:latest`. To tag you may also be able to use the SHA instead of the image name.

Then to push to the ACR you can simply use the command `docker push <tagged name>`.

To run the docker image locally use command `docker run -it -d -p <ports> <image-tag>`. FriendFace is set up so that UI is on port 3000 so in the run you set the ports as 3000:80 and the API runs on port 8080, so you'd set the ports so that it is 8080:8080. But you can map the ports to different ones if you wanted to.

## FriendFace app

Build the docker image, push to the ACR then you can either use the CLI command `az containerapp up -n <name of app> --location uksouth --image <image tag name>`. This command is like a PUT request as in it creates or updates, if you only need to update then you can use the command `az containerapp update -n <name of app> -g <resource group> --image <image tag name>`, and it might be worth giving it more CPU and memory if it's an API e.g. `--cpu 1 --memory 2.0Gi` (there are specific amounts of CPU to memory ratio that you must follow).
You can also do this in the portal by going on the revision management section → create new revision → selecting the image from the repository and provision its resources.

Needed to specify the location of the API on the UI in the constants which was the address of the container app. Normally you would have environment variables, so you could still run locally.

Note: I had a problem with my API not having enough resources, and it was extremely difficult to realise that this was the problem as it just wasn't working and there were minimal logs, it took 10 mins to provision the container and then had an error saying it crashed.

Question: would you normally deploy a container app for each container? When you've got multiple containers how would you normally do it? For web apps you would deploy a web app for each service, I'm doing it with a docker compose file and using the command `az containerapp compose` (hopefully if it works)

Should make a `docker-compose.yml` file so that they can be built together in the same container, Then publish that to container apps?
