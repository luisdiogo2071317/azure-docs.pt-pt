---
title: Implementar para Azure Kubernetes Service (AKS) com o Jenkins e o padrão de implementação de azul/verde
description: Saiba como implementar para Azure Kubernetes Service (AKS) com o Jenkins e o padrão de implementação de azul/verde
services: app-service\web
documentationcenter: ''
author: tomarcher
manager: jpconnock
editor: ''
ms.assetid: ''
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 07/23/2018
ms.author: tarcher
ms.custom: jenkins
ms.openlocfilehash: 472622f78303593b7a4d5d5136aa47f34a1f44b1
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228191"
---
# <a name="deploy-to-azure-kubernetes-service-aks-using-jenkins-and-bluegreen-deployment-pattern"></a>Implementar para Azure Kubernetes Service (AKS) com o Jenkins e o padrão de implementação de azul/verde

O Azure Kubernetes Service (AKS) faz a gestão do seu ambiente alojado do Kubernetes, permitindo implementar e gerir rápida e facilmente aplicações contentorizadas sem que sejam necessários conhecimentos em termos de orquestração de contentores. AKS também elimina o fardo de operações e manutenção contínuas ao aprovisionar, atualizar e dimensionar recursos a pedido, sem colocar as suas aplicações offline. Para obter mais informações sobre o AKS, consulte para o [documentação AKS](/azure/aks/).

Implementação de azul/verde é um padrão de entrega contínua de DevOps (CD) que se baseia em manter uma versão (azul) existente em direto quando um novo (verde) está a ser implementado. Normalmente, esse padrão emprega para direcionar o aumento de quantidades de tráfego para a implementação de verde de balanceamento de carga. Se a monitorização Deteta um incidente, o tráfego pode ser reencaminhado para a implementação de azul ainda em execução. Para obter mais informações sobre a entrega contínua, consulte o artigo [o que é a entrega contínua](/azure/devops/what-is-continuous-delivery).

Neste tutorial, saiba como realizar as seguintes tarefas em aprender a implementar no AKS com o Jenkins e o padrão de implementação de azul/verde:

> [!div class="checklist"]
> * Saiba mais sobre o padrão de implementação de azul/verde
> * Crie um cluster de Kubernetes gerido.
> * Executar um script de exemplo para configurar um cluster do Kubernetes
> * Configurar manualmente um cluster do Kubernetes
> * Criar e executar uma tarefa do Jenkins

