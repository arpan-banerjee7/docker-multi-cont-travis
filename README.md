# docker-multi-cont-travis
A multi container docker setup and continuous integration with Travis CI

# Developed using Docker for Windows running on WSL2 backend. So all the below steps are written as per that.

## Steps to run this project in Production Mode
1. Download and configure Docker
2. Open `Windows Terminal`
3. **Run-** `docker-compose up`


## Steps to run this project in Development Mode, with hot reloads using docker volumes
**When using WSL2 as a backend for Docker Desktop, the project should be created on or copied directly to the Linux file system. If the project is on the Windows file system, the volumes will likely not work. All docker commands should be run within WSL2 and not on Windows.**
1. To open your WSL2 operating system, type wsl in the Windows / Cortana Search Bar and click wsl.
2. In the WSL2 terminal change into your root user directory by running: `cd ~`
3. Run explorer.exe . to open up a file browser within WSL2.
4. **Run-** `docker-compose up`

> Now, you can make any changes in the source code files and it will automatically reflect the changes in the app running inside the container!

*Rmember, if you make any changes to the package.json files , then you have to run `docker-compose build` then run `docker-compose up` to be super sure!*

## Steps to generate Secure Access Token to enable Travis-ci to push images to your Docker Hub as shown in the next steps
Rather than use your login password in Travis CI, you can use access tokens from docker hub. This way you can revoke the key if it's compromised and can keep your password safe. See https://docs.docker.com/docker-hub/access-tokens/

1. Go to hub.docker.com  and login
2. At the top right, click on your profile -> account settings
3. Navigate to "Security"
4. Under access tokens, click "New Access Token"
5. Enter a name such as "Travis CI" and click "Create"
6. Copy the access token to the clipboard and use this as your "DOCKER_PASSWORD" environment variable instead of your account password.
7. This should be passed in the same way as in the video, i.e.  <br>
`- echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_ID" --password-stdin`

## Steps for Travis-CI integration
1. Create an account in https://travis-ci.com/ if you do not already have an account.
2. Authorize it to access GitHub Repos.
3. In the `home page`, select the  `repository` you want to build from the left hand side menu.
4. Click on the `more` option.
5. Click on `settings` option.
6. Scroll down to `Environment Variables` section.
7. If you look at the `travis.yml` file you will see two `environment variables` are being used to push the images to `Docker Hub` after successful build. 
8. Add these two `environment variables` to `travis-ci` to authorize it to push the images to your `Docker Hub`.
9. Make sure to give the `key names` as present in `travis.yml` file - `DOCKER_ID` and `DOCKER_PASSWORD`. In place of `DOCKER_PASSWORD` give the `access token` generated from `Docker Hub` in the earlier step.

> Now, when you push any changes to the your github repo, travis will automatically trigger a build, ruyn the tests and push the production version of the images to docker hub !!
