ROOT_DIR := $(shell dirname $(realpath $(lastword $(MAKEFILE_LIST))))

IMAGE = lexty/lnmp
NAME = web
VOLUMES = -v $(ROOT_DIR)/www:/var/www:rw
PORTS = -p 80:80

.PHONY: all build run

all: build

build: Dockerfile
	docker build -t $(IMAGE) .

run:
	docker run $(VOLUMES) $(PORTS) --name $(NAME) -d $(IMAGE) /sbin/my_init

kill:
	docker rm -f $(NAME)

shell:
	docker exec -it $(NAME) bash

inspect:
	docker inspect $(NAME)

ip:
	docker inspect --format='{{.NetworkSettings.IPAddress}}' $(NAME)

ports:
	docker inspect --format='{{range $$p, $$conf := .NetworkSettings.Ports}} {{$$p}} -> {{(index $$conf 0).HostPort}} {{end}}' $(NAME)
