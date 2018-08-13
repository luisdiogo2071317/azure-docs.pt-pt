---
title: Implementar para o Azure Kubernetes Service (AKS) com o Jenkins e o padrão de implementação de azul/verde
description: Aprenda a implementar para o Azure Kubernetes Service (AKS) com o Jenkins e o padrão de implementação de azul/verde.
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
ms.openlocfilehash: 384681ae0ba212b485022ac81743528f96075ec8
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39716464"
---
# <a name="deploy-to-azure-kubernetes-service-aks-by-using-jenkins-and-the-bluegreen-deployment-pattern"></a>Implementar para o Azure Kubernetes Service (AKS) com o Jenkins e o padrão de implementação de azul/verde

O Azure Kubernetes Service (AKS) gere o seu ambiente alojado do Kubernetes, tornando rápido e fácil de implementar e gerir aplicações em contentores. Não precisa de conhecimentos de orquestração de contentores. AKS também elimina a carga de operações em curso e manutenção, por aprovisionar, atualizar e dimensionar recursos a pedido. Não precisa de pôr as aplicações offline. Para obter mais informações sobre o AKS, consulte a [documentação AKS](/azure/aks/).

Implementação de azul/verde é um padrão de entrega contínua de DevOps do Azure que se baseia em manter uma versão (azul) existente em direto, quando um novo (verde) está a ser implementado. Normalmente, esse padrão emprega para direcionar o aumento de quantidades de tráfego para a implementação de verde de balanceamento de carga. Se a monitorização Deteta um incidente, o tráfego pode ser reencaminhado para a implementação azul, o que ainda está em execução. Para obter mais informações sobre a entrega contínua, veja [o que é a entrega contínua](/azure/devops/what-is-continuous-delivery).

Neste tutorial, saiba como realizar as seguintes tarefas:

> [!div class="checklist"]
> * Saiba mais sobre o padrão de implementação de azul/verde
> * Criar um cluster de Kubernetes gerido
> * Executar um script de exemplo para configurar um cluster do Kubernetes
> * Configurar manualmente um cluster do Kubernetes
> * Criar e executar uma tarefa do Jenkins

