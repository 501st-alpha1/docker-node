.PHONY: all push

USER="501stalpha1"
ifeq ($(shell uname -m), i686)
  PLATFORM="386"
else
  PLATFORM="amd64"
endif
FULL_VERSION="7.7.2"
MINOR_VERSION=$(shell echo $(FULL_VERSION) | cut -d '.' -f -2)
MAJOR_VERSION=$(shell echo $(FULL_VERSION) | cut -d '.' -f 1)

all: .last-docker-build

.last-docker-build: Dockerfile
	docker build -t "$(USER)/node:$(FULL_VERSION)-$(PLATFORM)" .
	docker tag "$(USER)/node:$(FULL_VERSION)-$(PLATFORM)" "$(USER)/node:$(MINOR_VERSION)-$(PLATFORM)"
	docker tag "$(USER)/node:$(FULL_VERSION)-$(PLATFORM)" "$(USER)/node:$(MAJOR_VERSION)-$(PLATFORM)"
	docker tag "$(USER)/node:$(FULL_VERSION)-$(PLATFORM)" "$(USER)/node:latest-$(PLATFORM)"
	@touch $@

push: .last-docker-push

.last-docker-push: .last-docker-build
	docker push "$(USER)/node:$(FULL_VERSION)-$(PLATFORM)"
	manifest-tool push from-spec ./manifest-full.yml
	docker push "$(USER)/node:$(MINOR_VERSION)-$(PLATFORM)"
	manifest-tool push from-spec ./manifest-minor.yml
	docker push "$(USER)/node:$(MAJOR_VERSION)-$(PLATFORM)"
	manifest-tool push from-spec ./manifest-major.yml
	docker push "$(USER)/node:latest-$(PLATFORM)"
	@touch $@
