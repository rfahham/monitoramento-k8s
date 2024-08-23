# monitoramento-k8s

## Cluster local utilizando o K3D

Criando e acessando o diretório

```bash
mkdir k3d
cd k3d
```

## Install current latest release

```bash
curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
```

## Create a cluster named mycluster with just a single server node:

```bash
k3d cluster create dev
k3d cluster create qa
k3d cluster create prod

k3d cluster delete dev
```
## Verificando a criação do cluster

```bash
kubectx

k3d-dev
```

## Verificando os nodes

```bash
kubectl get nodes
NAME               STATUS   ROLES                  AGE   VERSION
k3d-dev-server-0   Ready    control-plane,master   15s   v1.30.3+k3s1
```

## Verificando os namespaces

```bash
kubens

default
kube-node-lease
kube-public
kube-system
```

## Fazer o clone do prometheus-operator

```bash
git clone https://github.com/prometheus-operator/kube-prometheus
```

## Criar os CRDs

Custon Resources Definitioms será criado no namespace monitoring

```bash
kubectl create -f manifests/setup/

customresourcedefinition.apiextensions.k8s.io/alertmanagerconfigs.monitoring.coreos.com created
customresourcedefinition.apiextensions.k8s.io/alertmanagers.monitoring.coreos.com created
customresourcedefinition.apiextensions.k8s.io/podmonitors.monitoring.coreos.com created
customresourcedefinition.apiextensions.k8s.io/probes.monitoring.coreos.com created
customresourcedefinition.apiextensions.k8s.io/prometheuses.monitoring.coreos.com created
customresourcedefinition.apiextensions.k8s.io/prometheusagents.monitoring.coreos.com created
customresourcedefinition.apiextensions.k8s.io/prometheusrules.monitoring.coreos.com created
customresourcedefinition.apiextensions.k8s.io/scrapeconfigs.monitoring.coreos.com created
customresourcedefinition.apiextensions.k8s.io/servicemonitors.monitoring.coreos.com created
customresourcedefinition.apiextensions.k8s.io/thanosrulers.monitoring.coreos.com created
namespace/monitoring created
```

```bash
kubectl get customresourcedefinition.apiextensions.k8s.io

NAME                                        CREATED AT
addons.k3s.cattle.io                        2024-08-23T14:59:11Z
alertmanagerconfigs.monitoring.coreos.com   2024-08-23T15:05:28Z
alertmanagers.monitoring.coreos.com         2024-08-23T15:05:28Z
etcdsnapshotfiles.k3s.cattle.io             2024-08-23T14:59:11Z
helmchartconfigs.helm.cattle.io             2024-08-23T14:59:11Z
helmcharts.helm.cattle.io                   2024-08-23T14:59:11Z
ingressroutes.traefik.containo.us           2024-08-23T14:59:50Z
ingressroutes.traefik.io                    2024-08-23T14:59:50Z
ingressroutetcps.traefik.containo.us        2024-08-23T14:59:50Z
ingressroutetcps.traefik.io                 2024-08-23T14:59:50Z
ingressrouteudps.traefik.containo.us        2024-08-23T14:59:50Z
ingressrouteudps.traefik.io                 2024-08-23T14:59:50Z
middlewares.traefik.containo.us             2024-08-23T14:59:50Z
middlewares.traefik.io                      2024-08-23T14:59:50Z
middlewaretcps.traefik.containo.us          2024-08-23T14:59:50Z
middlewaretcps.traefik.io                   2024-08-23T14:59:50Z
podmonitors.monitoring.coreos.com           2024-08-23T15:05:28Z
probes.monitoring.coreos.com                2024-08-23T15:05:28Z
prometheusagents.monitoring.coreos.com      2024-08-23T15:05:29Z
prometheuses.monitoring.coreos.com          2024-08-23T15:05:29Z
prometheusrules.monitoring.coreos.com       2024-08-23T15:05:29Z
scrapeconfigs.monitoring.coreos.com         2024-08-23T15:05:29Z
serverstransports.traefik.containo.us       2024-08-23T14:59:50Z
serverstransports.traefik.io                2024-08-23T14:59:50Z
serverstransporttcps.traefik.io             2024-08-23T14:59:50Z
servicemonitors.monitoring.coreos.com       2024-08-23T15:05:29Z
thanosrulers.monitoring.coreos.com          2024-08-23T15:05:29Z
tlsoptions.traefik.containo.us              2024-08-23T14:59:50Z
tlsoptions.traefik.io                       2024-08-23T14:59:50Z
tlsstores.traefik.containo.us               2024-08-23T14:59:50Z
tlsstores.traefik.io                        2024-08-23T14:59:50Z
traefikservices.traefik.containo.us         2024-08-23T14:59:50Z
traefikservices.traefik.io                  2024-08-23T14:59:50Z
```

Mudar para o namespace monitoring

```bash
kubens monitoring
```

## Criando os recursos

```bash
kubectl apply -f manifests

kubectl delete -f manifests
```

## Verificando os recursos criados

```bash
kubectl get pods

NAME                                   READY   STATUS    RESTARTS   AGE
alertmanager-main-0                    2/2     Running   0          35s
alertmanager-main-1                    2/2     Running   0          35s
alertmanager-main-2                    2/2     Running   0          35s
blackbox-exporter-597d86cf5c-ks2lc     3/3     Running   0          55s
grafana-549c49555-6b2nn                1/1     Running   0          55s
kube-state-metrics-789f4b647d-bx4jg    3/3     Running   0          54s
node-exporter-7cvs4                    2/2     Running   0          54s
prometheus-adapter-5794d7d9f5-bbw2h    1/1     Running   0          54s
prometheus-adapter-5794d7d9f5-srvwx    1/1     Running   0          54s
prometheus-k8s-0                       2/2     Running   0          35s
prometheus-k8s-1                       2/2     Running   0          35s
prometheus-operator-7545b58fd7-fqplj   2/2     Running   0          53s
```

## Verificando os Services

```bash
kubectl get svc
NAME                    TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
alertmanager-main       ClusterIP   10.43.207.4     <none>        9093/TCP,8080/TCP            2m9s
alertmanager-operated   ClusterIP   None            <none>        9093/TCP,9094/TCP,9094/UDP   109s
blackbox-exporter       ClusterIP   10.43.78.117    <none>        9115/TCP,19115/TCP           2m9s
grafana                 ClusterIP   10.43.102.101   <none>        3000/TCP                     2m9s
kube-state-metrics      ClusterIP   None            <none>        8443/TCP,9443/TCP            2m8s
node-exporter           ClusterIP   None            <none>        9100/TCP                     2m8s
prometheus-adapter      ClusterIP   10.43.21.168    <none>        443/TCP                      2m8s
prometheus-k8s          ClusterIP   10.43.196.35    <none>        9090/TCP,8080/TCP            2m8s
prometheus-operated     ClusterIP   None            <none>        9090/TCP                     109s
prometheus-operator     ClusterIP   None            <none>        8443/TCP                     2m7s
```

## Acessando Prometheus e o Grafana

Prometheus

```bash
kubectl port-forward svc/prometheus-k8s 9090:9090
```

Acessar o Prometheus

localhost:9090

Grafana

```bash
kubectl port-forward svc/grafana 3000:3000
```

Acessar o Prometheus

localhost:3000

http://prometheus-k8s.monitoring.svc:9090

## Para acessar os dados

No Grafana procurar por Dashboards

Escolher o serviço que será monitorado


!!! Está dando erro de plugin

## Documentação

https://github.com/badtuxx/DescomplicandoPrometheus

https://livro.descomplicandoprometheus.com.br/index.html