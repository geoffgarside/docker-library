REGISTRY ?= geoffgarside
RELEASE  := $(shell head -n1 .version)

appdash-amd64: Dockerfile.amd64
	docker build -t $(REGISTRY)/$@:$(RELEASE) -f $< .

appdash-arm32: Dockerfile.arm32
	docker build -t $(REGISTRY)/$@:$(RELEASE) -f $< .

appdash-arm64: Dockerfile.arm64
	docker build -t $(REGISTRY)/$@:$(RELEASE) -f $< .

appdash: appdash-amd64 appdash-arm32 appdash-arm64

push: appdash
	docker push $(REGISTRY)/appdash-amd64:$(RELEASE)
	docker push $(REGISTRY)/appdash-arm64:$(RELEASE)
	docker push $(REGISTRY)/appdash-arm32:$(RELEASE)
	docker manifest create --amend $(REGISTRY)/appdash:$(RELEASE) \
		$(REGISTRY)/appdash-amd64:$(RELEASE) \
		$(REGISTRY)/appdash-arm64:$(RELEASE) \
		$(REGISTRY)/appdash-arm32:$(RELEASE)
	docker manifest annotate $(REGISTRY)/appdash:$(RELEASE) \
		$(REGISTRY)/appdash-amd64:$(RELEASE) \
		--os linux --arch amd64
	docker manifest annotate $(REGISTRY)/appdash:$(RELEASE) \
		$(REGISTRY)/appdash-arm64:$(RELEASE) \
		--os linux --arch arm64
	docker manifest annotate $(REGISTRY)/appdash:$(RELEASE) \
		$(REGISTRY)/appdash-arm32:$(RELEASE) \
		--os linux --arch arm
	docker manifest push $(REGISTRY)/appdash:$(RELEASE)

# Version Management
bump-major:
	@echo "$(RELEASE)" | awk 'BEGIN { FS = "." };{ print $$1+1 ".0.0" }' > .version
	@cat .version

bump-minor:
	@echo "$(RELEASE)" | awk 'BEGIN { FS = "." };{ print $$1 "." $$2+1 ".0" }' > .version
	@cat .version

bump-patch:
	@echo "$(RELEASE)" | awk 'BEGIN { FS = "." };{ print $$1 "." $$2 "." $$3+1 }' > .version
	@cat .version