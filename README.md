# RoadRunner - StackGres

kubectl create namespace monitoring
kubectl create namespace stackgres

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

helm install --namespace monitoring prometheus-operator prometheus-community/kube-prometheus-stack

helm install --namespace stackgres stackgres-operator \
    --set grafana.autoEmbed=true \
    --set-string grafana.webHost=prometheus-operator-grafana.monitoring \
    --set-string grafana.secretNamespace=monitoring \
    --set-string grafana.secretName=prometheus-operator-grafana \
    --set-string grafana.secretUserKey=admin-user \
    --set-string grafana.secretPasswordKey=admin-password \
    --set-string adminui.service.type=LoadBalancer \
stackgres-charts/stackgres-operator

kubectl port-forward pods/stackgres-restapi-XXXXXXXXXXXXXXXXX 8443:9443 -n stackgres