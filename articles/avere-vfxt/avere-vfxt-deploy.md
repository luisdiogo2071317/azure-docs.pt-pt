---
title: Implementar Avere vFXT para o Azure
description: Passos para implementar o cluster de vFXT Avere no Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 8e265f2bed480f7b40476e09ab8f442aedcc9dd4
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52999458"
---
# <a name="deploy-the-vfxt-cluster"></a>Implementar o cluster vFXT

É a maneira mais fácil para criar um cluster de vFXT no Azure utilizar um controlador de cluster. O controlador de cluster é uma VM que inclui os scripts necessários, modelos e infra-estrutura de software para criar e gerir o cluster vFXT.

A implementação de um novo cluster de vFXT inclui estes passos:

1. [Criar o controlador de cluster](#create-the-cluster-controller-vm).
1. Se utilizar o armazenamento de Blobs do Azure, [criar um ponto de final de armazenamento](#create-a-storage-endpoint-if-using-azure-blob) na sua rede virtual.
1. [Ligar ao controlador de cluster](#access-the-controller). O restante dos seguintes passos são feitas a partir da VM do controlador de cluster. 
1. [Criar a função de acesso](#create-the-cluster-node-access-role) para os nós de cluster. É fornecido um protótipo.
1. [Personalizar o script de criação do cluster](#edit-the-deployment-script) para o tipo de cluster de vFXT que pretende criar.
1. [Execute o script de criação do cluster](#run-the-script).

Para obter mais informações sobre passos de implementação de cluster e o planejamento, leia [planear o seu sistema de vFXT Avere](avere-vfxt-deploy-plan.md) e [descrição geral da implementação](avere-vfxt-deploy-overview.md). 

Depois de seguir as instruções neste documento, terá uma rede virtual, uma sub-rede, um controlador e um cluster de vFXT, conforme mostrado no diagrama seguinte:

![diagrama que mostra a vnet que contém armazenamento de BLOBs opcional e uma sub-rede que contém três agrupados VMs denominadas vFXT nós/vFXT cluster e um controlador de cluster com nome de VM](media/avere-vfxt-deployment.png)

Antes de começar, certifique-se de que ter abordou esses pré-requisitos:  

1. [Nova subscrição](avere-vfxt-prereqs.md#create-a-new-subscription)
1. [Permissões de proprietário da subscrição](avere-vfxt-prereqs.md#configure-subscription-owner-permissions)
1. [Quota para o cluster vFXT](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster)

Opcionalmente, pode criar a função de nó de cluster [antes de](avere-vfxt-pre-role.md) criar o controlador de cluster, mas é mais simples de fazê-lo mais tarde.

## <a name="create-the-cluster-controller-vm"></a>Criar a VM do controlador de cluster

A primeira etapa é criar a máquina virtual que irá criar e configurar os nós de cluster vFXT. 

O controlador de cluster é uma VM do Linux com o software de criação do cluster de vFXT Avere e os scripts pré-instalado. Não é necessário espaço de armazenamento ou de energia de processamento significativo, para que possa escolher as opções de baixo custo. Esta VM será utilizado ocasionalmente durante o ciclo de vida do vFXT cluster.

Existem dois métodos para criar a VM do controlador de cluster. Uma [modelo Azure Resource Manager](#create-controller---arm-template) é fornecido [abaixo](#create-controller---arm-template) simplificar o processo, mas também pode criar o controlador a partir do [imagem do Azure Marketplace](#create-controller---azure-marketplace-image). 

Pode criar um novo grupo de recursos como parte da criação do controlador.

> [!TIP]
>
> Decida se deve ou não utilizar um endereço IP público no controlador de cluster. Um endereço IP público fornece acesso mais fácil para o cluster vFXT, mas cria um risco de segurança pequeno.
>
>  * Um endereço IP público no controlador de cluster permite-lhe utilizá-lo como um anfitrião de ligação para ligar ao cluster de vFXT Avere de fora da sub-rede privada.
>  * Se não configurar um endereço IP público no controlador, tem de utilizar outro rápida ao anfitrião, uma ligação VPN ou ExpressRoute para aceder ao cluster. Por exemplo, crie o controlador de dentro de uma rede virtual que tenha uma ligação de VPN configurada.
>  * Se criar um controlador com um endereço IP público, é necessário proteger o controlador de VM com um grupo de segurança de rede. Permita o acesso apenas por meio de porta 22 para fornecer acesso à internet.

### <a name="create-controller---resource-manager-template"></a>Criar o controlador - modelo do Resource Manager

Para criar o nó de controlador de cluster a partir do portal, clique no botão "Implementar no Azure" abaixo. Isto implementar o modelo cria a VM que irá criar e gerir o cluster de vFXT Avere.

[![botão para criar o controlador](media/deploytoazure.png)](https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAvere%2Fmaster%2Fsrc%2Fvfxt%2Fazuredeploy.json)

Forneça as seguintes informações.

Na **BASIC** secção:  

* **Subscrição** para o cluster
* **Grupo de recursos** para o cluster 
* **Localização** 

Na **definições** secção:

* Se deve ou não criar uma nova rede virtual

  * Se criar uma nova vnet, o controlador de cluster será atribuído um endereço IP público, de modo a que pode aceder ao mesmo. Um grupo de segurança de rede também é criado para esta vnet que restringe o tráfego de entrada apenas a porta 22.
  * Se pretender utilizar o ExpressRoute ou uma VPN para ligar ao controlador de cluster, definir este valor como `false` e especificar uma vnet já existente nos campos restantes. O controlador de cluster irá utilizar essa vnet para a comunicação de rede. 

* Grupo de recursos de rede virtual, o nome e o nome da sub-rede - escreva os nomes dos recursos existentes (se utilizar uma vnet já existente) ou escreva novos nomes de se criar uma nova vnet
* **Nome do controlador** -defina um nome para o controlador de VM
* A predefinição é de nome de utilizador de administrador controlador- `azureuser`
* Chave SSH - colar a chave pública para associar o nome de utilizador de administrador. Leia [como criar e utilizar chaves SSH](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) se precisar de ajuda.

Sob **termos e condições**: 

* Leia os termos de serviço e clique na caixa de verificação para aceitá-los. 

  > [!NOTE] 
  > Se não for um proprietário da subscrição, ter um proprietário de aceitar os termos para, seguindo o pré-requisito os passos [aceitar software termos com antecedência](avere-vfxt-prereqs.md#accept-software-terms-in-advance). 


Clique em **Compra** quando terminar. Depois de cinco ou seis minutos, o nó de controlador estará em execução.

Visite a página de saídas para reunir as informações de controlador que precisa para criar o cluster. Leia [informações necessárias para criar o cluster](#information-needed-to-create-the-cluster) para saber mais.

### <a name="create-controller---azure-marketplace-image"></a>Criar o controlador - imagem do Azure Marketplace

Encontrar o modelo de controlador ao pesquisar no Azure Marketplace para o nome ``Avere``. Selecione o **Avere vFXT para o controlador de Azure** modelo.

Se ainda não o fez, aceite os termos e ativar o acesso programático para a imagem do Marketplace ao clicar em "Desejam implantar programaticamente?" ligar a por baixo da **criar** botão.

![Captura de ecrã de uma ligação para o acesso programático, que está abaixo do botão Criar](media/avere-vfxt-deploy-programmatically.png)

Clique nas **ativar** botão e guardar a definição.

![Captura de ecrã que mostra clique do mouse para permitir o acesso programático](media/avere-vfxt-enable-program.png)

Regressar à página principal dos **Avere vFXT para o controlador do Azure** modelo e clique em **criar**. 

No painel à primeira, preencha ou confirmar estas opções básicas:

* **Subscrição**
* **Grupo de recursos** (introduza um novo nome se quiser criar um novo grupo.)
* **Nome da máquina virtual** -o nome do controlador
* **Região**
* **Opções de disponibilidade** -não é necessária a redundância
* **Imagem** -a imagem de nó de controlador do Avere vFXT
* **Tamanho** - deixe a predefinição ou escolha outro tipo de baixo custo
* **Conta de administrador** -defina como iniciar sessão no controlador de cluster: 
  * Selecione o nome de utilizador/palavra-passe ou chave pública SSH (recomendado).
  
    > [!TIP] 
    > Uma chave SSH é mais segura. Leia [como criar e utilizar chaves SSH](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) se precisar de ajuda. 
  * Especifique o nome de utilizador 
  * Cole a chave SSH, ou introduza e confirme a palavra-passe
* **Regras de porta de entrada** - se utilizando um endereço IP público, abrir porta 22 (SSH)

Clique em **seguinte** para definir as opções de disco:

* **Tipo de disco de SO** -o valor predefinido de HDD é suficiente
* **Utilizar discos não geridos** - não é necessário
* **Discos de dados** -não utilizar

Clique em **seguinte** para as opções de redes:

* **Rede virtual** – selecione a vnet para o controlador ou introduza um nome para criar uma nova vnet. Se não pretender utilizar um endereço IP público no controlador, considere a sua localização dentro de uma vnet com o ExpressRoute ou outro método de acesso já configurado.
* **Sub-rede** -Selecione uma sub-rede dentro da vnet (opcional). Se criar uma nova vnet, pode criar uma nova sub-rede ao mesmo tempo.
* **IP público** -se de que pretende utilizar um endereço IP público, pode especificá-los aqui. 
* **Grupo de segurança de rede** -mantenha a predefinição (**básica**) 
* **Portas de entrada públicas** - se utilizando um endereço IP público, usar esse controle para permitir o acesso de tráfego de SSH. 
* **Accelerated networking** não está disponível para esta VM.

Clique em **seguinte** para definir as opções de gestão:

* **Diagnóstico de arranque** -alterar para **desativado**
* **SO convidado diagnóstico** -deixe desativado
* **Conta de armazenamento de diagnóstico** - opcionalmente, selecione ou especifique uma conta nova para armazenar informações de diagnóstico.
* **Identidade de serviço gerida** -alterar esta opção para **no**, que cria um principal de serviço do Azure AD para o controlador de cluster.
* **Automatické vypnutí** -deixe de fora 

Neste momento, pode clicar em **rever + criar** se não pretender utilizar etiquetas de instância. Caso contrário, clique em **próxima** duas vezes para ignorar o **convidado config** página e chegar à página de etiquetas. Quando terminar de lá, clique em **rever + criar**. 

Depois das suas seleções são validadas, clique nas **criar** botão.  

Criação demora cinco ou seis minutos.

## <a name="create-a-storage-endpoint-if-using-azure-blob"></a>Criar um ponto de final de armazenamento (se utilizar BLOBs do Azure)

Se estiver a utilizar o armazenamento de Blobs do Azure para o armazenamento de dados back-end, deve criar um ponto de extremidade do serviço de armazenamento na sua rede virtual. Isso [ponto final de serviço](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) mantém o tráfego de Blobs do Azure local em vez de encaminhá-lo através da internet.

1. No portal, clique em **redes virtuais** à esquerda.
1. Selecione a vnet para seu controlador. 
1. Clique em **pontos finais de serviço** à esquerda.
1. Clique em **adicionar** na parte superior.
1. Deixe o serviço como ``Microsoft.Storage`` e selecione a sub-rede do controlador.
1. Na parte inferior, clique em **adicionar**.

  ![Captura de ecrã portal do Azure com anotações para obter os passos de criação de ponto final do serviço](media/avere-vfxt-service-endpoint.png)

## <a name="information-needed-to-create-the-cluster"></a>Informações necessárias para criar o cluster

Depois de criar o controlador de cluster, certifique-se de que tem as informações que necessárias para os passos seguintes. 

Informações necessárias para ligar ao controlador de: 

* Nome de utilizador do controlador e chave SSH (ou a palavra-passe)
* Endereço IP do controlador ou de outro método para ligar ao controlador de VM

Informações necessárias para o cluster: 

* Nome do grupo de recursos
* Localização Azure 
* Nome da rede virtual:
* Nome da sub-rede
* Nome de função do nó de cluster – este nome é definido quando criar a função, descrita [abaixo](#create-the-cluster-node-access-role)
* Nome de conta de armazenamento, se criar um contentor de BLOBs

Se o nó de controlador que criou utilizando o modelo do Resource Manager, pode obter as informações a partir da [saída de modelos de](#find-template-output). 

Se utilizou a imagem do Azure Marketplace para criar o controlador, fornecidos diretamente a maioria desses itens. 

Localize todos os itens em falta ao navegar para a página de informações de VM do controlador. Por exemplo, clique em **todos os recursos** e procure o nome de controlador, em seguida, clique no nome de controlador para ver os respetivos detalhes.

### <a name="find-template-output"></a>Encontrar o resultado de modelo

Para encontrar estas informações do Resource Manager a saída do modelo, siga este procedimento:

1. Vá para o grupo de recursos para seu controlador de cluster.

1. No lado esquerdo, clique em **implementações**e, em seguida **Template**.

   ![Página de portal de grupo de recursos com implementações selecionado à esquerda e Template a mostrar numa tabela em nome da implementação](media/avere-vfxt-deployment-template.png)

1. No lado esquerdo, clique em **saídas**. Copie os valores em cada um dos campos. 

   ![página de saídas com SSHSTRING, RESOURCE_GROUP, localização, rede, sub-rede e SUBNET_ID mostrado nos campos à direita das etiquetas](media/avere-vfxt-template-outputs.png)

## <a name="access-the-controller"></a>O controlador de acesso

Para fazer o resto dos passos de implementação, terá de ligar ao controlador de cluster.

1. O método para ligar ao seu controlador de cluster depende da sua configuração.

   * Se o controlador tem um endereço IP público, o SSH para IP o controlador como o nome de utilizador do administrador, definir (por exemplo, ``ssh azureuser@40.117.136.91``).
   * Se o controlador não tem um IP público, utilize uma VPN ou [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) ligação à vnet.

1. Depois de iniciar sessão seu controlador, autenticar através da execução `az login`. Copie o código de autenticação fornecido no shell, em seguida, utilizar um browser para carregar [ https://microsoft.com/devicelogin ](https://microsoft.com/devicelogin) e autenticar com o Microsoft system. Devolver o shell de confirmação.

   ![Saída de linha de comandos do comando de "Início de sessão AZ" exibindo o código de ligação e autenticação do browser](media/avere-vfxt-azlogin.png)

1. Adicione a subscrição ao executar este comando usando o seu ID de subscrição:  ```az account set --subscription YOUR_SUBSCRIPTION_ID```

## <a name="create-the-cluster-node-access-role"></a>Criar a função de acesso de nó de cluster

> [!NOTE] 
> * Se não for um proprietário da subscrição e a função já não tiver sido criada, ter um proprietário de subscrição, siga estes passos ou utilize o procedimento [criar a função de acesso de tempo de execução do Avere vFXT cluster sem um controlador](avere-vfxt-pre-role.md).
> 
> * Usuários internos da Microsoft devem utilizar a função existente com o nome "Operador de tempo de execução do Cluster de Avere" em vez de tentar criar um. 

[Controlo de acesso baseado em funções](https://docs.microsoft.com/azure/role-based-access-control/) (RBAC) dá os nós do cluster vFXT autorização para efetuar tarefas necessárias.  

Como parte da operação de cluster normal vFXT, vFXT individual, nós têm de fazer coisas como ler propriedades de recursos do Azure, gerir o armazenamento e controlar as definições de interface de rede dos outros nós. 

1. No controlador, abra o ``/avere-cluster.json`` ficheiro num editor.

   ![consola que mostra um comando de lista e, em seguida, "vi /avere-cluster.json"](media/avere-vfxt-open-role.png)

1. Edite o ficheiro para incluir o seu ID de subscrição e eliminar a linha acima dela. Guarde o ficheiro como ``avere-cluster.json``.

   ![Consola do editor de texto, que mostra o ID de subscrição e o "remover esta linha" selecionados para eliminação](media/avere-vfxt-edit-role.png)

1. Use este comando para criar a função:  

   ```bash
   az role definition create --role-definition /avere-cluster.json
   ```

Passará o nome da função para o script de criação de cluster no passo seguinte. 

## <a name="create-nodes-and-configure-the-cluster"></a>Nós de criar e configurar o cluster

Para criar o cluster de vFXT Avere, edite um dos scripts de exemplo incluídos no controlador e executá-lo lá. Scripts de exemplo estão localizados no diretório de raiz (`/`) no controlador de cluster.

* Se pretender criar um contentor de BLOBs para utilizar como sistema de armazenamento de back-end do vFXT de Avere, utilize o ``create-cloudbacked-cluster`` script.

* Se irá adicionar armazenamento mais tarde, utilize o ``create-minimal-cluster`` script.

> [!TIP]
> Scripts de protótipo para adicionar nós e destruir o cluster vFXT também estão incluídas no `/` diretório da VM do controlador de cluster.

### <a name="edit-the-deployment-script"></a>Edite o script de implementação

Abra o script de exemplo num editor. Pode querer guardar o script personalizado com um nome diferente para evitar Sobrescrever o exemplo original.

Introduza valores para estas variáveis de script.

* Nome do grupo de recursos

  * Se usar componentes de armazenamento ou de rede que estão em grupos de recursos diferente, anule os comentários as variáveis e também de fornecer esses nomes. 

```python
# Resource groups
# At a minimum specify the resource group.  If the network resources live in a
# different group, specify the network resource group.  Likewise for the storage
# account resource group.
RESOURCE_GROUP=
#NETWORK_RESOURCE_GROUP=
#STORAGE_RESOURCE_GROUP=
```

* Nome da localização
* Nome da rede virtual:
* Nome da sub-rede
* Nome do Azure AD runtime função - se seguiu o exemplo na [criar a função de acesso de nó de cluster](#create-the-cluster-node-access-role), utilize ``avere-cluster``. 
* Nome de conta de armazenamento (se criar um novo contentor de BLOBs)
* Nome do cluster - não pode ter dois clusters de vFXT com o mesmo nome no mesmo grupo de recursos. Dê um nome exclusivo para o procedimento recomendado de cada cluster.
* Palavra-passe administrativa - escolha uma palavra-passe segura para a monitorização e administrar o cluster. Esta palavra-passe é atribuída ao utilizador ``admin``. 
* Tipo de instância de nó - consulte [tamanhos de nó vFXT](avere-vfxt-deploy-plan.md#vfxt-node-sizes) para obter informações
* Tamanho da cache de nó - consulte [tamanhos de nó vFXT](avere-vfxt-deploy-plan.md#vfxt-node-sizes) para obter informações

Guarde o ficheiro e saia.

### <a name="run-the-script"></a>Executar o script

Execute o script, escrevendo o nome de ficheiro que criou. (Exemplo: `./create-cloudbacked-cluster-west1`)  

> [!TIP]
> Considere executar este comando dentro de um [terminal Multiplexador](http://linuxcommand.org/lc3_adv_termmux.php) como `screen` ou `tmux` no caso de perder a ligação.  

A saída também é registada para `~/vfxt.log`.

Quando o script tiver concluído, copie o endereço IP de gestão, o que é necessária para a administração de cluster.

![Saída da linha de comando do script exibindo o endereço IP de gestão perto do fim](media/avere-vfxt-mgmt-ip.png)

> [!IMPORTANT] 
> Se tiver criado um novo contentor de BLOBs, pode estar encriptado com uma chave de predefinição que não é guardada fora do cluster. Antes de armazenar os dados no contentor, tem de transferir o ficheiro de recuperação de chaves ou criar sua própria chave de encriptação e guardar o ficheiro de recuperação numa localização persistente. 
> 
> Se utilizar a chave predefinida sem a transferir o ficheiro de recuperação, é possível perder o acesso aos dados criptografados no filtro de núcleo Blob se o cluster vFXT é destruído ou perdido.
>
> Se o seu script mostra `WARNING` mensagens, como aquelas com círculos na captura de ecrã abaixo, siga as instruções em [configurar o armazenamento](avere-vfxt-add-storage.md) para transferir o ficheiro de chave ou crie uma nova chave para o contentor de Blobs. Utilize a ferramenta de configuração de cluster, o painel de controlo de Avere.

![Saída da linha de comando do script exibindo mensagens de aviso sobre a criação de uma nova chave de encriptação](media/avere-vfxt-key-warning.png)

## <a name="next-step"></a>Passo seguinte

Agora que o cluster está em execução e sabe que o respetivo endereço IP de gestão, pode [ligue-se para a ferramenta de configuração de cluster](avere-vfxt-cluster-gui.md) para ativar o suporte, adicionar armazenamento, se necessário, ou o endereço a chave de encriptação predefinido no seu novo armazenamento de Blobs.