## <a name="prerequisites"></a>Pré-requisitos
- [Conta do GitHub](https://github.com) : precisa de uma conta do GitHub para clonar o repositório de exemplo.
- [CLI 2.0 do Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) : A CLI 2.0 do Azure é utilizado para criar o cluster de Kubernetes.
- [Chocolatey](https://chocolatey.org) -um Gestor de pacotes utilizado para instalar o kubectl.
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) : uma interface de linha de comandos para executar comandos em relação a clusters do Kubernetes.
- [jq](https://stedolan.github.io/jq/download/) : um processador JSON de linha de comandos simples.

## <a name="clone-the-sample-app-from-github"></a>Clone a aplicação de exemplo do GitHub

É uma aplicação de exemplo que ilustra como implementar no AKS com o Jenkins e o padrão de azul/verde no repositório Microsoft no GitHub. Nesta secção, criar um fork do repositório do que no seu GitHub e clone a aplicação ao seu sistema local.

1. Navegue para o repositório do GitHub para o [todo-aplicação-java-no azure](https://github.com/microsoft/todo-app-java-on-azure.git) aplicação de exemplo.

    ![Aplicação de exemplo no repositório do GitHub de Microsoft.](./media/jenkins-aks-blue-green-deployment/github-sample-msft.png)

1. Bifurcar o repositório, selecionando **Fork** no canto superior direito da página e siga as instruções para bifurcar o repositório na sua conta do GitHub.

    ![A aplicação de exemplo para a sua conta do GitHub do fork.](./media/jenkins-aks-blue-green-deployment/github-sample-msft-fork.png)

1. Depois de bifurcar o repositório, verá que o nome da conta é alterado para o seu nome de conta e uma observação indica de onde foi bifurcou repositório (Microsoft).

    ![Exemplo de aplicação depois de a ser bifurcou para outra conta do GitHub.](./media/jenkins-aks-blue-green-deployment/github-sample-msft-forked.png)

1. Selecionando **clonar ou transferir**.

    ![GitHub permite-lhe rapidamente clonar ou transferir um repositório.](./media/jenkins-aks-blue-green-deployment/github-sample-clone.png)

1. Na **Clone com HTTPS** janela, selecione o ícone de cópia.

    ![Copie o URL do clone para a área de transferência.](./media/jenkins-aks-blue-green-deployment/github-sample-copy.png)

1. Abra um terminal ou janela de Bash.

1. Altere os diretórios para a localização pretendida, onde pretende armazenar a cópia local (clona) do repositório.

1. Usando o `git clone` comando este problema, clone o URL que copiou anteriormente.

    ![Escreva "clone de git" e o URL do clone para criar um clone do repositório.](./media/jenkins-aks-blue-green-deployment/git-clone-command.png)

1. Selecione o &lt;Enter > tecla para iniciar o processo de clonagem.

    ![O comando "clone de git" cria uma cópia do repositório na qual pode testar](./media/jenkins-aks-blue-green-deployment/git-clone-results.png)

1. Altere os diretórios para o diretório recém-criado, que contém o clone da origem de aplicação.

## <a name="create-and-configure-a-managed-kubernetes-cluster"></a>Criar e configurar um cluster de Kubernetes gerido

Nesta secção, execute os seguintes passos:

- Utilize a CLI 2.0 do Azure para criar um cluster de Kubernetes gerido.
- Saiba como configurar um cluster através da utilização o script de configuração ou manualmente.
- Crie o registo de contentor do Azure.

> [!NOTE]   
> AKS está atualmente em pré-visualização. Para obter informações sobre como ativar a pré-visualização para a sua subscrição do Azure. ver [início rápido: implementar um cluster do Azure Kubernetes Service (AKS) ](/azure/aks/kubernetes-walkthrough#enabling-aks-preview-for-your-azure-subscription) para obter mais detalhes.

### <a name="use-the-azure-cli-20-to-create-a-managed-kubernetes-cluster"></a>Utilizar a CLI 2.0 do Azure para criar um cluster de Kubernetes gerido
Para criar um cluster de Kubernetes gerido com [CLI 2.0 do Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest), certifique-se de que está a utilizar a CLI do Azure versão 2.0.25 ou posterior.

1. Inicie sessão na sua conta do Azure. Depois de digitar o seguinte `az login` de comando, são fornecidas instruções que explicam como concluir o início de sessão. 
    
    ```bash
    az login
    ```

1. Quando executa o `az login` comando no passo anterior, uma lista de todas as suas subscrições do Azure apresenta (juntamente com suas IDs de subscrição). Neste passo, defina a predefinição de subscrição do Azure. Substitua o &lt;your subscription-id > espaço reservado com o ID de subscrição do Azure pretendida. 

    ```bash
    az account set -s <your-subscription-id>
    ```

1. Crie um grupo de recursos. Substitua a &lt;your---nome do grupo recursos > marcador de posição pelo nome do seu novo grupo de recursos e substitua o &lt;sua localização > marcador de posição pela localização da. O comando `az account list-locations` apresenta todas as localizações do Azure. Durante a pré-visualização do AKS, nem todas as localizações estão disponíveis. Se introduzir uma localização que não é válida neste momento, a mensagem de erro apresentará uma lista das localizações disponíveis.

    ```bash
    az group create -n <your-resource-group-name> -l <your-location>
    ```

1. Crie o cluster de Kubernetes. Substitua a &lt;your---nome do grupo recursos > com o nome do grupo de recursos criado no passo anterior e substitua o &lt;---nome do cluster kubernetes > com o nome do novo cluster. (Este processo pode demorar vários minutos a concluir.)

    ```bash
    az aks create -g <your-resource-group-name> -n <your-kubernetes-cluster-name> --generate-ssh-keys --node-count 2
    ```

### <a name="set-up-the-kubernetes-cluster"></a>Como configurar o cluster de Kubernetes

Configurar uma implementação de azul/verde no AKS pode ser feito com um script de instalação fornecido no exemplo clonado anteriormente ou manualmente. Nesta secção, verá como fazer ambos.

#### <a name="set-up-the-kubernetes-cluster-via-the-sample-setup-script"></a>Como configurar o cluster de Kubernetes via o script de configuração de exemplo
1. Editar a **deploy/aks/setup/setup.sh** ficheiro, substituindo os marcadores de posição seguintes com os valores adequados para o seu ambiente: 

    - **&lt;recurso nome da-grupo >**
    - **&lt;kubernetes cluster-nome da >**
    - **&lt;sua localização >**
    - **&lt;your-dns-nome-sufixo >**

    ![O script de setup.sh contém vários marcadores de posição, que podem ser modificados para o seu ambiente.](./media/jenkins-aks-blue-green-deployment/edit-setup-script.png)

1. Execute o script de configuração.

    ```bash
    sh setup.sh
    ```

#### <a name="set-up-a-kubernetes-cluster-manually"></a>Configurar um cluster de Kubernetes manualmente 
1. Transferir a configuração do Kubernetes para a pasta de perfil.

    ```bash
    az aks get-credentials -g <your-resource-group-name> -n <your-kubernetes-cluster-name> --admin
    ```

1. Altere o diretório para o **implementar/aks/configuração** diretório. 

1. Execute o seguinte **kubectl** comandos para configurar os serviços para o ponto final público e dois pontos de extremidade do teste.

    ```bash
    kubectl apply -f  service-green.yml
    kubectl apply -f  test-endpoint-blue.yml
    kubectl apply -f  test-endpoint-green.yml
    ```

1. Atualize o nome DNS para o público e pontos finais de teste. Quando é criado um cluster do Kubernetes, um [grupo de recursos adicionais](https://github.com/Azure/AKS/issues/3) é criado com o padrão de nomenclatura de **MC_&lt;recurso nome da-grupo > _&lt; kubernetes cluster-nome da >_&lt;sua localização >**.

    Localize o ip público no grupo de recursos

    ![IP público no grupo de recursos](./media/jenkins-aks-blue-green-deployment/publicip.png)

    Para cada um dos serviços, encontre o endereço IP externo ao executar o seguinte comando:
    
    ```bash
    kubectl get service todoapp-service
    ```
    
    Atualize o nome DNS para o endereço IP correspondente com o seguinte comando:

    ```bash
    az network public-ip update --dns-name aks-todoapp --ids /subscriptions/<your-subscription-id>/resourceGroups/MC_<resourcegroup>_<aks>_<location>/providers/Microsoft.Network/publicIPAddresses/kubernetes-<ip-address>
    ```

    Repita a chamada para `todoapp-test-blue` e `todoapp-test-green`:

    ```bash
    az network public-ip update --dns-name todoapp-blue --ids /subscriptions/<your-subscription-id>/resourceGroups/MC_<resourcegroup>_<aks>_<location>/providers/Microsoft.Network/publicIPAddresses/kubernetes-<ip-address>

    az network public-ip update --dns-name todoapp-green --ids /subscriptions/<your-subscription-id>/resourceGroups/MC_<resourcegroup>_<aks>_<location>/providers/Microsoft.Network/publicIPAddresses/kubernetes-<ip-address>
    ```

    O nome DNS tem de ser exclusivo na sua subscrição. `<your-dns-name-suffix>` pode ser utilizado para garantir a exclusividade.

### <a name="create-azure-container-registry"></a>Criar registo de contentor do Azure

1. Execute o `az acr create` comando para criar um Azure Container Registry. Após a criação do Azure Container Registry, utilize `login server` como o URL de registo do Docker na próxima seção.

    ```bash
    az acr create -n <your-registry-name> -g <your-resource-group-name>
    ```

1. Execute o `az acr credential` comando para mostrar as suas credenciais do Azure Container Registry. Tenha em atenção o nome de utilizador de registo de Docker e a palavra-passe pois vão ser utilizados na secção seguinte.

    ```bash
    az acr credential show -n <your-registry-name>
    ```

## <a name="prepare-the-jenkins-server"></a>Preparar o servidor Jenkins

Nesta secção, verá como preparar o servidor do Jenkins para executar uma compilação, que serve para fins de teste. No entanto, como explicado no artigo Jenkins no [implicações de segurança da criação no mestre](https://wiki.jenkins.io/display/JENKINS/Security+implication+of+building+on+master), recomenda-se que utilize um [agente de VM do Azure](https://plugins.jenkins.io/azure-vm-agents) ou [agentes de contentor do Azure](https://plugins.jenkins.io/azure-container-agents) para Prepare um agente no Azure para executar suas compilações. 

1. Implementar um [mestre do Jenkins no Azure](https://aka.ms/jenkins-on-azure).

1. Ligar ao servidor através de SSH e instale as ferramentas de compilação no servidor onde executa sua compilação.
   
   ```bash
   sudo apt-get install git maven 
   ```
   
1. [Instalar o Docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce). Certifique-se o usuário `jenkins` tem permissão para executar o `docker` comandos.

1. [Instalar o kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

1. [Transferir jq](https://stedolan.github.io/jq/download/).

1. Instale jq com o seguinte comando:

   ```bash
   sudo apt-get install jq
   ```
   
1. Instale os plug-ins no Jenkins, efetuando os seguintes passos no painel do Jenkins:

    1. Selecione **gerir Jenkins > Gerir plug-ins > disponível**.
    1. Procure e instale o plug-in do Azure Container Service.

1. Tem de adicionar as credenciais que serão utilizadas para gerir recursos no Azure. Se ainda não tiver o plug-in, instale o **credenciais do Azure** Plug-in.

1. Adicionar as suas credenciais de Principal de serviço do Azure como um tipo/tipo **Principal de serviço do Microsoft Azure**.

1. Adicionar o seu nome de registo do Docker Azure e a palavra-passe (tal como obtidas na seção, **criar Azure Container Registry**) como um tipo/tipo **nome de utilizador com palavra-passe**.

## <a name="edit-the-jenkinsfile"></a>Editar o Jenkinsfile

1. No seu próprio repositório, navegue para `/deploy/aks/` e abrir `Jenkinsfile`

2. Atualize o ficheiro da seguinte forma:

    ```groovy
    def servicePrincipalId = '<your-service-principal>'
    def resourceGroup = '<your-resource-group-name>'
    def aks = '<your-kubernetes-cluster-name>'

    def cosmosResourceGroup = '<your-cosmodb-resource-group>'
    def cosmosDbName = '<your-cosmodb-name>'
    def dbName = '<your-dbname>'

    def dockerRegistry = '<your-acr-name>.azurecr.io'
    ```
    
    E atualize o ID de credencial do ACR:
    
    ```groovy
    def dockerCredentialId = '<your-acr-credential-id>'
    ```

## <a name="create-the-job"></a>Criar a tarefa
1. Adicionar uma nova tarefa no tipo **Pipeline**.

1. Selecione **Pipeline > Definições > Pipeline de script do SCM**.

1. Introduza o url do repositório SCM com sua &lt;seu repositório bifurcou >

1. Introduza o caminho do script como `deploy/aks/Jenkinsfile`.

## <a name="run-the-job"></a>Executar a tarefa

1. Certifique-se de que pode executar o seu projeto com êxito no seu ambiente local. [Execute o projeto no computador local](https://github.com/Microsoft/todo-app-java-on-azure/blob/master/README.md#run-it)

1. Execute a tarefa do Jenkins. Ao executar a tarefa do Jenkins pela primeira vez, o Jenkins irá implementar a aplicação de lista de tarefas para o ambiente azul, o que é o ambiente de inativo predefinido. 

1. Para verificar que foi executada a tarefa, navegue para os urls:
    - Ponto de final público: `http://aks-todoapp<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`
    - Ponto final de azul- `http://aks-todoapp-blue<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`
    - Ponto de final verde- `http://aks-todoapp-green<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`

O público e os pontos finais de teste azul têm a mesma atualização enquanto o ponto final verde mostra a imagem predefinida do tomcat. 

Se executar a compilação mais de uma vez, ele percorre implementações azul e verde. Em outras palavras, se o ambiente atual for azul, a tarefa irá implementar/teste para o ambiente de verde e, em seguida, atualizar o ponto final público da aplicação para encaminhar o tráfego para o ambiente verde se tudo estiver correto, com o teste.

## <a name="additional-information"></a>Informações adicionais

Para saber mais sobre a implantação sem período de indisponibilidade, veja esta [modelo de início rápido](https://github.com/Azure/azure-quickstart-templates/tree/master/301-jenkins-aks-zero-downtime-deployment). 

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, elimine os recursos do Azure que criou neste tutorial.

```bash
az group delete -y --no-wait -n <your-resource-group-name>
```

## <a name="troubleshooting"></a>Resolução de problemas

Se tiver quaisquer erros com os plug-ins do Jenkins, comunique o assunto na [Jenkins JIRA](https://issues.jenkins-ci.org/) para o componente específico.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu a implementar para Azure Kubernetes Service (AKS) com o Jenkins e o padrão de implementação de azul/verde. Para saber mais sobre o fornecedor do Azure Jenkins, veja o Jenkins no site do Azure.

> [!div class="nextstepaction"]
> [Jenkins no Azure](/azure/jenkins/)