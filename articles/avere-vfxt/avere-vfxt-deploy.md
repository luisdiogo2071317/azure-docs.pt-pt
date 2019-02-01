---
title: Implementar Avere vFXT para o Azure
description: Passos para implementar o cluster de vFXT Avere no Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: v-erkell
ms.openlocfilehash: f6d847e9042341f47a06fde0f9aa4a70f2549a07
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55512164"
---
# <a name="deploy-the-vfxt-cluster"></a>Implementar o cluster vFXT

Este procedimento explica como utilizar o Assistente de implementação disponível no Azure Marketplace. O assistente implementa automaticamente o cluster com um modelo Azure Resource Manager. Depois de introduzir os parâmetros na forma e clique em **criar**, Azure preenche automaticamente estes passos: 

* Crie o controlador de cluster, o que é uma VM básica que contém o software necessário para implementar e gerir o cluster.
* Configure o grupo de recursos e a infraestrutura de rede virtual, incluindo a criação de novos elementos se for necessário.
* Criar o cluster de VMs de nó e configurá-los como o cluster Avere.
* Se solicitado, crie um novo contentor de Blobs do Azure e configurá-lo como um filtro de núcleo de cluster.

Depois de seguir as instruções neste documento, terá uma rede virtual, uma sub-rede, um controlador e um cluster de vFXT, conforme mostrado no diagrama seguinte:

![diagrama que mostra a vnet que contém armazenamento de BLOBs opcional e uma sub-rede que contém três agrupados VMs denominadas vFXT nós/vFXT cluster e um controlador de cluster com nome de VM](media/avere-vfxt-deployment.png)

