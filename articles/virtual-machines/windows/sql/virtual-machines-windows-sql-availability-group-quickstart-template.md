---
title: Criar WSFC, o serviço de escuta e configurar o ILB para um grupo de disponibilidade Always On numa VM do SQL Server com o modelo de início rápido do Azure
description: Utilize modelos de início rápido do Azure para simplificar o processo de criação de grupos de disponibilidade para VMs do SQL Server no Azure utilizando um modelo para criar o cluster, Junte-se as VMs do SQL para o cluster, criar o serviço de escuta e configurar o ILB.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/04/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 9be8717bc9b1d15a59486edf206dd0657a711c06
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2019
ms.locfileid: "54360330"
---
# <a name="create-wsfc-listener-and-configure-ilb-for-an-always-on-availability-group-on-a-sql-server-vm-with-azure-quickstart-template"></a>Criar WSFC, o serviço de escuta e configurar o ILB para um grupo de disponibilidade Always On numa VM do SQL Server com o modelo de início rápido do Azure
Este artigo descreve como utilizar os modelos de início rápido do Azure para parcialmente automatizar a implementação de uma configuração de grupo Always On disponibilidade para máquinas virtuais SQL Server no Azure. Existem dois modelos de início rápido do Azure que são utilizados neste processo. 

   | Modelo | Descrição |
   | --- | --- |
   | [101-sql-vm-ag-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-ag-setup) | Cria o Cluster de ativação pós-falha do Windows e associa a VMs do SQL Server para o mesmo. |
   | [101-sql-vm-aglistener-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-aglistener-setup) | Cria o serviço de escuta do grupo de disponibilidade e configura o Balanceador de carga interno. |
   | &nbsp; | &nbsp; |

Outras partes da configuração do grupo de disponibilidade devem ser feitas manualmente, por exemplo, criar o grupo de disponibilidade e criar o Balanceador de carga interno. Este artigo fornece a seqüência de etapas automatizadas e manuais.
 

