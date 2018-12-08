The difference between Dockerfile and Dockerfile.dev
Dockerfile => For production builds ( `npm run build`)
Dockerfile.dev => For starting local environment ( `npm run start` )

When we build it using `docker build .`; Docker searches for a Dockerfile and since we have a Dockerfile.dev
It gives an error saying `unable to prepare context: unable to evaluate symlinks in Dockerfile path: lstat /home/suraj/projects/docker-projects/frontend/Dockerfile: no such file or directory`

We can run the Dockerfile.dev using `sudo docker build -f Dockerfile.dev .`
So, our react-app already has tons of dependencies already installed inside node_modules folder.
Hence we can delete our local node_modules directory and rebuild the docker container using `sudo docker build -f Dockerfile.dev .`

Till now; we were doing `COPY . .`; We were copying all files from our local to our docker environment.
So, we create volumes in Docker which create reference between local and our Docker folder.

`sudo docker run -p 3000:30 -v $(pwd):/app ec25c36085c1`
Here, `-v $(pwd:/app)` means we are creating a volume under pwd/app with refernce to the contents in our local directory.
The above command gives us this error; `Local package.json exists, but node_modules missing, did you mean to install?`
This issue occurs because we want to create a reference of node_modules folder which is not inside Docker.
We can fix the missing node_modules issue by adding `-v /app/node_modules`. It adds a placeholder inside the folder and tells it to not map it with anything.

volumes:
  - /app/node_modules ( This means .. DO NOT try to map a folder against /app/node_modules folder )
  - .:/app ( Maps current folder to /app folder )
  build:
    context: . ( Location of current folder )
    dockerfile: Dockerfile.dev ( Location of docker file )  
`docker attach container-name` => Attaches terminal input with standard input, output and error of container.
