OS=linux

all:
	kubectl -n jenkins get deploy,pod,svc,secret,pvc,ingress

install:
	helm install . --name jenkins --namespace jenkins \
	--set rootPassword=07Apples \
	--set serviceType=NodePort \
	--set persistence.enabled=true \
	--set ingress.enabled=false \
	--set ingress.url=jenkins.codreamer.online \
	--set ingress.path=/

upgrade:
	helm upgrade jenkins . \
	--set persistence.enabled=true

del:
	helm del --purge jenkins

restart:
	kubectl -n default-1 scale deploy jenkins --replicas=0 && \
	kubectl -n default-1 scale deploy jenkins --replicas=1
