---
title: Implementação contínua Jenkins com Kubernetes no serviço de Kubernetes do Azure
description: Como automatizar um processo de implementação contínua com Jenkins para implementar e atualizar uma aplicação no Kubernetes no serviço de Kubernetes do Azure
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/26/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 376d3b916c4e01ea6111e6c1db63e976dd1ea320
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/11/2018
ms.locfileid: "34069345"
---
# <a name="continuous-deployment-with-jenkins-and-azure-kubernetes-service"></a>Implementação contínua com Jenkins e o serviço de Kubernetes do Azure

Este documento demonstra como configurar um fluxo de trabalho básico de implementação contínua entre Jenkins e um cluster do serviço de Kubernetes do Azure (AKS).

O fluxo de trabalho de exemplo inclui os seguintes passos:

> [!div class="checklist"]
> * Implemente a aplicação de voto do Azure para o cluster Kubernetes.
> * Atualize o código de aplicação de voto do Azure e enviar para um repositório do GitHub, que inicia o processo de implementação contínua.
> * Jenkins clones repositório e cria uma nova imagem de contentor com o código atualizado.
> * Esta imagem é feita para registo de contentor do Azure (ACR).
> * A aplicação em execução no AKS cluster é atualizada com a nova imagem do contentor.

## <a name="prerequisites"></a>Pré-requisitos

Terá dos itens seguintes para concluir os passos neste artigo.

- Compreensão básica do Kubernetes, o Git, o CI/CD e o registo de contentor do Azure (ACR).
- Um [cluster do serviço de Kubernetes do Azure (AKS)] [ aks-quickstart] e [credenciais AKS configuradas] [ aks-credentials] no sistema de desenvolvimento.
- Um [registo de registo de contentor do Azure (ACR)][acr-quickstart], o nome do servidor ACR início de sessão, e [credenciais ACR] [ acr-authentication] com acesso de push e pull.
- CLI do Azure instalado no seu sistema de desenvolvimento.
- Docker instalado no seu sistema de desenvolvimento.
- Conta do GitHub, [token de acesso pessoal GitHub][git-access-token]e o Git instalados no sistema de desenvolvimento.

## <a name="prepare-application"></a>Preparar a aplicação

A aplicação de voto do Azure utilizada em todo este documento contém uma interface web alojada no pods de um ou mais e uma segunda pod Redis de alojamento para o armazenamento de dados temporária.

Antes de criar o Jenkins / integração AKS, preparar e implementar a aplicação de voto do Azure para o cluster AKS. Considere este como versão da aplicação.

Copiar o repositório do GitHub seguinte.

```
https://github.com/Azure-Samples/azure-voting-app-redis
```

Quando tiver sido criada a bifurcação, clone-o sistema de desenvolvimento. Certifique-se de que está a utilizar o URL da sua bifurcação ao clonar este repositório.

```bash
git clone https://github.com/<your-github-account>/azure-voting-app-redis.git
```

Altere os diretórios para trabalhar a partir do diretório clonado.

```bash
cd azure-voting-app-redis
```

Execute o `docker-compose.yaml` ficheiros para criar o `azure-vote-front` imagem contentor e inicie a aplicação.

```bash
docker-compose up -d
```

Quando tiver terminado, utilize o [imagens docker] [ docker-images] comando para ver a imagem de criação.

Repare que foram transferidas ou criadas três imagens. A imagem `azure-vote-front` contém a aplicação e utilizar a imagem `nginx-flask` como base. A imagem `redis` é utilizada para iniciar uma instância do Redis.

```console
$ docker images

REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Obter o servidor de início de sessão ACR com o [lista de acr az] [ az-acr-list] comando. Certifique-se de que atualiza o nome do grupo de recursos com o grupo de recursos que aloja o seu registo ACR.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Utilize o [tag de docker] [ docker-tag] comando para marcar a imagem com o nome do servidor de início de sessão e um número de versão de `v1`.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

Atualize o valor de servidor de início de sessão ACR com o nome do servidor de início de sessão ACR e push o `azure-vote-front` imagem ao registo.

```bash
docker push <acrLoginServer>/azure-vote-front:v1
```

## <a name="deploy-application-to-kubernetes"></a>Implementar a aplicação Kubernetes

Um Kubernetes ficheiro de manifesto pode ser localizado na raiz do repositório de voto do Azure e pode ser utilizado para implementar a aplicação em Kubernetes cluster.

Em primeiro lugar, atualize o **azure-vote-all-in-one-redis.yaml** ficheiro de manifesto com a localização do seu registo ACR. Abra o ficheiro com qualquer editor de texto e substitua `microsoft` com o nome de servidor de início de sessão ACR. Este valor foi encontrado na linha **47** do ficheiro de manifesto.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:v1
```

