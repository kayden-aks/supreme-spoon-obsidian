# Docker CMD

- Build image:

`docker build -t  [image_name]:[tag_name] -f [path/to/Dockerfile folder/contain/build/context]`

- Update image tag:

`docker tag [image_name]:[tag_name] [image_name]:[tag_name]`

- Inspect container info:

`docker inspect [container_name]`

- Log container info:

`docker logs [container_name] -f`

- Get into the container to debug:

`docker exec -it [container_name] bash`

- Check for connection between containers:

`docker exec [container_name_1] ping [container_name_2]`

- List networks:

`docker network`

