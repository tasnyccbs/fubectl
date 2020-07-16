IMAGE := k8s-test
REGISTRY ?= msgongora
TAG ?= latest

REPO_URL = $(shell git remote get-url origin)
BRANCH := $(shell git branch|sed -n 's/\* \(.*\)/\1/p')
GITREV_REMOTE = $(shell git ls-remote $(REPO_URL)|grep "/$(BRANCH)$$"|cut -f1|cut -c1-8)
GITREV = $(shell git rev-parse --short HEAD)

.PHONY: help build push clean

.DEFAULT: help

help:
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; \
		{printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}'


build: ## Build the docker image
	docker build -t $(REGISTRY)/$(IMAGE):$(TAG) \
		$(BUILD_ARG) .

push: build ## Push the docker image to the registry.
	docker push $(REGISTRY)/$(IMAGE):$(TAG)

build-git: TAG = $(GITREV) ## Build the docker image using git sha as TAG.
build-git: build
push-git: TAG = $(GITREV) ## Push the docker image to the registry (TAG=git sha)
push-git: push

clean: ## Delete current docker image.
	docker rmi -f $(REGISTRY)/$(IMAGE):$(TAG) 2>/dev/null || true

img-git: ## Present the docker image name using git sha as tag.
	@echo $(REGISTRY)/$(IMAGE):$(GITREV)

img: ## Shows the docker image to be build.
	@echo $(REGISTRY)/$(IMAGE):$(TAG)