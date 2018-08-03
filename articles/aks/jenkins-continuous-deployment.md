---
title: Implementação contínua do Jenkins com Kubernetes no Azure Kubernetes Service
description: Como automatizar um processo de implementação contínua com Jenkins para implementar e atualizar uma aplicação em contentores no Kubernetes no Azure Kubernetes Service
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/26/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: a1a6799bc049fea829f8e32d12705e26e3a41dc0
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39425763"
---
# <a name="continuous-deployment-with-jenkins-and-azure-kubernetes-service"></a>Implementação contínua com Jenkins e o Azure Kubernetes Service

Este documento demonstra como configurar um fluxo de trabalho de implementação contínua básica entre o Jenkins e um cluster do Azure Kubernetes Service (AKS).

O fluxo de trabalho de exemplo inclui os seguintes passos:

> [!div class="checklist"]
> * Implemente a aplicação de voto do Azure no seu cluster do Kubernetes.
> * Atualizar o código de aplicação de voto do Azure e enviar para um repositório do GitHub, que inicia o processo de implementação contínua.
> * Jenkins clona o repositório e cria uma nova imagem de contentor com o código atualizado.
> * Esta imagem é enviada por push para o Azure Container Registry (ACR).
> * O aplicativo em execução no cluster do AKS é atualizado com a nova imagem de contentor.

## <a name="prerequisites"></a>Pré-requisitos

Precisa dos seguintes itens para concluir os passos neste artigo.

- Noções básicas sobre Kubernetes, o Git, CI/CD e do Azure Container Registry (ACR).
- Uma [cluster do Azure Kubernetes Service (AKS)] [ aks-quickstart] e [credenciais do AKS configuradas] [ aks-credentials] no sistema de desenvolvimento.
- Uma [registo de Azure Container Registry (ACR)][acr-quickstart], o nome de servidor de início de sessão do ACR, e [credenciais ACR] [ acr-authentication] com acesso de push e pull.
- CLI do Azure instalada no sistema de desenvolvimento.
- Docker instalado no sistema de desenvolvimento.
- Conta do GitHub [token de acesso pessoal do GitHub][git-access-token]e o cliente de Git instalado no sistema de desenvolvimento.

## <a name="prepare-application"></a>Preparar aplicação

A aplicação de voto do Azure utilizada em todo este documento contém uma interface da web hospedada em pods de um ou mais e um segundo pod Redis de alojamento para o armazenamento de dados temporário.

Antes de criar o Jenkins / integração do AKS, preparar e implementar a aplicação de voto do Azure no seu cluster do AKS. Pense nisso como a versão da aplicação.

Bifurcar o repositório de GitHub seguinte.

```
https://github.com/Azure-Samples/azure-voting-app-redis
```

Assim que criou o fork, clone-sistema de desenvolvimento. Certifique-se de que está a utilizar o URL do seu fork quando este repositório de clonagem.

```bash
git clone https://github.com/<your-github-account>/azure-voting-app-redis.git
```

Altere os diretórios para trabalhar a partir do diretório clonado.

```bash
cd azure-voting-app-redis
```

Executar o `docker-compose.yaml` arquivo para criar o `azure-vote-front` imagem de contentor e iniciar a aplicação.

```bash
docker-compose up -d
```

Ao concluir, utilize o [imagens do docker] [ docker-images] comando para ver a imagem criada.

Repare que foram transferidas ou criadas três imagens. A imagem `azure-vote-front` contém a aplicação e utilizar a imagem `nginx-flask` como base. A imagem `redis` é utilizada para iniciar uma instância do Redis.

```console
$ docker images

REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Obter o servidor de início de sessão ACR com o [lista de acr az] [ az-acr-list] comando. Certifique-se atualizar o nome do grupo de recursos com o grupo de recursos que aloja o seu registo ACR.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Utilize o [etiqueta do docker] [ docker-tag] comando para etiquetar a imagem com o nome do servidor de início de sessão e um número de versão de `v1`.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

Atualizar o valor de servidor de início de sessão ACR com o nome de servidor de início de sessão do ACR, push e o `azure-vote-front` imagem no Registro.

```bash
docker push <acrLoginServer>/azure-vote-front:v1
```

## <a name="deploy-application-to-kubernetes"></a>Implementar aplicação no Kubernetes

Kubernetes com o arquivo de manifesto pode ser encontrado na raiz do repositório do Azure vote e pode ser utilizado para implementar a aplicação no seu cluster do Kubernetes.

Em primeiro lugar, atualize o **azure-vote-all-in-one-redis.yaml** arquivo de manifesto com a localização do seu registo ACR. Abra o ficheiro com qualquer editor de texto e substituir `microsoft` com o nome de servidor de início de sessão do ACR. Este valor foi encontrado na linha **47** do ficheiro de manifesto.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:v1
```

Em seguida, utilize o [aplicam-se de kubectl] [ kubectl-apply] comando para executar a aplicação. Este comando analisa o ficheiro de manifesto e cria os objetos de Kubernetes definidos.

```bash
kubectl apply -f azure-vote-all-in-one-redis.yaml
```

