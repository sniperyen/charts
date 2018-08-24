PWD=$(shell pwd)

init-provisoner:
	kubectl apply -f devbox/storageclass.yaml
	kubectl apply -f devbox/hostpath-provisoner.yaml

# init-nginx:
# 	kubectl apply -f templates/nginx/configmap.yaml
# 	kubectl apply -f templates/nginx/http-backend.yaml
# 	kubectl apply -f templates/nginx/serviceaccount.yaml
# 	kubectl apply -f templates/nginx/ingress-controller.yaml

# without ingressController
install-in-devops:
	helm install . --name gitlab --namespace scm \
	--set externalUrl=https://alaudak8s-int.alauda.cn/gitlab \
	--set ingressController.enabled=false \
	--set ingress.enabled=true \
	--set ingress.url=alaudak8s-int.alauda.cn \
	--set ingress.path=/gitlab \
	--set serviceType=NodePort \
	--set gitlabRootPassword=07Apples \
	--set persistence.gitlabEtc.enabled=false \
	--set persistence.gitlabData.enabled=false \
	--set postgresql.persistence.enabled=false \
	--set redis.persistence.enabled=false

# with ingressController
install:
	helm install . --name gitlab --namespace scm \
	--set externalUrl=http://gitlab.codreamer.online \
	--set ingressController.enabled=true \
	--set ingress.enabled=true \
	--set ingress.url=gitlab.codreamer.online \
	--set ingress.path=/ \
	--set serviceType=NodePort \
	--set gitlabRootPassword=07Apples \
	--set persistence.gitlabEtc.enabled=false \
	--set persistence.gitlabData.enabled=false \
	--set postgresql.persistence.enabled=false \
	--set redis.persistence.enabled=false

debug:
	helm install --debug --dry-run . --name gitlab --namespace scm \
	--set externalUrl=http://gitlab.codreamer.online \
	--set ingressController.enabled=true \
	--set ingress.enabled=true \
	--set ingress.url=gitlab.codreamer.online \
	--set ingress.path=/ \
	--set serviceType=NodePort \
	--set gitlabRootPassword=07Apples \
	--set persistence.gitlabEtc.enabled=false \
	--set persistence.gitlabData.enabled=false \
	--set postgresql.persistence.enabled=false \
	--set redis.persistence.enabled=false

all:
	kubectl -n scm get deploy,pod,svc,pvc,pv,sa

del:
	helm del --purge gitlab

restart:
	kubectl -n scm scale deploy/nginx-ingress-controller --replicas=0
	kubectl -n scm scale deploy/nginx-ingress-controller --replicas=1
