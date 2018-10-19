install:
	helm install . --name gitlab-ce --namespace default \
	--set portal.debug=true \
	# --dry-run --debug

del:
	helm del --purge gitlab-ce