R [serviço do Kubernetes] [ kubernetes-service] é criado para expor a aplicação na internet. Este processo pode demorar alguns minutos.

Para monitorizar o progresso, utilize o comando [kubectl get service][kubectl-get] com o argumento `--watch`.

```bash
kubectl get service azure-vote-front --watch
```

Inicialmente, o *EXTERNAL-IP* do serviço *azure-vote-front* aparece como *pendente*.

```
azure-vote-front   10.0.34.242   <pending>     80:30676/TCP   7s
```

Quando o endereço *EXTERNAL-IP* mudar de *pendente* para *Endereço IP*, utilize `control+c` para parar o processo de observação do kubectl.

```
azure-vote-front   10.0.34.242   13.90.150.118   80:30676/TCP   2m
```

Para ver a aplicação, navegue para o endereço IP externo.

![Imagem do cluster do Kubernetes no Azure no Azure](media/aks-jenkins/azure-vote-safari.png)

## <a name="deploy-jenkins-to-vm"></a>Implementar o Jenkins para VM

Um script tiver sido previamente criado para implementar uma máquina virtual, configure o acesso de rede e concluir uma instalação básica do Jenkins. Além disso, o script copia o ficheiro de configuração de Kubernetes do seu sistema de desenvolvimento para o sistema do Jenkins. Este ficheiro é utilizado para autenticação entre o Jenkins e o cluster do AKS.

Execute os seguintes comandos para transferir e executar o script. O URL abaixo também pode ser utilizado para rever o conteúdo do script.

> [!WARNING]
> Este script de exemplo é para fins de demonstração aprovisionar rapidamente um ambiente Jenkins, que é executado numa VM do Azure. Utiliza a extensão de script personalizado do Azure para configurar uma VM e, em seguida, apresentar as credenciais necessárias. Sua *~/.kube/config* é copiado para a VM do Jenkins.

```console
curl https://raw.githubusercontent.com/Azure-Samples/azure-voting-app-redis/master/jenkins-tutorial/deploy-jenkins-vm.sh > azure-jenkins.sh
sh azure-jenkins.sh
```

Quando o script tiver concluído, produz um endereço para o servidor do Jenkins, como também a chave para desbloquear o Jenkins. Navegue para o URL, introduza a chave e seguir as mensagens no ecrã para concluir a configuração do Jenkins.

```console
Open a browser to http://52.166.118.64:8080
Enter the following to Unlock Jenkins:
667e24bba78f4de6b51d330ad89ec6c6
```

## <a name="jenkins-environment-variables"></a>Variáveis de ambiente Jenkins

Uma variável de ambiente Jenkins é utilizada para conter o nome de servidor de início de sessão do Azure Container Registry (ACR). Esta variável é referenciada durante a tarefa de implementação contínua do Jenkins.

Enquanto no portal de administrador do Jenkins, clique em **gerir Jenkins** > **configurar o sistema**.

Sob **propriedades globais**, selecione **variáveis de ambiente**e adicione uma variável com o nome `ACR_LOGINSERVER` e um valor do seu servidor de início de sessão do ACR.

![Variáveis de ambiente Jenkins](media/aks-jenkins/env-variables.png)

Quando terminar, clique em **guardar** na página de configuração do Jenkins.

## <a name="jenkins-credentials"></a>Credenciais do Jenkins

Agora armazene as suas credenciais ACR num objeto de credencial do Jenkins. Estas credenciais são referenciadas durante a tarefa de compilação do Jenkins.

Novamente no portal de administração do Jenkins, clique em **credenciais** > **Jenkins** > **Global de credenciais (ilimitado)**  >   **Adicionar credenciais**.

Certifique-se de que é o tipo de credencial **nome de utilizador com palavra-passe** e introduza os seguintes itens:

- **Nome de utilizador** -ID de principal de utilização do serviço para a autenticação com o registo do ACR.
- **Palavra-passe** -segredo do cliente da principal de utilização do serviço para a autenticação com o registo do ACR.
- **ID** -como o identificador de credencial `acr-credentials`.

Quando terminar, o formulário de credenciais deve ter um aspeto semelhante a esta imagem:

![Credenciais do ACR](media/aks-jenkins/acr-credentials.png)

Clique em **OK** e regresse ao portal de administrador do Jenkins.

## <a name="create-jenkins-project"></a>Criar projeto Jenkins

A partir do portal de administração do Jenkins, clique em **Novo Item**.

O projeto dê um nome ao, por exemplo `azure-vote`, selecione **projeto Freestyle**e clique em **OK**.

![Projeto Jenkins](media/aks-jenkins/jenkins-project.png)

Sob **gerais**, selecione **projeto GitHub** e introduza o URL para o fork do projeto GitHub do Azure vote.

![Projetos do GitHub](media/aks-jenkins/github-project.png)

Sob **gestão de código fonte**, selecione **Git**, introduza o URL para o fork do repositório do GitHub de voto do Azure.

Para as credenciais, clique em e **Add** > **Jenkins**. Sob **tipo**, selecione **texto secreto** e introduza o seu [token de acesso pessoal do GitHub] [ git-access-token] como o segredo.

