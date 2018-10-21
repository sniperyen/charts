install:
	helm install . --name gitlab-ce --namespace default \
	--set portal.debug=true \
	--set gitlabHost=119.28.187.185 \
	--set gitlabRootPassword=Gitlab12345 \
	--set service.ports.http.nodePort=31001 \
	--set service.ports.ssh.nodePort=31002 \
	# --dry-run --debug

install-with-domain:
	helm install . --name gitlab-ce --namespace default \
	--set portal.debug=true \
	--set ingress.enabled=true \
	--set ingress.hosts.portal=gitlab.codreamer.online \
	--set gitlabRootPassword=Gitlab12345 \
	# --dry-run --debug

del:
	helm del --purge gitlab-ce