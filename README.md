# RoadRunner - OpsPost

## StackGres Operator

kubectl create namespace monitoring
kubectl create namespace stackgres

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

helm install --namespace monitoring prometheus-operator prometheus-community/kube-prometheus-stack

helm repo add stackgres-charts https://stackgres.io/downloads/stackgres-k8s/stackgres/helm/

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

## Zalando Operator
# add repo for postgres-operator
helm repo add postgres-operator-charts https://opensource.zalando.com/postgres-operator/charts/postgres-operator

# install the postgres-operator
helm install postgres-operator postgres-operator-charts/postgres-operator

# add repo for postgres-operator-ui
helm repo add postgres-operator-ui-charts https://opensource.zalando.com/postgres-operator/charts/postgres-operator-ui

# install the postgres-operator-ui
helm install postgres-operator-ui postgres-operator-ui-charts/postgres-operator-ui

kubectl port-forward svc/postgres-operator-ui 8081:80