Depois de criar o cluster, deve [criar um ponto de final de armazenamento](#create-a-storage-endpoint-if-using-azure-blob) na sua rede virtual, se utilizar o armazenamento de Blobs. 

Antes de utilizar o modelo de criação, certifique-se de que ter abordou esses pré-requisitos:  

1. [Nova subscrição](avere-vfxt-prereqs.md#create-a-new-subscription)
1. [Permissões de proprietário da subscrição](avere-vfxt-prereqs.md#configure-subscription-owner-permissions)
1. [Quota para o cluster vFXT](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster)
1. [Funções de acesso personalizado](avere-vfxt-prereqs.md#create-access-roles) -tem de criar uma função de controlo de acesso baseado em funções para atribuir a nós do cluster. Tem a opção de também criar uma função de acesso personalizado para o controlador de cluster, mas a maioria dos utilizadores irá demorar a função de proprietário predefinida, que oferece os privilégios do controlador correspondente a um proprietário do grupo de recursos. Leia [funções incorporadas para recursos do Azure](../role-based-access-control/built-in-roles.md#owner) para obter mais detalhes.

Para obter mais informações sobre passos de implementação de cluster e o planejamento, leia [planear o seu sistema de vFXT Avere](avere-vfxt-deploy-plan.md) e [descrição geral da implementação](avere-vfxt-deploy-overview.md).

## <a name="create-the-avere-vfxt-for-azure"></a>Criar o vFXT Avere para o Azure

Aceder ao modelo de criação no portal do Azure ao pesquisar Avere e selecionar "Avere vFXT ARM implantação". 

![Trilhas de janela do browser que mostra o portal do Azure com o pão "Novo > Marketplace > tudo". Em tudo, a página, o campo de pesquisa tem o termo "avere" e o segundo resultado, "Avere vFXT ARM implementação" é descrito em vermelho para destacá-la.](media/avere-vfxt-template-choose.png)

Depois de ler os detalhes na página Avere vFXT implementação ARM, clique em **criar** para começar. 

![O Azure marketplace com a primeira página da mostrar de modelo de implementação](media/avere-vfxt-deploy-first.png)

O modelo está dividido em quatro passos - coleta de informações de duas páginas, além de validação e de confirmação de passos. 

* Página um concentra-se nas definições para a VM do controlador de cluster. 
* Página dois recolhe parâmetros para criar o cluster e os recursos associados, como sub-redes e armazenamento. 
* Página três resume as definições e valida a configuração. 
* Página quatro explica software termos e condições e permite-lhe iniciar o processo de criação do cluster. 

## <a name="page-one-parameters---cluster-controller-information"></a>Página parâmetros de um - informações de controlador do cluster

A primeira página do modelo de implementação de coleta informações sobre o controlador de cluster. 

![Primeira página do modelo de implementação](media/avere-vfxt-deploy-1.png)

Preencha as seguintes informações:

* **Nome do controlador de cluster** -defina o nome de VM do controlador de cluster.

* **Nome de utilizador do controlador** -preencha o nome de utilizador de raiz para a VM do controlador de cluster. 

* **Tipo de autenticação** -escolher palavra-passe ou os autenticação de chave pública SSH para ligar ao controlador. Recomenda-se o método de chave público SSH; Leia [como criar e utilizar chaves SSH](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) se precisar de ajuda.

* **Palavra-passe** ou **chave pública SSH** -dependendo do tipo de autenticação que selecionou, tem de fornecer uma chave pública RSA ou uma palavra-passe nos campos seguintes. Esta credencial é utilizada com o nome de utilizador fornecido anteriormente.

* **ID de função de criação do Avere cluster** -Utilize este campo para especificar a função de controlo de acesso para o controlador de cluster. O valor predefinido é a função incorporada [proprietário](../role-based-access-control/built-in-roles.md#owner). Privilégios de proprietário para o controlador de cluster estão limitados ao grupo de recursos do cluster. 

  Tem de utilizar o identificador exclusivo global que corresponde à função. Para o valor predefinido (proprietário), o GUID é 8e3af657-a8ff-443c-a75c-2fe8c4bcb635. Para localizar o GUID de uma função personalizada, utilize este comando: 

  ```azurecli
  az role definition list --query '[*].{roleName:roleName, name:name}' -o table --name 'YOUR ROLE NAME'
  ```

* **Subscrição** -selecione a subscrição para o vFXT Avere. 

* **Grupo de recursos** - selecione o grupo de recursos para o cluster de vFXT Avere, ou clique em "Criar nova" e introduza um nome de grupo de recursos novo. 

* **Localização** -selecione a localização do Azure para o cluster e recursos.

Clique em **OK** quando terminar. 

> [!NOTE]
> Se pretender que o controlador de cluster para ter um endereço IP destinado ao público, crie uma nova rede virtual para o cluster em vez de selecionar uma rede existente. Se esta definição na página de dois.

## <a name="page-two-parameters---vfxt-cluster-information"></a>Página dois parâmetros - vFXT informações do cluster

A segunda página do modelo de implementação permite-lhe definir o tamanho do cluster, tipo de nó, o tamanho da cache e os parâmetros de armazenamento, entre outras definições. 

![Segunda página do modelo de implementação](media/avere-vfxt-deploy-2.png)

* **Contagem de nós de cluster de vFXT Avere** -escolher o número de nós para utilizar no cluster. O mínimo é de três nós e o máximo é doze. 

* **Palavra-passe de administração do cluster** -criar a palavra-passe para a administração de cluster. Esta palavra-passe será utilizado com o nome de utilizador ```admin``` para iniciar sessão no painel de controle de cluster para monitorizar o cluster e configurar as definições.

* **Função de operações de cluster Avere** -especifique o nome da função de controlo de acesso para os nós do cluster. Esta é uma função personalizada que foi criada como um passo dos pré-requisitos. 

  O exemplo descrito em [criar a função de acesso de nó de cluster](avere-vfxt-prereqs.md#create-the-cluster-node-access-role) guarda o ficheiro como ```avere-operator.json``` e o nome de função correspondente é ```avere-operator```.

* **Nome do cluster Avere vFXT** -dê um nome exclusivo ao cluster. 

* **Tamanho** -especifique o tipo VM a utilizar ao criar os nós do cluster. 

* **Colocar em cache de tamanho por nó** -a cache de cluster é distribuído por todos os nós do cluster, por isso, o tamanho de total cache no seu cluster de vFXT Avere será o tamanho da cache por nó, multiplicado pelo número de nós. 

  A configuração recomendada é utilizar 1 TB por nó, se utilizar nós de cluster Standard_D16s_v3 e utilizar 4 TB por nó, se utilizar Standard_E32s_v3 nós.

* **Rede virtual** – selecionar uma vnet já existente para abrigar o cluster, ou definir uma nova vnet para criar. 

  > [!NOTE]
  > Se criar uma nova vnet, o controlador de cluster terão um endereço IP público para que possa acessar a nova rede privada. Se optar por uma vnet já existente, o controlador de cluster está configurado sem um endereço IP público. 
  > 
  > Um endereço IP publicamente visível no controlador de cluster proporciona um acesso simplificado ao vFXT cluster, mas cria um risco de segurança pequeno. 
  >  * Um endereço IP público no controlador de cluster permite-lhe utilizá-lo como um anfitrião de ligação para ligar ao cluster de vFXT Avere de fora da sub-rede privada.
  >  * Se não configurar um endereço IP público no controlador, tem de utilizar outro rápida ao anfitrião, uma ligação VPN ou ExpressRoute para aceder ao cluster. Por exemplo, crie o controlador de dentro de uma rede virtual que já tenha uma ligação de VPN configurada.
  >  * Se criar um controlador com um endereço IP público, é necessário proteger o controlador de VM com um grupo de segurança de rede. Por predefinição, o vFXT Avere para implementação do Azure cria um grupo de segurança de rede e restringe o acesso de entrada apenas a porta 22 para controladores com endereços IP públicos. Pode proteger ainda mais o sistema, bloqueando para baixo de acesso aos seus endereços de origem do intervalo de IP - ou seja, permitir apenas ligações a partir de máquinas que pretende utilizar para o acesso de cluster.

* **Sub-rede** – Selecione uma sub-rede da sua rede virtual existente ou crie um novo. 

* **Utilizar o blob storage** -escolha **true** para criar um novo contentor de Blobs do Azure e configurá-lo como armazenamento de back-end para o novo cluster de vFXT Avere. Esta opção também cria uma nova conta de armazenamento dentro do mesmo grupo de recursos do cluster. 

  Defina este campo para **false** se não pretender criar um novo contentor. Neste caso, tem de ligar e configurar o armazenamento depois de criar o cluster. Leia [configurar o armazenamento](avere-vfxt-add-storage.md) para obter instruções. 

* **Conta de armazenamento** - se criar um novo contentor de Blobs do Azure, introduza um nome para a nova conta de armazenamento. 

## <a name="validation-and-purchase"></a>Validação e compra

Página três fornece um resumo da configuração e valida os parâmetros. Após a validação tiver êxito, clique nas **OK** botão para continuar. 

![Terceira página do modelo de implementação - validação](media/avere-vfxt-deploy-3.png)

Na página quatro, clique a **criar** botão para aceitar os termos e criar o vFXT Avere para cluster do Azure. 

![Quarta página do modelo de implementação - termos e condições, crie um botão](media/avere-vfxt-deploy-4.png)

Implementação do cluster demora 15-20 minutos.

## <a name="gather-template-output"></a>Reunir os dados de modelo

Quando o modelo de vFXT Avere acabar de criar o cluster, ele produz algumas informações importantes sobre o novo cluster. 

> [!TIP]
> Certifique-se copiar o endereço IP de gestão a partir da saída do modelo. Tem este endereço para administrar o cluster.

Para encontrar estas informações, siga este procedimento:

1. Vá para o grupo de recursos para seu controlador de cluster.

1. No lado esquerdo, clique em **implementações**e, em seguida **microsoft-avere.vfxt-modelo**.

   ![Página do portal de grupo de recursos com implementações selecionadas à esquerda e da microsoft-avere.vfxt-modelo, que mostra uma tabela em nome da implementação](media/avere-vfxt-outputs-deployments.png)

1. No lado esquerdo, clique em **saídas**. Copie os valores em cada um dos campos. 

   ![Devolve a página que mostra SSHSTRING, RESOURCE_GROUP, localização, NETWORK_RESOURCE_GROUP, rede, sub-rede, SUBNET_ID, VSERVER_IPs e MGMT_IP valores nos campos à direita das etiquetas](media/avere-vfxt-outputs-values.png)


## <a name="create-a-storage-endpoint-if-using-azure-blob"></a>Criar um ponto de final de armazenamento (se utilizar BLOBs do Azure)

Se estiver a utilizar o armazenamento de Blobs do Azure para o armazenamento de dados back-end, deve criar um ponto de extremidade do serviço de armazenamento na sua rede virtual. Isso [ponto final de serviço](../virtual-network/virtual-network-service-endpoints-overview.md) mantém o tráfego de Blobs do Azure local em vez de encaminhá-lo fora da rede virtual.

1. No portal, clique em **redes virtuais** à esquerda.
1. Selecione a vnet para seu controlador. 
1. Clique em **pontos finais de serviço** à esquerda.
1. Clique em **adicionar** na parte superior.
1. Deixe o serviço como ``Microsoft.Storage`` e selecione a sub-rede do controlador.
1. Na parte inferior, clique em **adicionar**.

  ![Captura de ecrã portal do Azure com anotações para obter os passos de criação de ponto final do serviço](media/avere-vfxt-service-endpoint.png)

## <a name="next-step"></a>Passo seguinte

Agora que o cluster está em execução e sabe que o respetivo endereço IP de gestão, pode [ligue-se para a ferramenta de configuração de cluster](avere-vfxt-cluster-gui.md) para ativar o suporte, adicionar armazenamento, se necessário e personalizar outras configurações de cluster.
