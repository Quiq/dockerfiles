VERSION=16.18.0
IMAGE=docker.quiq.sh/node:${VERSION}

.DEFAULT: buildx

build buildx:
	@../pre_buildx.sh
	@docker buildx build --build-arg VERSION=${VERSION} --platform linux/amd64,linux/arm64 -t ${IMAGE} -t $(subst sh,im,${IMAGE}) --push .
	@echo
	@echo "The image has been built: ${IMAGE}"
	@echo