## <a name="prerequisites"></a>Pré-requisitos 
Para automatizar a configuração de um grupo de disponibilidade Always On na através de modelos de início rápido, já tem de ter os seguintes pré-requisitos: 
- Uma [subscrição do Azure](https://azure.microsoft.com/free/).
- Um grupo de recursos com um [controlador de domínio](https://docs.microsoft.com/azure/architecture/reference-architectures/identity/adds-forest). 
- Um ou mais associado a um domínio [VMs no Azure em execução do SQL Server 2016 (ou superior) Enterprise edition](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) na mesma disponibilidade ou conjunto de zona de disponibilidade que tenham sido [registado com o fornecedor de recursos de VM do SQL Server](#register-existing-sql-vm-with-new-resource-provider).  

## <a name="register-existing-sql-vm-with-new-resource-provider"></a>Registar a VM de SQL existente com o novo fornecedor de recursos
Desde a estes modelos de início rápido do Azure contam com o fornecedor de recursos de VM do SQL (Microsoft.SqlVirtualMachine) um grupo de disponibilidade, VMs existentes do SQL Server tem de estar registadas com o fornecedor de recursos de VM do SQL Server. Ignore este passo se tiver criado a sua VM do SQL Server depois de Dezembro de 2018, como todas as VMs do SQL Server criados após esta data são automaticamente registrados. Esta secção fornece os passos para registar com o fornecedor com o portal do Azure, mas também pode utilizar [PowerShell](virtual-machines-windows-sql-ahb.md#powershell). 

  >[!IMPORTANT]
  > Se remover o seu recurso de VM do SQL Server, irá voltar para a definição de licença hard-coded da imagem. 

1. Abra o portal do Azure e navegue para **todos os serviços**. 
1. Navegue para **subscrições** e selecione a subscrição de interesse.  
1. Na **subscrições** painel, navegue até à **fornecedor de recursos**. 
1. Tipo de `sql` no filtro para apresentar os fornecedores de recursos relacionados com o SQL. 
1. Selecione *registar*, *voltar a registar*, ou *Unregister* para o **Microsoft.SqlVirtualMachine** fornecedor consoante sua ação desejada. 

  ![Modificar o fornecedor](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

## <a name="step-1---create-the-wsfc-and-join-sql-server-vms-to-the-cluster-using-quickstart-template"></a>Passo 1 - criar o WSFC e Junte-se a VMs do SQL Server para o cluster utilizando o modelo de início rápido 
Depois das suas VMs do SQL Server foram registrados com o fornecedor de recursos novos de VM do SQL Server, pode participar em suas VMs do SQL Server em *SqlVirtualMachineGroup*. Este recurso define os metadados do Cluster de ativação pós-falha do Windows, incluindo a versão, edição, nome de domínio completamente qualificado, contas de AD para gerir o cluster e a conta de armazenamento como o testemunho de nuvem. Adicionar VM do SQL Server para o *SqlVirtualMachineGroup* inicializa o serviço de Cluster de ativação pós-falha do Windows e associa a VMs do SQL Server para o cluster. Este passo é automatizado com o **101-sql-vm-ag-setup** modelo de início rápido e pode ser implementada com os seguintes passos:

1. Navegue para o [ **101-sql-vm-ag-setup** ](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-ag-setup) modelo de início rápido e selecione **implementar no Azure** para iniciar o modelo de início rápido no portal do Azure.
1. Preencha os campos necessários para configurar os metadados do Cluster de ativação pós-falha do Windows. Os campos opcionais podem ser deixados em branco.

    A tabela seguinte mostra os valores necessários para o modelo: 

   | **Campo** | Valor |
   | --- | --- |
   | **Subscrição** |  A subscrição onde existem as suas VMs do SQL Server. |
   |**Grupo de recursos** | O grupo de recursos onde residem as suas VMs do SQL Server. | 
   |**Nome do Cluster de ativação pós-falha** | O nome pretendido para o novo Cluster de ativação pós-falha do Windows. |
   | **Lista de Vm existente** | As VMs do SQL Server que pretende participar no grupo de disponibilidade e, assim, fazer parte deste cluster novo. Separar esses valores com uma vírgula e um espaço (ex: SQLVM1, SQLVM2). |
   | **Versão do SQL Server** | Selecione a versão do SQL Server das suas VMs do SQL Server na lista pendente. Imagens de SQL 2016 e SQL 2017 atualmente, apenas são suportadas. |
   | **Nome de domínio completamente qualificado existente** | O FQDN existente para o domínio no qual residem as suas VMs do SQL Server. |
   | **Conta de domínio existente** | Uma conta de domínio existente que tenha acesso de administrador do sistema para o SQL Server. | 
   | **Palavra-passe da conta de domínio** | A palavra-passe para a conta de domínio mencionado anteriormente. | 
   | **Conta de serviço do Sql existente** | A conta de utilizador de domínio que está a ser utilizada para controlar o serviço SQL Server. Estas informações podem ser encontradas com o [ **Gestor de configuração do SQL Server**](https://docs.microsoft.com/sql/relational-databases/sql-server-configuration-manager?view=sql-server-2017). |
   | **Palavra-passe do serviço do SQL** | A palavra-passe utilizada pela conta de utilizador do domínio que controla o serviço SQL Server. |
   | &nbsp; | &nbsp; |

1. Se concordar com os termos e condições, selecione a caixa de verificação junto a **concordo com os termos e condições indicados acima** e selecione **Compra** para finalizar a implementação do modelo de início rápido. 
1. Para monitorizar a implementação, selecione a implementação a partir da **notificações** bell ícone na sua faixa de navegação superior, ou navegar para sua **grupo de recursos** no portal do Azure, selecione  **Implementações** no **definições** campo e selecione a implementação de 'Template'. 

## <a name="step-2---manually-create-the-availability-group"></a>Passo 2 - criar manualmente o grupo de disponibilidade 
Criar manualmente o grupo de disponibilidade, tal como faria normalmente, através de um [PowerShell](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell?view=sql-server-2017), [SQL Server Management Studio](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio?view=sql-server-2017) ou [Transact-SQL](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/create-an-availability-group-transact-sql?view=sql-server-2017). 

  >[!IMPORTANT]
  > Fazer **não** criar um serviço de escuta neste momento, porque é um processo automatizado pelos **101-sql-vm-aglistener-setup** modelo de início rápido no passo 4. 

## <a name="step-3---manually-create-the-internal-load-balancer-ilb"></a>Passo 3 - criar manualmente o Balanceador de carga interno (ILB)
O Always On (AG) serviço de escuta requer um balanceador de carga de Azure interno (ILB). O ILB fornece um endereço IP "flutuante" para o serviço de escuta de AG que permite que mais rápida de ativação pós-falha e restabelecimento de ligação. Se as VMs do SQL Server num grupo de disponibilidade fazem parte do mesmo conjunto de disponibilidade, pode utilizar um balanceador de carga básico; caso contrário, terá de utilizar um balanceador de carga Standard.  **O ILB deve estar na mesma vNet como as instâncias de VM do SQL Server.** O ILB apenas tem de ser criado, o restante da configuração (como o conjunto back-end, a sonda de estado de funcionamento e balanceamento de carga regras) é processado pela **101-sql-vm-aglistener-setup** modelo de início rápido no passo 4. 

1. No portal do Azure, abra o grupo de recursos que contém as máquinas virtuais do SQL Server. 
2. No grupo de recursos, clique em **adicionar**.
3. Procure **Balanceador de carga** e, em seguida, nos resultados da pesquisa, selecione **Balanceador de carga**, que é publicado pela **Microsoft**.
4. Sobre o **Balanceador de carga** painel, clique em **criar**.
5. Na **criar Balanceador de carga** diálogo caixa, configurar o Balanceador de carga da seguinte forma:

   | Definição | Valor |
   | --- | --- |
   | **Nome** |Um nome de texto que representa o Balanceador de carga. Por exemplo, **sqlLB**. |
   | **Tipo** |**Interno**: A maioria das implementações utilizar um balanceador de carga interno, que permite que aplicativos dentro da mesma rede virtual ligar ao grupo de disponibilidade.  </br> **Externo**: Permite que os aplicativos para se ligar ao grupo de disponibilidade por meio de uma ligação de Internet pública. |
   | **Rede virtual** |Selecione a rede virtual que as instâncias do SQL Server estão em. |
   | **Sub-rede** |Selecione a sub-rede que instâncias do SQL Server estão em. |
   | **Atribuição de endereços IP** |**Estático** |
   | **Endereço IP privado** |Especifique um endereço IP disponível na sub-rede. Utilize este endereço IP quando criar um serviço de escuta no cluster.|
   | **Subscrição** |Se tiver várias subscrições, este campo poderá ser apresentada. Selecione a subscrição que pretende associar este recurso. Normalmente é a mesma subscrição que todos os recursos para o grupo de disponibilidade. |
   | **Grupo de recursos** |Selecione o grupo de recursos que as instâncias do SQL Server estão em. |
   | **Localização** |Selecione a localização do Azure que as instâncias do SQL Server estão em. |
   | &nbsp; | &nbsp; |

6. Selecione **Criar**. 


  >[!NOTE]
  > O recurso de IP público para cada VM do SQL Server deve ter um SKU standard para ser compatível com o Balanceador de carga Standard. Para determinar o SKU de recurso de IP público da VM, navegue até à sua **grupo de recursos**, selecione seu **endereço IP público** recurso para o SQL Server VM pretendida e localize o valor sob **SKU**  das **descrição geral** painel. 

## <a name="step-4---create-the-ag-listener-and-configure-the-ilb-with-the-quickstart-template"></a>Passo 4 - criar o serviço de escuta de AG e configurar o ILB com o modelo de início rápido

Criar o serviço de escuta do grupo de disponibilidade e configurar o Balanceador de carga interno (ILB) automaticamente com o **101-sql-vm-aglistener-setup** modelo de início rápido à medida que Aprovisiona Microsoft.SqlVirtualMachine/Sql Virtual Recurso de grupos/serviço de escuta de máquina. O **101-sql-vm-aglistener-setup** modelo de início rápido, via o fornecedor de recursos de VM do SQL Server, faz as seguintes ações:

 - Configura as definições de rede para o cluster e o ILB. 
 - Configura o conjunto de back-end do ILB, a sonda de estado de funcionamento e balanceamento de carga regras.
 - Cria o serviço de escuta de AG com o endereço IP indicado e o nome.

Para configurar o ILB e criar o serviço de escuta de AG, faça o seguinte:
1. Navegue para o [ **101-sql-vm-aglistener-setup** ](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-aglistener-setup) modelo de início rápido e selecione **implementar no Azure** para iniciar o modelo de início rápido no portal do Azure.
1. Preencha os campos necessários para configurar o ILB e criar o serviço de escuta do grupo de disponibilidade. Os campos opcionais podem ser deixados em branco. 

    A tabela seguinte mostra os valores necessários para o modelo: 

   | **Campo** | Valor |
   | --- | --- |
   |**Grupo de recursos** | O grupo de recursos onde as VMs do SQL Server e o grupo de disponibilidade existem. | 
   |**Nome do Cluster de ativação pós-falha existente** | O nome do que as suas VMs do SQL Server estão associados ao cluster. |
   | **Grupo de disponibilidade existente do Sql**| O nome do grupo de disponibilidade que as suas VMs do SQL Server são uma parte. |
   | **Lista de Vm existente** | Os nomes das VMs do SQL Server que fazem parte do grupo de disponibilidade mencionado anteriormente. Os nomes devem ser separados por uma vírgula e um espaço (ex: SQLVM1, SQLVM2). |
   | **Nome de domínio completamente qualificado existente** | O FQDN existente para o domínio no qual residem as suas VMs do SQL Server. |
   | **Serviço de escuta** | O nome DNS que pretende atribuir ao serviço de escuta. Por predefinição, este modelo especifica o nome "aglistener', mas pode ser alterado. |
   | **Porta do serviço de escuta** | A porta que pretende que o serviço de escuta para utilizar. Normalmente, essa porta deve ser a porta predefinida 1433 e, como tal, este é o número de porta especificado por este modelo. No entanto, se a porta predefinida tiver sido alterada, em seguida, a porta do serviço de escuta use esse valor em vez disso. | 
   | **Vnet existente** | O nome da vNet onde residem as VMs do SQL Server e o ILB. |
   | **Sub-rede existente** | O *nome* da sub-rede interna das suas VMs do SQL Server (ex: predefinição). Este valor pode ser determinado ao navegar para o seu **grupo de recursos**, selecionar seu **vNet**, selecionar **sub-redes** sob o **definições**painel e copiar o valor sob **nome**. |
   | **Balanceador de carga interno existente** | O nome do ILB que criou no passo 3. |
   | **Porta de sonda** | A porta de sonda que pretende que o ILB para utilizar. O modelo utiliza 59999 por predefinição, mas este valor pode ser alterado. |
   | &nbsp; | &nbsp; |

1. Se concordar com os termos e condições, selecione a caixa de verificação junto a **concordo com os termos e condições indicados acima** e selecione **Compra** para finalizar a implementação do modelo de início rápido. 
1. Para monitorizar a implementação, selecione a implementação a partir da **notificações** bell ícone na sua faixa de navegação superior, ou navegar para sua **grupo de recursos** no portal do Azure, selecione  **Implementações** no **definições** campo e selecione a implementação de 'Template'. 

  >[!NOTE]
  >Se a implementação falhar a meio caminho pela, precisará manualmente [remover o serviço de escuta recém-criado](#remove-availability-group-listener) com o PowerShell antes de voltar a implementar a **101-sql-vm-aglistener-setup** modelo de início rápido. 

## <a name="remove-availability-group-listener"></a>Remover o serviço de escuta de grupo de disponibilidade
Se precisar de remover o serviço de escuta configurado pelo modelo mais tarde, deve percorrer o fornecedor de recursos de VM do SQL Server. Uma vez que o serviço de escuta é registrado por meio do Provedor de recursos de VM do SQL Server, basta excluí-lo através do SQL Server Management Studio é insuficiente. Na verdade, ele deve ser eliminado por meio do Provedor de recursos de VM do SQL Server com o PowerShell. Se o fizer, remove os metadados do serviço de escuta de AG do fornecedor de recursos de VM do SQL Server e exclui fisicamente o serviço de escuta do grupo de disponibilidade. 

O fragmento de código seguinte elimina o serviço de escuta do grupo de disponibilidade do SQL a partir de ambos os o fornecedor de recursos do SQL e do seu grupo de disponibilidade: 

```PowerShell
# Remove the AG listener
# example: Remove-AzureRmResource -ResourceId '/subscriptions/a1a11a11-1a1a-aa11-aa11-1aa1a11aa11a/resourceGroups/SQLAG-RG/providers/Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/Cluster/availabilitygrouplisteners/aglistener' -Force
Remove-AzureRmResource -ResourceId '/subscriptions/<SubscriptionID>/resourceGroups/<resource-group-name>/providers/Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/<cluster-name>/availabilitygrouplisteners/<listener-name>' -Force
```
 
## <a name="known-issues-and-errors"></a>Erros e problemas conhecidos
Esta secção aborda alguns problemas conhecidos e respectivas resoluções possíveis. 

### <a name="availability-group-listener-for-availability-group-ag-name-already-exists"></a>Serviço de escuta de grupo de disponibilidade para o grupo de disponibilidade '\<AG-Name >' já existe
O grupo de disponibilidade selecionado utilizado no serviço de escuta de AG modelo de início rápido do Azure já contém um serviço de escuta e fisicamente dentro do grupo de disponibilidade ou como metadados dentro do fornecedor de recursos de VM do SQL Server.  Remover o serviço de escuta usando [PowerShell](#remove-availability-group-listener) antes de voltar a implementar a **101-sql-vm-aglistener-setup** modelo de início rápido. 

### <a name="connection-only-works-from-primary-replica"></a>Ligação só funciona da réplica primária
Este comportamento é provável que uma falha de **101-sql-vm-aglistener-setup** implementação do modelo mantendo a configuração de ILB num estado inconsistente. Certifique-se de que o conjunto de back-end lista o conjunto de disponibilidade e de que existem regras para a sonda de estado de funcionamento e para as regras de balanceamento de carga. Se nada estiver em falta, em seguida, a configuração de ILB é um estado inconsistente. 

Para resolver esse comportamento, remover o serviço de escuta usando [PowerShell](#remove-availability-group-listener)eliminar o Balanceador de carga interno através do portal do Azure e recomeçar [passo 3](#step-3---manually-create-the-internal-load-balanced-ilb). 

### <a name="badrequest---only-sql-virtual-machine-list-can-be-updated"></a>BadRequest - lista de máquinas virtuais do SQL só pode ser atualizado
Este erro pode ocorrer ao implementar o **101-sql-vm-aglistener-setup** modelo se o serviço de escuta foi eliminado através do SQL Server Management Studio (SSMS), mas não foi eliminado do fornecedor de recursos de VM do SQL Server. A eliminar o serviço de escuta através do SSMS não remove os metadados do serviço de escuta do fornecedor de recursos de VM do SQL Server; o serviço de escuta tem de ser eliminado do fornecedor de recursos usando [PowerShell](#remove-availability-group-listener). 


## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações, veja os artigos seguintes: 

* [Descrição geral da VM do SQL Server](virtual-machines-windows-sql-server-iaas-overview.md)
* [FAQ de VM do SQL Server](virtual-machines-windows-sql-server-iaas-faq.md)
* [Documentação de orientação de preços de VM do SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Notas de versão de VM do SQL Server](virtual-machines-windows-sql-server-iaas-release-notes.md)
* [Modelos de licenciamento mudar para uma VM do SQL Server](virtual-machines-windows-sql-ahb.md)



