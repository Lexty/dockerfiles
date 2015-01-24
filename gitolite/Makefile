ROOT_DIR := $(shell dirname $(realpath $(lastword $(MAKEFILE_LIST))))

IMAGE = lexty/gitolite
NAME = git
VOLUMES = -v $(ROOT_DIR)/data/repositories:/home/git/repositories:rw -v $(ROOT_DIR)/data/.ssh:/home/git/.ssh:rw
PORTS = -p 65000:22
KEY = ~/.ssh/id_rsa.pub

.PHONY: all build run

all: build

build:
	docker build -t $(IMAGE) .

run:
	docker run --name $(NAME) $(VOLUMES) $(PORTS) -d $(IMAGE)

run2:
	docker run --name $(NAME) $(VOLUMES) $(PORTS) -d -e SSH_KEY="$$(cat $(KEY))" $(IMAGE)

kill:
	docker rm -f $(NAME)

shell:
	docker exec -it $(NAME) bash

ip:
	docker inspect --format='{{.NetworkSettings.IPAddress}}' $(NAME)

ports:
	docker inspect --format='{{range $$p, $$conf := .NetworkSettings.Ports}} {{$$p}} -> {{(index $$conf 0).HostPort}} {{end}}' $(NAME)
