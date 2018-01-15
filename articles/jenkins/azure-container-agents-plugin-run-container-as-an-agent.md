---
title: "Criar um projeto no Azure utilizando o Jenkins e instâncias de contentor do Azure"
description: "Saiba como utilizar o plug-in do agente de contentor do Azure para Jenkins para criar um projeto no Azure com instâncias de contentor do Azure"
services: multiple
documentationcenter: 
author: tomarcher
manager: rloutlaw
editor: 
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 12/11/2017
ms.author: tarcher
ms.custom: jenkins
ms.openlocfilehash: 04a60bf021ec6e265a3880264386ad32ec7e8177
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="build-a-project-in-azure-using-jenkins-and-azure-container-instances"></a>Criar um projeto no Azure utilizando o Jenkins e instâncias de contentor do Azure

Instâncias de contentor do Azure torna mais fácil para que obtenha cópias de segurança e em execução sem ter de aprovisionar as máquinas virtuais ou que adotar um serviço de nível mais elevado. Instâncias de contentor do Azure fornece faturação por segundo com base na capacidade que terá; tornando-se de uma boa opção para cargas de trabalho transitórias como executar uma compilação.

Saiba como:
> [!div class="checklist"]
> * Instalar e configurar um servidor de Jenkins no Azure
> * Instalar e configurar o plug-in de agentes de contentor do Azure para Jenkins
> * Utilize as instâncias de contentor do Azure para criar o [mola PetClinic exemplo de aplicação](https://github.com/spring-projects/spring-petclinic)

## <a name="prerequisites"></a>Pré-requisitos

- **Subscrição do Azure** - para saber mais sobre as opções de compra do Azure, consulte [como comprar o Azure](https://azure.microsoft.com/pricing/purchase-options/) ou [um mês avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/).

- **Azure CLI 2.0 ou a Shell de nuvem do Azure** -instalar um dos seguintes produtos na qual introduz os comandos do Azure:

    - [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest) -permite-lhe executar comandos do Azure a partir de um comando ou janela de terminal.
    - [Shell de nuvem do Azure](/azure/cloud-shell/quickstart.md) - baseada no Browser shell experiência. Shell de nuvem permite o acesso a uma experiência da linha de comandos baseada no browser criada com as tarefas de gestão do Azure em mente.

## <a name="install-a-jenkins-server-on-azure-using-the-jenkins-marketplace-image"></a>Instalar um servidor de Jenkins no Azure utilizando a imagem do Jenkins Marketplace

Jenkins suporta um modelo de onde Jenkins delegados servidor funcionam para um ou mais agentes para permitir uma instalação de Jenkins único para alojar um grande número de projetos de ou para fornecer diferentes ambientes necessários para baseia-se ou os testes. Os passos nesta secção ajudá-lo a instalar e configurar um servidor de Jenkins no Azure.

[!INCLUDE [jenkins-install-from-azure-marketplace-image](../../includes/jenkins-install-from-azure-marketplace-image.md)]

## <a name="connect-to-the-jenkins-server-running-on-azure"></a>Ligar ao servidor Jenkins em execução no Azure

Assim que instalou Jenkins no Azure, terá de ligar ao Jenkins. Os seguintes passos guiá-lo a configurar uma ligação SSH para a VM Jenkins em execução no Azure. 

[!INCLUDE [jenkins-connect-to-jenkins-server-running-on-azure](../../includes/jenkins-connect-to-jenkins-server-running-on-azure.md)]

## <a name="update-jenkins-dns"></a>Atualização Jenkins DNS

Jenkins precisa de saber o seu próprio URL quando está a criar ligações que apontam para si próprio. Por exemplo, o URL tem de ser utilizado quando Jenkins envia mensagens de correio eletrónico que contém hiperligações diretas para criar os resultados. 

Esta secção explica como definir o URL de Jenkins.

1. No seu browser, navegue para o dashboard de Jenkins em `http://localhost:8080`.

1. Selecione **gerir Jenkins**.

    ![Gerir as opções de Jenkins no dashboard do Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-jenkins.png)

1. Selecione **Configurar sistema**.

    ![Gerir a opção de plug-ins Jenkins no dashboard do Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-configure-system.png)

1. Em **Jenkins localização**, introduza o URL do seu servidor Jenkins.

1. Selecione **Guardar**.

## <a name="update-jenkins-to-allow-java-network-launch-protocol-jnlp"></a>Atualizar Jenkins para permitir o protocolo de rede de iniciar do Java (JNLP)

O agente de Jenkins estabelece ligação com o servidor de Jenkins através de Java rede iniciar protocolo (JNLP). Esta secção explica como especificar uma porta para agentes JNLP a utilizar ao comunicar com o servidor de Jenkins.

1. No dashboard do Jenkins, selecione **gerir Jenkins**.

    ![Gerir as opções de Jenkins no dashboard do Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-jenkins.png)

1. Selecione **configurar a segurança Global**.

    ![Configurar a segurança global no dashboard do Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-configure-global-security.png)

1. Em **agentes**, selecione **Fixed**e introduza uma porta. A captura de ecrã mostra um valor de porta de 12345 como exemplo. Deve especificar uma porta que faz sentido para o seu ambiente.

    ![Atualizar as definições de segurança global Jenkins para permitir JNLP](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-set-jnlp.png)

1. Selecione **Guardar**.

1. Utilizar o Azure CLI 2.0 ou a Shell de nuvem, introduza o seguinte comando para criar uma regra de entrada para o grupo de segurança de rede Jenkins:

    ```azurecli
    az network nsg rule create  \
    --resource-group JenkinsResourceGroup \
    --nsg-name JenkinsNSG  \
    --name allow-https \
    --description "Allow access to port 12345 for HTTPS" \
    --access Allow \
    --protocol Tcp  \
    --direction Inbound \
    --priority 102 \
    --source-address-prefix "*"  \
    --source-port-range "*"  \
    --destination-address-prefix "*" \
    --destination-port-range "12345"
    ```

## <a name="create-and-add-an-azure-service-principal-to-the-jenkins-credentials"></a>Criar e adicionar um principal de serviço do Azure para as credenciais de Jenkins

Precisa de um principal de serviço do Azure para implementar no Azure. Os seguintes passos guiá-lo durante o processo de criação de um principal de serviço (se ainda não tiver um) e atualizar Jenkins com o principal de serviço.

1. Se já tiver um principal de serviço (e conhecer o respetivo ID de subscrição, o inquilino, o appId e a palavra-passe), pode ignorar este passo. Se precisar de criar um principal de segurança, consulte o artigo [criar um Azure principal de serviço com o Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest). Ao criar o principal, tome nota dos valores para o ID de subscrição, o inquilino, o appId e a palavra-passe.

1. Selecione **credenciais**.

    ![Gerir a opção de credenciais no dashboard do Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-credentials.png)

1. Em **credenciais**, selecione **sistema**.

    ![Gerir a opção de credenciais do sistema no dashboard do Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-credentials-system.png)

1. Selecione **Global de credenciais (sem restrições)**.

    ![Gerir a opção de sistema global de credenciais no dashboard do Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-credentials-global.png)

1. Selecione **adicionar algumas credenciais**.

    ![Adicione as credenciais no dashboard do Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-adding-credentials.png)

1. No **tipo** na lista pendente, selecione **Principal de serviço do Microsoft Azure** para fazer com que a página apresentar os campos específicos a adição de um principal de serviço. Em seguida, forneça os valores pedidos da seguinte forma:

    - **Âmbito** -selecionar a opção para **Global (Jenkins, nós, itens, todos os itens subordinados, etc.)** .
    - **ID de subscrição** -utilize o ID de subscrição do Azure especificado durante a execução `az account set`.
    - **ID de cliente** -utilize o `appId` valor devolvido do `az ad sp create-for-rbac`.
    - **Segredo do cliente** -utilize o `password` valor devolvido do `az ad sp create-for-rbac`.
    - **ID de inquilino** -utilize o `tenant` valor devolvido do `az ad sp create-for-rbac`.
    - **Ambiente do Azure** - selecione `Azure`.
    - **ID** -introduza `myTestSp`. Este valor é utilizado mais tarde neste tutorial.
    - **Descrição** - (opcional) introduza um valor de descrição para este principal.

    ![Especifique novas propriedades de principal de serviço no dashboard do Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-principal-properties.png)

1. Depois de introduzir as informações de definir o principal, opcionalmente, pode selecionar **verificar Principal de serviço** para se certificar de que tudo está a funcionar corretamente. Se o principal de serviço está definido corretamente, verá uma mensagem a indicar, "verificada com êxito o Principal de serviço do Microsoft Azure." abaixo o **Descrição** campo.

1. Quando tiver terminado, selecione **OK** para adicionar o principal para Jenkins. O dashboard de Jenkins apresenta o principal recentemente adicionado no **Global de credenciais** página.

## <a name="create-an-azure-resource-group-for-your-azure-container-instances"></a>Criar um grupo de recursos do Azure para as instâncias de contentor do Azure

Instâncias de contentor do Azure tem de ser colocadas num grupo de recursos do Azure. Um grupo de recursos do Azure é um contentor que retém recursos relacionados para uma solução do Azure.

Utilizando o Azure CLI 2.0 ou a Shell de nuvem, introduza o seguinte comando para criar um grupo de recursos denominado `JenkinsAciResourceGroup` na localização `eastus`:

```azurecli
az group create --name JenkinsAciResourceGroup --location eastus
```

Quando terminar, o `az group create` comando apresenta resultados semelhantes ao seguinte exemplo:

```JSON
{
  "id": "/subscriptions/<subscriptionId>/resourceGroups/JenkinsAciResourceGroup",
  "location": "eastus",
  "managedBy": null,
  "name": "JenkinsAciResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="install-the-azure-container-agents-plugin-for-jenkins"></a>Instalar o plug-in de agentes de contentor do Azure para Jenkins

Se já tiver instalado o plug-in de agentes de contentor do Azure, pode ignorar esta secção.

Assim que tiver o grupo de recursos do Azure criado para o agente de Jenkins, os passos seguintes ilustram como instalar o plug-in de agentes de contentor do Azure:

1. No dashboard do Jenkins, selecione **gerir Jenkins**.

    ![Gerir as opções de Jenkins no dashboard do Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-jenkins.png)

1. Selecione **gerir plug-ins**.

    ![Gerir a opção de plug-ins Jenkins no dashboard do Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-plugins.png)

1. Selecione **disponíveis**.

    ![Ver a opção de plug-ins Jenkins disponível no dashboard do Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-view-available-plugins.png)

1. Introduza `Azure Container Agents` para o **filtro** caixa de texto. (A lista de filtros, introduza o texto.)

    ![Filtrar a disponíveis plug-ins de Jenkins no dashboard do Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-filter-available-plugins.png)

1. Selecione a caixa de verificação junto a **agentes de contentor do Azure** Plug-in e uma das opções de instalação. Para efeitos desta demonstração, posso selecionou o **instalar sem reinício** opção.

    ![Instalar o plug-ins de agentes de contentor do Azure a partir do Jenkins dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-install-aks-agent-plugin.png)

1.  Depois de selecionar a opção para instalar o plugin(s) pretendido, o dashboard de Jenkins apresenta uma página com detalhes sobre o estado de que está a instalar.

    ![Estado da instalação de instalar o plug-ins de agentes de contentor do Azure a partir do Jenkins dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-install-aks-agent-plugin-confirmation.png)

    Para voltar a página do Jenkins dashboard principal, selecione **regressar à página superior**.

## <a name="configure-the-azure-container-agents-plugin"></a>Configurar o plug-in de agentes de contentor do Azure

Depois do plug-in de agentes de contentor do Azure é instalado, esta secção orienta-o a configurar o plug-in no Jenkins dashboard.

1. No dashboard do Jenkins, selecione **gerir Jenkins**.

    ![Gerir as opções de Jenkins no dashboard do Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-jenkins.png)

1. Selecione **Configurar sistema**.

    ![Gerir a opção de plug-ins Jenkins no dashboard do Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-configure-system.png)

1. Localize o **nuvem** secção na parte inferior da página e para o **adicionar uma nova nuvem** na lista pendente, selecione **instâncias de contentor do Azure**.

    ![Adicionar um novo fornecedor de nuvem a partir do Jenkins dashboard](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-cloud-provider.png)

1. No **instâncias de contentor do Azure** secção, especifique os valores seguintes:

    - **Nome da nuvem** - (opcional como este valor será assumida a um nome gerado automaticamente.) Especifique um nome para esta instância. 
    - **Credenciais do Azure** - selecione a seta de lista pendente e, em seguida, selecione o `myTestSp` entrada que identifica o principal de serviço do Azure que criou anteriormente.
    - **Grupo de recursos** - selecione a seta de lista pendente e, em seguida, selecione o `JenkinsAciResourceGroup` entrada que identifica o grupo de recursos de instância de contentor do Azure que criou anteriormente.

    ![Definir as propriedades de instância de contentor do Azure](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-aci-properties.png)

1. Selecione o **adicionar modelo de contentor** seta de lista pendente e, em seguida, selecione **Aci contentor modelo**.

1. No **Aci contentor modelo** secção, especifique os valores seguintes:

    - **Nome** -introduza `ACI-container`.
    - **As etiquetas** -introduza `ACI-container`.
    - **Imagem de docker** -introduza`cloudbees/jnlp-slave-with-java-build-tools`

    ![Definir as propriedades da imagem de instância de contentor do Azure](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-aci-image-properties.png)

1. Selecione **avançadas**.

1. Selecione o **estratégia de retenção** seta de lista pendente e selecione **estratégia de retenção de inatividade contentor**. Ao selecionar esta opção, Jenkins mantém o agente até não é executada nenhuma tarefa de novo no agente e o tempo de inatividade especificado tiver decorrido.

    ![Definir a estratégia de retenção de instância de contentor do Azure](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-aci-retention-strategy.png)

1. Selecione **Guardar**.

## <a name="create-the-spring-petclinic-application-job-in-jenkins"></a>Criar a tarefa de mola PetClinic aplicação no Jenkins

Os seguintes passos guiá-lo através da criação de uma tarefa de Jenkins - como um projeto freestyle - para criar a aplicação de PetClinic mola.

1. No dashboard do Jenkins, selecione **Novo Item**.

    ![Opção de menu novo item no dashboard do Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-item.png)

1. Introduza `myPetClinicProject` para o nome do item, selecione **projeto Freestyle**.

    ![Novo projeto freestyle no dashboard do Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-freestyle-project.png)

1. Selecione **OK**.

1. Selecione o **geral** separador e especifique os seguintes valores:

    - **Restringir onde pode ser executado projeto** -Selecione esta opção.
    - **Etiqueta expressão** -introduza `ACI-container`. Quando sair do campo, será apresentada uma mensagem com a confirmar que a etiqueta é fornecida na configuração da nuvem criada no passo anterior.

    ![Definições gerais para um novo projeto freestyle no dashboard do Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-freestyle-project-general.png)

1. Selecione o **gestão da origem de código** separador e especifique os seguintes valores:

    - **Gestão de código de origem** - selecione **Git**.
    - **URL do repositório** -introduza o seguinte URL para o repositório do GitHub de aplicação de exemplo do mola PetClinic: `https://github.com/spring-projects/spring-petclinic.git`.

1. Selecione o **criar** separador e efetuar as seguintes tarefas:

    - Selecione o **Adicionar passo de compilação** seta de lista pendente e selecione **invocar destinos nível superior do Maven**.

    - Para **objetivos**, introduza `package`.

1. Selecione **guardar** para guardar a nova definição de projeto.

## <a name="build-the-spring-petclinic-application-job-in-jenkins"></a>Criar a tarefa de mola PetClinic aplicação Jenkins

Está na altura de criar o projeto! Esta secção explica como criar um projeto a partir do Jenkins dashboard.

1. No dashboard do Jenkins, selecione `myPetClinicProject`.

    ![Selecione o projeto para criar a partir do Jenkins dashboard.](./media/azure-container-agents-plugin-run-container-as-an-agent/select-project-to-build.png)

1. Selecione **compilar agora**. 

    ![Compilar o projeto a partir do Jenkins dashboard.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-project.png)

1. Ao iniciar uma compilação no Jenkins, a compilação é colocada na fila. No caso de um agente de contentor do Azure, a compilação não pode ser executada até que o agente de contentor do Azure é iniciado e colocado online. Até lá, verá uma mensagem a indicar o nome do agente e o facto de que a compilação está pendente. (Este processo demora cerca de cinco minutos, mas é necessário apenas a primeira vez, que utilize o agente de compilação. Compilações subsequentes são muito mais rápidas do que o agente está online nessa altura.)

    ![A compilação é colocada na fila até que o agente é criado e colocado online.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-pending.png)

    Assim que for iniciada a compilação, uma barra de progresso de pólo barber indica que a compilação está em execução:

    ![A compilação está em execução quando vir a barra de progresso de pólo barber.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-running.png)

1. Quando a barra de progresso de pólo barber desaparece, selecione a seta junto ao número de compilação. No menu de contexto, selecione **consola saída**.

    ![Clique no item de menu do registo da consola para ver as informações de compilação.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-console-log-menu.png)

1. As informações de registo de consola emitidas do processo de compilação. Para ver todas as informações de compilação (incluindo informações sobre a compilação que está a ser executada remotamente no agente do Azure criou), selecione **registo completo**.

    ![Clique na ligação de registo completo para ver informações mais detalhadas de compilação.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-console-log.png)

    O registo completo apresenta informações de compilação mais verbosas:

    ![O registo completo apresenta informações de compilação mais verbosas.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-console-log-full.png)
    
1. Para ver a disposição a compilação, desloque-se na parte inferior do registo.

    ![A disposição de compilação é apresentada na parte inferior da registo de compilação.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-disposition.png)

## <a name="clean-up-azure-resources"></a>Limpar os recursos do Azure

Neste tutorial, vai criar recursos contidos em dois grupos de recursos do Azure: 
    - `JenkinsResourceGroup`-Contém os recursos do Azure para o servidor de Jenkins.
    - `JenkinsAciResourceGroup`-Contém os recursos do Azure para o agente de Jenkins.
    
Se já não precisar de utilizar qualquer um dos recursos num grupo de recursos do Azure, pode eliminar o grupo de recursos utilizando o `az group delete` comando da seguinte forma (substituir a &lt;resourceGroup > marcador de posição com o nome do grupo de recursos que pretende Elimine):

```azurecli
az group delete -n <resourceGroup>
```

## <a name="next-steps"></a>Passos Seguintes
> [!div class="nextstepaction"]
> [Visite o Jenkins no hub do Azure para ver as mais recentes artigos e amostras](https://docs.microsoft.com/azure/jenkins/)