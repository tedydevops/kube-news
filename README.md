## Pipeline completa do projeto "INICIATIVA DEVOPS" do Fabricio Veronez
### minha dockerhub: https://hub.docker.com/repository/docker/tedydevops/conversao-temperatura
PIPELINE CI/CD etapas:
# parte de CI
 1) criar cluster na nuvem (no caso é a Digitalocean): 
 - criar [arquivo terraform](https://github.com/tedydevops/kube-news/tree/main/iac) main.tf com especificações do cluster a ser gerado.
 - na pasta com os arquivos de iac de terraform:
~~~linux
$ terraform init
$ terraform apply
~~~
 2) para interagir com o cluster: 
 - copiar configurações de acesso kube_config.yaml para configurações do cluster local:
~~~linux
$ cp ./kube_config.yaml ~/.kube/config
~~~
 3) fazer no Github Actions o main.yaml do [workflows](https://github.com/tedydevops/kube-news/tree/main/.github/workflows) a parte de CI
 4) no Github Settings criar Secrets: 
  - DOCKERHUB_USER com seu usuário do dockerhub
  - DOCKERHUB_PWD com a respectiva senha
### 5) *lembrar de criar .gitignore pra só subir main.tf no Github para não expor informações sensíveis como token da nuvem, presente nos arquivos gerados *.tfstate e *.tfvars e o kube_config.yaml !!!
# parte de CD
 5) complementar o [workflows](https://github.com/tedydevops/kube-news/tree/main/.github/workflows) a parte de CD e:
 - criar Secrets: K8S_CONFIG  e dentro dele colar todo o conteudo do kubec_config.yaml

## Prometheus e Grafana

6) na pasta kube-news instalar pelo helm, o [Prometheus do artifacthub](https://artifacthub.io/packages/helm/prometheus-community/prometheus):
~~~linux
$ helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
$ helm repo update
$ helm upgrade --install prometheus prometheus-community/prometheus --set alertmanager.enabled=false,server.persistenVolume.enabled=false,server.service.type=LoadBalancer,server.global.scrape_interval=10s
~~~
7) inserir annotations no [k8s deployment.yaml](https://github.com/tedydevops/kube-news/tree/main/k8s):
- ![alt text](https://github.com/tedydevops/kube-news/blob/main/annotations.JPG)
8) aplicar mudançças no deployment.yaml :
 ~~~linux
$ kubectl apply -f deployment.yaml
~~~~
- obs.: caso queira acessar, abra pagina da internet e escreva "localhost:IP", substitua "IP" pelo EXTERNAL-IP do prometheus-server mostrado pelo comando: 
~~~linux
$ kubectl get svc
~~~