Selecione **adicionar** quando tiver terminado.

![Credenciais do GitHub](media/aks-jenkins/github-creds.png)

Sob **criar Acionadores**, selecione **acionador de hook do GitHub para consulta GITScm**.

![Acionadores de compilação do Jenkins](media/aks-jenkins/build-triggers.png)

Sob **ambiente de compilação**, selecione **utilize textos secretos ou ficheiros**.

![Ambiente de compilação do Jenkins](media/aks-jenkins/build-environment.png)

Sob **enlaces**, selecione **Add** > **nome de utilizador e palavra-passe (separados)**.

Introduza `ACR_ID` para o **variável nome de utilizador**, e `ACR_PASSWORD` para o **variável de palavra-passe**.

![Enlaces do Jenkins](media/aks-jenkins/bindings.png)

Adicionar uma **passo de compilação** do tipo **executar shell** e utilize o seguinte texto. Este script cria uma nova imagem de contentor e envia-o para o seu registo ACR.

```bash
# Build new image and push to ACR.
WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
docker build -t $WEB_IMAGE_NAME ./azure-vote
docker login ${ACR_LOGINSERVER} -u ${ACR_ID} -p ${ACR_PASSWORD}
docker push $WEB_IMAGE_NAME
```

Adicione outro **passo de compilação** do tipo **executar shell** e utilize o seguinte texto. Este script atualiza a implementação de Kubernetes.

```bash
# Update kubernetes deployment with new image.
WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
kubectl set image deployment/azure-vote-front azure-vote-front=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
```

Depois de concluído, clique em **guardar**.

## <a name="test-the-jenkins-build"></a>Testar a compilação do Jenkins

Antes de continuar, teste a compilação do Jenkins. Isso valida que a tarefa de compilação foi configurada corretamente, o ficheiro de autenticação adequado do Kubernetes está no local e que as credenciais corretas do ACR foram fornecidas.

Clique em **compilar agora** no menu da esquerda do projeto.

![Compilação de teste do Jenkins](media/aks-jenkins/test-build.png)

Durante este processo, o repositório do GitHub for clonado para o servidor de compilação do Jenkins. Uma nova imagem de contentor é criada e enviada para o registo do ACR. Por fim, a aplicação de voto do Azure em execução no cluster do AKS é atualizada para utilizar a nova imagem. Uma vez que não foram efetuadas alterações ao código do aplicativo, o aplicativo não é alterado.

Depois do processo estiver concluído, clique em **criar #1** em histórico de compilações e selecione **saída da consola** para ver toda a saída do processo de compilação. A linha final deve indicar uma compilação com êxito.

## <a name="create-github-webhook"></a>Criar um webhook do GitHub

Em seguida, vincular o repositório da aplicação para o servidor de compilação do Jenkins, de forma que em qualquer consolidação, uma nova compilação é acionada.

1. Navegue para o repositório bifurcado do GitHub.
2. Selecione **configurações**, em seguida, selecione **Webhooks** no lado esquerdo.
3. Optar por **adicionar webhook**. Para o *URL de Payload*, introduza `http://<publicIp:8080>/github-webhook/` onde `publicIp` é o endereço IP do servidor Jenkins. Certifique-se incluir à direita /. Manter os padrões para o tipo de conteúdo e para disparar *push* eventos.
4. Selecione **adicionar webhook**.

    ![Webhook do GitHub](media/aks-jenkins/webhook.png)

## <a name="test-cicd-process-end-to-end"></a>Teste o processo de CI/CD ponto a ponto

No computador de desenvolvimento, abra a aplicação clonada com um editor de código.

Sob o **/azure-vote/azure-vote** directory, localizar um ficheiro com o nome **config_file.cfg**. Atualize os valores de voto neste ficheiro para algo que não sejam gatos e cães.

O exemplo seguinte mostra e atualizado **config_file.cfg** ficheiro.

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Quando terminar, guarde o ficheiro, confirme as alterações e distribuí-los para o fork do repositório do GitHub.... Depois de concluída a consolidação, o GitHub webhook aciona uma nova compilação Jenkins, que atualiza a imagem de contentor e a implementação do AKS. Monitorize o processo de compilação na consola do administrador do Jenkins.

Depois de concluída a compilação, navegue novamente para o ponto final da aplicação para observar as alterações.

![Do Azure vote atualizado](media/aks-jenkins/azure-vote-updated-safari.png)

Neste momento, um processo de implementação contínua simples foi concluído. Os passos e as configurações mostradas neste exemplo podem ser utilizadas para criar um automação de compilação contínua mais robusta e prontos para produção.

<!-- LINKS - external -->
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[git-access-token]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[az-acr-list]: /cli/azure/acr#az-acr-list
[acr-authentication]: ../container-registry/container-registry-auth-aks.md
[acr-quickstart]: ../container-registry/container-registry-get-started-azure-cli.md
[aks-credentials]: /cli/azure/aks#az-aks-get-credentials
[aks-quickstart]: kubernetes-walkthrough.md
[azure-cli-install]: /cli/azure/install-azure-cli