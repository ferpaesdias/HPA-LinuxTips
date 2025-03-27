# HPA - LinuxTips

<br>

Criar, usando o Kubernetes, Pods com alta disponibilidade usando a imagem `ferpdias/linuxtips-giropops-senhas:1.0`.
Para a alta disponibilidade será usado o HPA (Horizontal Pod Autoscaler) e o para estressar os Pods será usado o Locust.  
O cluster Kubernetes será criado no MS Azure usando o repositório [Terraform-Azure-VMs-Kubernetes](https://github.com/ferpaesdias/Terraform-Azure-VMs-Kubernetes).

<br>

## Clonar repositório

Clone este repositório e acesse o diretório que foi criado

```shell
git clone https://github.com/ferpaesdias/HPA-LinuxTips.git

cd HPA-LinuxTips
```

<br>

## Giropops-Senhas

Primeiro vamos iniciar o Deployment e o Service (`ClusterIP`) do Redis:

```shell
kubectl apply -f redis-deployment-svc.yaml
```

<br>


Para iniciar o Deployment e o Service (`NodePort`) do app Giropops-Senhas execute o comando abaixo:

```shell
kubectl apply -f giropops-deployment-svc.yaml
```

<br>

Verifique se os Pods estão sendo executados:

```shell
kubectl get pods
```

<br>

A saída do comando deve ser semelhante a abaixo, como um Pod do Giropops-Senhas e um Pod do Redis em execução:

```shell
kubectl get pods
NAME                               READY   STATUS    RESTARTS   AGE
giropops-senhas-5db4765fc5-vrsk8   1/1     Running   0          9s
redis-974d9f5ff-s45w7              1/1     Running   0          31s
```

<br>

Verifique se os Services estão sendo executados:

```shell
kubectl get svc
```

<br>

A saída do comando deve ser semelhante a abaixo, como o Service `redis` do tipo `ClusterIP` e o Service `giropops-senhas` do tipo `NodePort` em execução:

```shell
kubectl get svc
NAME              TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
giropops-senhas   NodePort    10.97.26.238   <none>        5000:30100/TCP   6m55s
kubernetes        ClusterIP   10.96.0.1      <none>        443/TCP          121m
redis             ClusterIP   10.96.20.34    <none>        6379/TCP         12m
```

Como em meu Cluster eu não tenho um objeto Ingress configurado, eu optei por expor a porta do Giropops-Senhas usando um Service do tipo `NodePort`. A porta exposta e a `30100`.
 
<br>