Em seguida, utilize o [kubectl aplicar] [ kubectl-apply] comando a executar a aplicação. Este comando analisa o ficheiro de manifesto e cria os objetos de Kubernetes definidos.

```bash
kubectl apply -f azure-vote-all-in-one-redis.yaml
```

A [Kubernetes serviço] [ kubernetes-service] é criado para expor a aplicação à internet. Este processo pode demorar alguns minutos.

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

## <a name="deploy-jenkins-to-vm"></a>Implementar Jenkins para VM

Um script foi previamente criado para implementar uma máquina virtual, configurar o acesso de rede e concluir uma instalação de Jenkins básica. Além disso, o script copia o ficheiro de configuração Kubernetes do sistema de desenvolvimento para o sistema Jenkins. Este ficheiro é utilizado para autenticação entre Jenkins e o cluster AKS.

Execute os seguintes comandos para transferir e executar o script. O abaixo URL também pode ser utilizado para rever o conteúdo do script.

```console
curl https://raw.githubusercontent.com/Azure-Samples/azure-voting-app-redis/master/jenkins-tutorial/deploy-jenkins-vm.sh > azure-jenkins.sh
sh azure-jenkins.sh
```

Quando o script tiver concluído, produz um endereço para o servidor de Jenkins, como também uma chave para desbloquear Jenkins. Navegar para o URL, introduza a chave e seguir os avisos no ecrã para concluir a configuração de Jenkins.

```console
Open a browser to http://52.166.118.64:8080
Enter the following to Unlock Jenkins:
667e24bba78f4de6b51d330ad89ec6c6
```

## <a name="jenkins-environment-variables"></a>Variáveis de ambiente de Jenkins

Uma variável de ambiente de Jenkins é utilizada para conter o nome do servidor de início de sessão de registo de contentor do Azure (ACR). Esta variável é referenciada durante a tarefa de implementação contínua Jenkins.

Enquanto no portal de administração Jenkins, clique em **gerir Jenkins** > **Configurar sistema**.

Em **propriedades globais**, selecione **variáveis de ambiente**e adicione uma variável com o nome `ACR_LOGINSERVER` e um valor do seu servidor de início de sessão ACR.

![Variáveis de ambiente de Jenkins](media/aks-jenkins/env-variables.png)

Quando terminar, clique em **guardar** na página de configuração Jenkins.

## <a name="jenkins-credentials"></a>Jenkins credenciais

Agora armazene as suas credenciais ACR num objeto de credencial Jenkins. Estas credenciais são referenciadas durante a tarefa de compilação Jenkins.

Novamente no portal de administração Jenkins, clique em **credenciais** > **Jenkins** > **Global de credenciais (sem restrições)**  >   **Adicionar credenciais**.

Certifique-se de que o tipo de credencial **nome de utilizador com palavra-passe** e introduza os seguintes itens:

- **Nome de utilizador** -ID a utilização de principal de serviço para a autenticação com o registo ACR.
- **Palavra-passe** -segredo do cliente da utilização de principal de serviço para a autenticação com o registo ACR.
- **ID** -tais como o identificador de credencial `acr-credentials`.

Quando terminar, o formulário de credenciais deve ter um aspeto semelhante a esta imagem:

![Credenciais ACR](media/aks-jenkins/acr-credentials.png)

Clique em **OK** e regresse ao portal de administração do Jenkins.

## <a name="create-jenkins-project"></a>Criar projeto Jenkins

No portal de administração Jenkins, clique em **Novo Item**.

Dê um nome, o projeto de exemplo por `azure-vote`, selecione **Freestyle projeto**e clique em **OK**.

![Jenkins projeto](media/aks-jenkins/jenkins-project.png)

Em **geral**, selecione **GitHub projeto** e introduza o URL para o repositório de cópia do projeto de GitHub de voto do Azure.

![Projeto de GitHub](media/aks-jenkins/github-project.png)

Em **gestão da origem de código**, selecione **Git**, introduza o URL para o repositório de cópia do repositório GitHub de voto do Azure.

As credenciais do utilizador, clique em e **adicionar** > **Jenkins**. Em **tipo**, selecione **texto secreto** e introduza o [token de acesso pessoal GitHub] [ git-access-token] como o segredo.

Selecione **adicionar** quando terminar.

![Credenciais do GitHub](media/aks-jenkins/github-creds.png)

Em **criar Acionadores**, selecione **acionador de rotina do GitHub para consulta GITScm**.

![Jenkins criar acionadores](media/aks-jenkins/build-triggers.png)

Em **criar ambiente**, selecione **utilizar textos secretos ou ficheiros**.

