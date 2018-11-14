NAME=timescaledb
# ORG=timescale
ORG=ez123
PG_VER=pg10
VERSION=$(shell awk '/^ENV TIMESCALEDB_VERSION/ {print $$3}' Dockerfile)

default: image

.build_$(VERSION)_$(PG_VER): Dockerfile
ifeq ($(PG_VER),pg9.6)
	docker build --build-arg PG_VERSION=9.6.11 -t $(ORG)/$(NAME):latest-$(PG_VER)-bn .
	docker tag $(ORG)/$(NAME):latest-$(PG_VER)-bn $(ORG)/$(NAME):latest-bn
else
	docker build --build-arg PG_VERSION=10.6.0 -t $(ORG)/$(NAME):latest-$(PG_VER)-bn .
endif
	docker tag $(ORG)/$(NAME):latest-$(PG_VER)-bn $(ORG)/$(NAME):$(VERSION)-$(PG_VER)-bn
	touch .build_$(VERSION)_$(PG_VER)-bn

image: .build_$(VERSION)_$(PG_VER)

push: image
	docker push $(ORG)/$(NAME):$(VERSION)-$(PG_VER)-bn
	docker push $(ORG)/$(NAME):latest-$(PG_VER)-bn
ifeq ($(PG_VER),pg9.6)
	docker push $(ORG)/$(NAME):latest-bn
endif

clean:
	rm -f *~ .build_*

.PHONY: default image push clean
