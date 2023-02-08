VERSION=3.10.5
MAJOR_VERSION=`echo ${VERSION}| cut -d. -f1-2`
DATE=`date "+%Y%m%d"`
IMAGE=docker.quiq.sh/python
PUBLIC_IMAGE=quiq/python

.DEFAULT: buildx

build:
	@docker build -t ${IMAGE}:${VERSION} .
	@echo
	@echo "The image has been built: ${IMAGE}:${VERSION}"
	@echo

buildx:
	@../pre_buildx.sh
	@docker buildx build --platform linux/amd64,linux/arm64 -t ${IMAGE}:${VERSION} -t $(subst sh,im,${IMAGE}:${VERSION}) -t ${IMAGE}:${MAJOR_VERSION} -t $(subst sh,im,${IMAGE}:${MAJOR_VERSION}) -t ${IMAGE}:${DATE} -t $(subst sh,im,${IMAGE}:${DATE}) --build-arg https_proxy --push .
	@echo
	@echo "The image has been built: ${IMAGE}:${VERSION}"
	@echo