![Ambiente de compilação Jenkins](media/aks-jenkins/build-environment.png)

Em **enlaces**, selecione **adicionar** > **nome de utilizador e palavra-passe (separados)**.

Introduza `ACR_ID` para o **variável nome de utilizador**, e `ACR_PASSWORD` para o **variável de palavra-passe**.

![Jenkins enlaces](media/aks-jenkins/bindings.png)

Adicionar um **passo Criar** do tipo **executar shell** e utilize o seguinte texto. Este script cria uma nova imagem de contentor e envia-la ao seu registo ACR.

```bash
# Build new image and push to ACR.
WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
docker build -t $WEB_IMAGE_NAME ./azure-vote
docker login ${ACR_LOGINSERVER} -u ${ACR_ID} -p ${ACR_PASSWORD}
docker push $WEB_IMAGE_NAME
```

Adicionar outro **passo Criar** do tipo **executar shell** e utilize o seguinte texto. Este script de implementação de Kubernetes de atualizações.

```bash
# Update kubernetes deployment with new image.
WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
kubectl set image deployment/azure-vote-front azure-vote-front=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
```

Depois de concluída, clique em **guardar**.

## <a name="test-the-jenkins-build"></a>Testar a compilação de Jenkins

Antes de continuar, teste a compilação de Jenkins. Esta ação valida que a tarefa de compilação foi configurada corretamente, é o ficheiro de autenticação Kubernetes adequado no local e que as credenciais corretas de ACR foram fornecidas.

Clique em **compilar agora** no menu da esquerda do projeto.

![Compilação de teste de Jenkins](media/aks-jenkins/test-build.png)

Durante este processo, é clonar o repositório do GitHub para o servidor de compilação Jenkins. Uma nova imagem do contentor é criada e enviada para o registo ACR. Por fim, a aplicação de voto do Azure em execução no AKS cluster é atualizada para utilizar a nova imagem. Porque não foram efetuadas alterações ao código da aplicação, a aplicação não é alterada.

Depois do processo estiver concluído, clique em **criar #1** em compilar o histórico e selecione **resultado da consola** para ver todos os resultados do processo de compilação. A linha final deve indicar uma compilação efetuada com êxito.

## <a name="create-github-webhook"></a>Criar um webhook do GitHub

Em seguida, ligue o repositório da aplicação para o servidor de compilação Jenkins para que em qualquer confirmação, é acionada uma nova compilação.

1. Navegue para o repositório do GitHub forked.
2. Selecione **Definições** e selecione **Integrações e serviços** no lado esquerdo.
3. Escolha **Adicionar serviço**, introduza `Jenkins (GitHub plugin)` na caixa do filtro e selecione o plug-in.
4. Para os Jenkins ligue URL, introduza `http://<publicIp:8080>/github-webhook/` onde `publicIp` é o endereço IP do servidor Jenkins. Certifique-se de que incluiu /.
5. Selecione Adicionar serviço.

![Webhook do GitHub](media/aks-jenkins/webhook.png)

## <a name="test-cicd-process-end-to-end"></a>Teste o processo de CI/CD ponto a ponto

No computador de desenvolvimento, abra a aplicação clonada com um editor de código.

Sob o **/azure-vote/azure-vote** diretório, localizar um ficheiro com o nome **config_file.cfg**. Atualize os valores de voto neste ficheiro para algo que não sejam cats e dogs.

O exemplo seguinte mostra e atualizado **config_file.cfg** ficheiro.

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Quando terminar, guarde o ficheiro, consolidar as alterações e push para o repositório de cópia do repositório GitHub... Depois de concluída a consolidação, o webhook GitHub aciona uma nova compilação Jenkins, que atualiza a imagem de contentor e a implementação de AKS. Monitorize o processo de compilação na consola de administração do Jenkins.

Depois de concluída a compilação, procure novamente para o ponto final da aplicação para observar as alterações.

![Azure voto atualizado](media/aks-jenkins/azure-vote-updated-safari.png)

Neste momento, um processo de implementação contínua simples foi concluído. Os passos e configurações mostradas neste exemplo, podem ser utilizadas para criar a saída de uma automatização de compilação contínua mais robusto e prontos para produção.

<!-- LINKS - external -->
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[git-access-token]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[az-acr-list]: /cli/azure/acr#az_acr_list
[acr-authentication]: ../container-registry/container-registry-auth-aks.md
[acr-quickstart]: ../container-registry/container-registry-get-started-azure-cli.md
[aks-credentials]: /cli/azure/aks#az_aks_get_credentials
[aks-quickstart]: kubernetes-walkthrough.md
[azure-cli-install]: /cli/azure/install-azure-cli