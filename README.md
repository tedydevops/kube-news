## Pipeline completa desafios 01, 02, 03, 04, 05 do projeto "INICIATIVA DEVOPS" do Fabricio Veronez
### dockerhub link: https://hub.docker.com/repository/docker/tedydevops/conversao-temperatura
PIPELINE CI/CD etapas:
# parte de CI
 1) criar cluster na nuvem: 
 - criar [arquivo terraform](https://github.com/tedydevops/kube-news/tree/main/iac) main.tf com especificações do cluster a ser gerado.(***lembrar de criar .gitignore pra só subir main.tf no Github para não expor informações sensíveis como token da nuvem presentes nos *.tfstate e *.tfvars***)
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
 #### - fazer no Github Actions o main.yaml do [workflows](https://github.com/tedydevops/kube-news/tree/main/.github/workflows) a parte de CI e:
 3) no Github Settings criar Secrets: 
  - DOCKERHUB_USER com seu usuário do dockerhub
  - DOCKERHUB_PWD com a respectiva senha

# parte de CD
 1) complementar o [workflows](https://github.com/tedydevops/kube-news/tree/main/.github/workflows) a parte de CD e:
 - criar Secrets: K8S_CONFIG  e dentro dele colar todo o conteudo do kubec_config.yaml