## <a name="prerequisites"></a>Pré-requisitos
- [Conta do GitHub](https://github.com) : precisa de uma conta do GitHub para clonar o repositório de exemplo.
- [CLI 2.0 do Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) : utilizar a CLI 2.0 do Azure para criar o cluster de Kubernetes.
- [Chocolatey](https://chocolatey.org): um Gestor de pacotes que utilizar para instalar o kubectl.
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/): uma interface de linha de comandos que utiliza para executar comandos em relação a clusters do Kubernetes.
- [jq](https://stedolan.github.io/jq/download/): um processador JSON simples e de linha de comandos.

## <a name="clone-the-sample-app-from-github"></a>Clone a aplicação de exemplo do GitHub

No repositório Microsoft no GitHub, pode encontrar uma aplicação de exemplo que ilustra como implementar no AKS com o Jenkins e o padrão de azul/verde. Nesta secção, criar um fork do repositório do que no seu GitHub e clone a aplicação ao seu sistema local.

1. Navegue para o repositório do GitHub para o [todo-aplicação-java-no azure](https://github.com/microsoft/todo-app-java-on-azure.git) aplicação de exemplo.

    ![Captura de ecrã da aplicação de exemplo no repositório do GitHub da Microsoft](./media/jenkins-aks-blue-green-deployment/github-sample-msft.png)

1. Bifurcar o repositório, selecionando **Fork** no canto superior direito da página e siga as instruções para bifurcar o repositório na sua conta do GitHub.

    ![Opção de captura de ecrã do GitHub para o fork](./media/jenkins-aks-blue-green-deployment/github-sample-msft-fork.png)

1. Depois de bifurcar o repositório, verá que o nome da conta é alterado para o seu nome de conta e uma observação indica de onde foi bifurcou repositório (Microsoft).

    ![Nome da conta de captura de ecrã do GitHub bem como observar](./media/jenkins-aks-blue-green-deployment/github-sample-msft-forked.png)

1. Selecione **clonar ou transferir**.

    ![Opção de captura de ecrã do GitHub para clonar ou transferir um repositório](./media/jenkins-aks-blue-green-deployment/github-sample-clone.png)

1. Na **Clone com HTTPS** janela, selecione a **cópia** ícone.

    ![Opção de captura de ecrã do GitHub para copiar o URL do clone para a área de transferência](./media/jenkins-aks-blue-green-deployment/github-sample-copy.png)

1. Abra um terminal ou janela do Git Bash.

1. Altere os diretórios para a localização pretendida, onde pretende armazenar a cópia local (clona) do repositório.

1. Usando o `git clone` comando este problema, clone o URL que copiou anteriormente.

    ![Captura de ecrã do Git Bash comando do git clone](./media/jenkins-aks-blue-green-deployment/git-clone-command.png)

1. Prima a tecla Enter para iniciar o processo de clonagem.

    ![Captura de ecrã do Git Bash resultados de comandos do git clone](./media/jenkins-aks-blue-green-deployment/git-clone-results.png)

1. Altere os diretórios para o diretório recém-criado, que contém o clone da origem de aplicação.

## <a name="create-and-configure-a-managed-kubernetes-cluster"></a>Criar e configurar um cluster de Kubernetes gerido

Nesta secção, execute os seguintes passos:

- Utilize a CLI 2.0 do Azure para criar um cluster de Kubernetes gerido.
- Saiba como configurar um cluster, utilizando o script de configuração ou manualmente.
- Crie uma instância do serviço do Azure Container Registry.

> [!NOTE]   
> AKS está atualmente em pré-visualização. Para obter informações sobre como ativar a pré-visualização para a sua subscrição do Azure, consulte [início rápido: implementar um cluster do Azure Kubernetes Service (AKS)](/azure/aks/kubernetes-walkthrough#enabling-aks-preview-for-your-azure-subscription).

### <a name="use-the-azure-cli-20-to-create-a-managed-kubernetes-cluster"></a>Utilizar a CLI 2.0 do Azure para criar um cluster de Kubernetes gerido
Para criar um cluster de Kubernetes gerido com o [CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), certifique-se de que está a utilizar a CLI do Azure versão 2.0.25 ou posterior.

1. Inicie sessão na sua conta do Azure. Depois de introduzir o seguinte comando, receberá instruções que explicam como concluir o início de sessão. 
    
    ```bash
    az login
    ```

1. Quando executa o `az login` comando no passo anterior, é apresentada uma lista de todas as suas subscrições do Azure (juntamente com suas IDs de subscrição). Neste passo, defina a predefinição de subscrição do Azure. Substitua o &lt;your subscription-id > espaço reservado com o ID de subscrição do Azure pretendida. 

    ```bash
    az account set -s <your-subscription-id>
    ```

1. Crie um grupo de recursos. Substitua a &lt;your---nome do grupo recursos > marcador de posição pelo nome do seu novo grupo de recursos e substitua o &lt;sua localização > marcador de posição pela localização da. O comando `az account list-locations` apresenta todas as localizações do Azure. Durante a pré-visualização do AKS, nem todas as localizações estão disponíveis. Se introduzir uma localização que não é válida neste momento, a mensagem de erro lista as localizações disponíveis.

    ```bash
    az group create -n <your-resource-group-name> -l <your-location>
    ```

1. Crie o cluster de Kubernetes. Substitua a &lt;your---nome do grupo recursos > com o nome do grupo de recursos criado no passo anterior e substitua o &lt;kubernetes cluster-nome da > com o nome do novo cluster. (Este processo pode demorar vários minutos a concluir.)

    ```bash
    az aks create -g <your-resource-group-name> -n <your-kubernetes-cluster-name> --generate-ssh-keys --node-count 2
    ```

### <a name="set-up-the-kubernetes-cluster"></a>Como configurar o cluster de Kubernetes

Pode configurar uma implementação de azul/verde no AKS manualmente ou com uma configuração de script fornecido no exemplo clonados anteriormente. Nesta secção, verá como fazer ambos.

#### <a name="set-up-the-kubernetes-cluster-via-the-sample-setup-script"></a>Como configurar o cluster de Kubernetes via o script de configuração de exemplo
1. Editar a **deploy/aks/setup/setup.sh** ficheiro, substituindo os marcadores de posição seguintes com os valores adequados para o seu ambiente: 

    - **&lt;recurso nome da-grupo >**
    - **&lt;kubernetes cluster-nome da >**
    - **&lt;sua localização >**
    - **&lt;your-dns-nome-sufixo >**

    ![Captura de ecrã setup.sh script no bash, com vários marcadores de posição realçado](./media/jenkins-aks-blue-green-deployment/edit-setup-script.png)

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

1. Atualize o nome DNS para o público e pontos finais de teste. Quando cria um cluster do Kubernetes, também cria um [grupo de recursos adicionais](https://github.com/Azure/AKS/issues/3), com o padrão de nomenclatura **MC_&lt;recurso nome da-grupo >_ &lt; kubernetes cluster-nome da >_&lt;sua localização >**.

    Localize os IPs públicos no grupo de recursos.

    ![Captura de ecrã dos IPs públicos no grupo de recursos](./media/jenkins-aks-blue-green-deployment/publicip.png)

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

    O nome DNS tem de ser exclusivo na sua subscrição. Para garantir a exclusividade, pode usar `<your-dns-name-suffix>`.

### <a name="create-an-instance-of-container-registry"></a>Criar uma instância do registo de contentor

1. Execute o `az acr create` comando para criar uma instância do registo de contentor. Na secção seguinte, em seguida, pode utilizar `login server` como o URL de registo do Docker.

    ```bash
    az acr create -n <your-registry-name> -g <your-resource-group-name>
    ```

1. Execute o `az acr credential` comando para mostrar as suas credenciais de registo de contentor. Tenha em atenção o nome de utilizador de registo de Docker e a palavra-passe, sempre que precisar na próxima seção.

    ```bash
    az acr credential show -n <your-registry-name>
    ```

## <a name="prepare-the-jenkins-server"></a>Preparar o servidor Jenkins

Nesta secção, verá como preparar o servidor do Jenkins para executar uma compilação, que serve para fins de teste. No entanto, deve usar uma [agente de VM do Azure](https://plugins.jenkins.io/azure-vm-agents) ou [agentes de contentor do Azure](https://plugins.jenkins.io/azure-container-agents) acelerar um agente no Azure para executar suas compilações. Para obter mais informações, consulte o artigo de Jenkins sobre o [implicações de segurança da criação no mestre](https://wiki.jenkins.io/display/JENKINS/Security+implication+of+building+on+master).

1. Implementar um [mestre do Jenkins no Azure](https://aka.ms/jenkins-on-azure).

1. Ligar ao servidor através de SSH e instale as ferramentas de compilação no servidor onde executa sua compilação.
   
   ```bash
   sudo apt-get install git maven 
   ```
   
1. [Instalar o Docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce). Certifique-se de que o usuário `jenkins` tem permissão para executar o `docker` comandos.

1. [Instalar o kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

1. [Transferir jq](https://stedolan.github.io/jq/download/).

1. Instale jq com o seguinte comando:

   ```bash
   sudo apt-get install jq
   ```
   
1. Instale os plug-ins no Jenkins, efetuando os seguintes passos no painel do Jenkins:

    1. Selecione **gerir Jenkins > Gerir plug-ins > disponível**.
    1. Procure e instale o plug-in do Azure Container Service.

1. Adicione as credenciais para gerir recursos no Azure. Se ainda não tiver o plug-in, instale o **credenciais do Azure** Plug-in.

1. Adicionar as suas credenciais de Principal de serviço do Azure como o tipo **Principal de serviço do Microsoft Azure**.

1. Adicionar o nome de utilizador de registo de Docker do Azure e a palavra-passe (como obtido na secção, "Criar uma instância do registo de contentor"), como o tipo **nome de utilizador com palavra-passe**.

## <a name="edit-the-jenkinsfile"></a>Editar o Jenkinsfile

1. No seu próprio repositório, aceda a `/deploy/aks/`e abra `Jenkinsfile`.

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
    
    Atualize o ID de credencial de registo de contentor:
    
    ```groovy
    def dockerCredentialId = '<your-acr-credential-id>'
    ```

## <a name="create-the-job"></a>Criar a tarefa
1. Adicionar uma nova tarefa no tipo **Pipeline**.

1. Selecione **Pipeline** > **definição** > **Pipeline de script do SCM**.

1. Introduza o URL do repositório SCM com sua &lt;seu repositório bifurcou >.

1. Introduza o caminho do script como `deploy/aks/Jenkinsfile`.

## <a name="run-the-job"></a>Executar a tarefa

1. Certifique-se de que pode executar o seu projeto com êxito no seu ambiente local. Eis como: [executar o projeto no computador local](https://github.com/Microsoft/todo-app-java-on-azure/blob/master/README.md#run-it).

1. Execute a tarefa do Jenkins. Na primeira vez que executar a tarefa, o Jenkins implementa a aplicação de lista de tarefas para o ambiente azul, o que é o ambiente de inativo predefinido. 

1. Para verificar que foi executada a tarefa, aceda a estes URLs:
    - Ponto de final público: `http://aks-todoapp<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`
    - Ponto final de azul- `http://aks-todoapp-blue<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`
    - Ponto de final verde- `http://aks-todoapp-green<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`

O público e os pontos finais de teste azul têm a mesma atualização, enquanto o ponto final verde mostra a imagem predefinida do tomcat. 

Se executar a compilação mais de uma vez, ele percorre implementações azuis e verdes. Em outras palavras, se o ambiente atual for azul, a tarefa implementa e testes no ambiente verde. Em seguida, se os testes são boas, a tarefa atualiza o ponto final público da aplicação para encaminhar o tráfego para o ambiente verde.

## <a name="additional-information"></a>Informações adicionais

Para saber mais sobre a implantação sem período de indisponibilidade, consulte esta [modelo de início rápido](https://github.com/Azure/azure-quickstart-templates/tree/master/301-jenkins-aks-zero-downtime-deployment). 

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não precisar dos recursos que criou neste tutorial, pode eliminá-los.

```bash
az group delete -y --no-wait -n <your-resource-group-name>
```

## <a name="troubleshooting"></a>Resolução de problemas

Se se deparar com erros nos plug-ins do Jenkins, comunique os problemas com os componentes específicos no [Jenkins JIRA](https://issues.jenkins-ci.org/).

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu a implementar no AKS com o Jenkins e o padrão de implementação de azul/verde. Para saber mais sobre o fornecedor do Azure Jenkins, veja o Jenkins no site do Azure.

> [!div class="nextstepaction"]
> [Jenkins no Azure](/azure/jenkins/)