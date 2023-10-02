install:
	helm upgrade --cleanup-on-fail --install ruby-app . \
	--namespace ruby-app --create-namespace \
	--values values.yaml

uninstall:
	helm uninstall ruby-app --namespace ruby-app