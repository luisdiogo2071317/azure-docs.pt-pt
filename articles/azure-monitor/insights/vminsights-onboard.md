---
title: Carregar Monitor do Azure para VMs (pré-visualização) | Documentos da Microsoft
description: Este artigo descreve como sua integração e configure o Azure Monitor para as VMs para que possa começar a entender sobre a execução da sua aplicação distribuída e quais problemas de estado de funcionamento foram identificados.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2018
ms.author: magoedte
ms.openlocfilehash: 4e374528a0fa757458e7e4881714370937b56f9c
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52998296"
---
# <a name="how-to-onboard-the-azure-monitor-for-vms-preview"></a>Como carregar para o Azure Monitor para VMs (pré-visualização)
Este artigo descreve como configurar o Azure Monitor para as VMs monitorizar o estado de funcionamento do sistema operativo de suas máquinas virtuais do Azure e conjuntos de dimensionamento de máquinas virtuais e máquinas virtuais no seu ambiente, incluindo a deteção e o mapeamento de dependências de aplicações que pode estar alojado nos mesmos.  

Ativar o Azure Monitor para VMs é realizada através de um dos seguintes métodos e detalhes sobre como utilizar cada método são fornecidas posteriormente neste artigo.  

* Uma única máquina virtual do Azure, selecionando **Insights (pré-visualização)** diretamente a partir da VM.
* Várias VMs do Azure com o Azure Policy para garantir a existentes e novas VMs avaliadas têm as dependências necessárias instaladas e estão configuradas corretamente.  VMs não compatíveis são relatadas para que possa decidir com base no que não está em conformidade, como pretende remediar.  
* Os conjuntos de dimensionamento de VMs do Azure ou numa máquina virtual múltiplas entre uma subscrição especificada ou o grupo de recursos com o PowerShell.

## <a name="prerequisites"></a>Pré-requisitos
Antes de começar, certifique-se de que compreende o seguinte, conforme descrito nas subsecções abaixo.

### <a name="log-analytics"></a>Log Analytics 

Atualmente é suportada uma área de trabalho do Log Analytics nas seguintes regiões:

  - EUA Centro-Oeste  
  - EUA Leste  
  - Europa Ocidental  
  - Sudeste asiático<sup>1</sup>  

<sup>1</sup> nesta região não suporta atualmente a funcionalidade de estado de funcionamento do Monitor do Azure para VMs   

>[!NOTE]
>Máquinas virtuais do Azure pode ser integrado em qualquer região e não estão limitadas para as regiões suportadas para a área de trabalho do Log Analytics.
>

Se não tiver uma área de trabalho, pode criar ele por meio [CLI do Azure](../../azure-monitor/learn/quick-create-workspace-cli.md), da funcionalidade [PowerShell](../../azure-monitor/learn/quick-create-workspace-posh.md), no [portal do Azure](../../azure-monitor/learn/quick-create-workspace.md), ou com [doAzureResourceManager](../../azure-monitor/platform/template-workspace-configuration.md).  Se pretende ativar a monitorização para uma única VM do Azure no portal do Azure, terá a opção para criar uma área de trabalho durante este processo.  

Ativar a solução para uma escala de cenário primeiro exige a configuração o seguinte na sua área de trabalho do Log Analytics:

* Instalar o **ServiceMap** e **InfrastructureInsights** soluções. Só pode ser realizada com um modelo Azure Resource Manager fornecido neste artigo.   
* Configure a área de trabalho do Log Analytics para recolher contadores de desempenho.

Para configurar a sua área de trabalho para no cenário de dimensionamento, veja [área de trabalho do Log Analytics do programa de configuração para na implementação de escala](#setup-log-analytics-workspace).

### <a name="supported-operating-systems"></a>Sistemas operativos suportados

A tabela seguinte lista os sistemas operativos Windows e Linux que são suportados com o Azure Monitor para as VMs.  Uma lista completa com detalhes sobre que o sistema operacional Linux principal e secundária da versão e versões de kernel suportadas são fornecidas posteriormente nesta secção.

|Versão do SO |Desempenho |Maps |Estado de Funcionamento |  
|-----------|------------|-----|-------|  
|Windows Server 2016 1803 | X | X | X |
|Windows Server 2016 | X | X | X |  
|Windows Server 2012 R2 | X | X | |  
|Windows Server 2012 | X | X | |  
|Windows Server 2008 R2 | X | X| |  
|RHEL 7, 6| X | X| X |  
|Ubuntu 18.04, 16.04, 14.04 | X | X | X |  
|Cento SO Linux 7, 6 | X | X | X |  
|SLES 12 | X | X | X |  
|Oracle Linux 7 | X<sup>1</sup> | | X |  
|Oracle Linux 6 | X | X | X |  
|Debian 9.4, 8 | X<sup>1</sup> | | X | 

<sup>1</sup> funcionalidade do desempenho do Azure Monitor para as VMs só está disponível a partir do Azure Monitor, não está disponível quando aceder a partir do painel à esquerda da VM do Azure diretamente.  

>[!NOTE]
>As seguintes informações aplicam-se para oferecer suporte a do sistema operativo Linux:  
> - Apenas as versões de kernel padrão e SMP Linux são suportadas.  
> - As versões kernel não padrão, tais como PAE e Xen, não são suportadas para qualquer distribuição de Linux. Por exemplo, um sistema com a cadeia de versão de "2.6.16.21-0.8-xen" não é suportado.  
> - Kernels personalizados, incluindo recompilações de kernels padrão, não são suportados.  
> - O kernel do CentOSPlus kernel não é suportado.  


#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| Versão do SO | Versão de kernel |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |
| 7.4 | 3.10.0-693 |
| 7.5 | 3.10.0-862 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| Versão do SO | Versão de kernel |
|:--|:--|
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6.7 | 2.6.32-573 |
| 6.8 | 2.6.32-642 |
| 6.9 | 2.6.32-696 |

#### <a name="ubuntu-server"></a>Ubuntu Server

| Versão do SO | Versão de kernel |
|:--|:--|
| Ubuntu 18.04 | o kernel 4.15. * |
| Ubuntu 16.04.3 | o kernel 4.15. * |
| 16.04 | 4.4.\*<br>4.8.\*<br>4.10.\*<br>4.11.\*<br>4.13.\* |
| 14.04 | 3.13.\*<br>4.4.\* |

#### <a name="oracle-enterprise-linux-6-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux 6 com o Kernel de Indestrutíveis Enterprise
| Versão do SO | Versão de kernel
|:--|:--|
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-enterprise-linux-5-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux 5 com o Kernel de Indestrutíveis Enterprise

| Versão do SO | Versão de kernel
|:--|:--|
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-12-enterprise-server"></a>SUSE Linux 12 Enterprise Server

| Versão do SO | Versão de kernel
|:--|:--|
|12 SP2 | 4.4. * |
|12 SP3 | 4.4. * |

### <a name="microsoft-dependency-agent"></a>Agente de Dependency da Microsoft
O Azure Monitor para o mapa de VMs obtém seus dados do agente do Microsoft Dependency. O agente de dependência depende do agente para estabelecer ligação com o Log Analytics e, portanto, um sistema tem de ter o agente do Log Analytics instalada e configurada com o agente de dependência do Log Analytics. Quando ativa do Azure Monitor para as VMs para uma única VM do Azure ou ao utilizar os métodos para na implementação de escala, a extensão de agente de dependência de VM do Azure é utilizada para instalar o agente como parte dessa experiência de integração. Com um ambiente híbrido, o agente de dependência pode ser transferido e instalado manualmente ou usando um método de implantação automatizadas a essas máquinas virtuais alojadas fora do Azure.  

A tabela seguinte descreve as origens ligadas que a funcionalidade de mapa suporta num ambiente híbrido.

| Origem ligada | Suportadas | Descrição |
|:--|:--|:--|
| Agentes do Windows | Sim | Para além da [agente do Log Analytics para Windows](../../azure-monitor/platform/log-analytics-agent.md), agentes do Windows exigem o agente do Microsoft Dependency. Veja os [sistemas operativos suportados](#supported-operating-systems) para obter uma lista completa das versões do sistema operativo. |
| Agentes do Linux | Sim | Para além da [agente do Log Analytics para Linux](../../azure-monitor/platform/log-analytics-agent.md), o agente do Microsoft Dependency necessitam de agentes do Linux. Veja os [sistemas operativos suportados](#supported-operating-systems) para obter uma lista completa das versões do sistema operativo. |
| Grupo de gestão do System Center Operations Manager | Não | |  

O agente de dependência pode ser transferido a partir da seguinte localização.

| Ficheiro | SO | Versão | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.7.1 | 55030ABF553693D8B5112569FB2F97D7C54B66E9990014FC8CC43EFB70DE56C6 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.7.1 | 43C75EF0D34471A0CBCE5E396FFEEF4329C9B5517266108FA5D6131A353D29FE |

## <a name="role-based-access-control"></a>Controlo de acesso baseado em funções
O seguinte acesso tem de ser concedido aos seus utilizadores para habilitar e aceder a recursos no Azure Monitor para as VMs.  
  
- Para ativar a solução, terá de ser adicionado como membro da função de Contribuidor do Log Analytics.  

- Para ver o desempenho, o estado de funcionamento e mapear dados, terá de adicionado como um membro da função do leitor de monitorização para a VM do Azure e a área de trabalho do Log Analytics configurado com o Azure Monitor para as VMs.   

Para obter mais informações sobre como controlar o acesso a uma área de trabalho do Log Analytics, consulte [gerir áreas de trabalho](../../azure-monitor/platform/manage-access.md).

## <a name="enable-from-the-azure-portal"></a>Ativar a partir do portal do Azure
Para ativar a monitorização da sua VM do Azure no portal do Azure, efetue o seguinte:

1. Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com). 
2. No portal do Azure, selecione **máquinas virtuais**. 
3. Na lista, selecione uma VM. 
4. Na página de VM, na **monitorização** secção, selecione **Insights (pré-visualização)**.
5. Sobre o **Insights (pré-visualização)** página, selecione **Experimente agora o**.

    ![Ativar o Azure Monitor para as VMs para uma VM](./media/vminsights-onboard/enable-vminsights-vm-portal-01.png)

5. Sobre o **integração de informações do Azure Monitor** página, se tiver um existentes do Log Analytics área de trabalho na mesma subscrição, selecione-o na lista pendente.  A lista preselects a área de trabalho predefinida e a localização que a máquina virtual é implementada na subscrição. 

    >[!NOTE]
    >Se quiser criar uma nova área de trabalho do Log Analytics para armazenar os dados de monitorização da VM, siga as instruções em [criar uma área de trabalho do Log Analytics](../../azure-monitor/learn/quick-create-workspace.md) em uma das regiões suportadas listadas anteriormente.   

Depois de ativar a monitorização, poderá demorar cerca de 10 minutos antes de poder visualizar as métricas de estado de funcionamento para a máquina virtual. 

![Ativar o Azure Monitor para monitorização de processamento da implementação de VMs](./media/vminsights-onboard/onboard-vminsights-vm-portal-status.png)


## <a name="on-boarding-at-scale"></a>Introdução à escala
Nas instruções nesta secção sobre como efetuar na implementação de escala do Azure Monitor para VMs com a política do Azure ou com o Azure PowerShell.  

Resumidos são os passos que necessários para efetuar para configurar previamente a sua área de trabalho do Log Analytics antes de poder prosseguir com a integração suas máquinas virtuais.

1. Criar uma nova área de trabalho se não ainda existir que pode ser utilizada para suportar o Azure Monitor de VMs. Revisão [gerir áreas de trabalho](../../azure-monitor/platform/manage-access.md?toc=/azure/azure-monitor/toc.json) antes de criar uma nova área de trabalho para compreender as considerações de custo, gestão e conformidade antes de continuar.       
2. Ative os contadores de desempenho na área de trabalho para a coleção em Linux e VMs do Windows.
3. Instalar e ativar a **ServiceMap** e **InfrastructureInsights** solução na sua área de trabalho.  

### <a name="setup-log-analytics-workspace"></a>Configurar área de trabalho do Log Analytics
Se não tiver uma área de trabalho do Log Analytics, reveja os métodos disponíveis sugeridos sob o [pré-requisitos](#log-analytics) secção para criar um.  

#### <a name="enable-performance-counters"></a>Ativar os contadores de desempenho
Se a área de trabalho do Log Analytics referenciada pela solução não está configurada para recolher já os contadores de desempenho necessários para a solução, terão de ser ativada. Pode fazê-lo manualmente, conforme descrito [aqui](../../azure-monitor/platform/data-sources-performance-counters.md), ou por baixar e executar um script do PowerShell disponível a partir do [galeria do Powershell do Azure](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1).
 
#### <a name="install-the-servicemap-and-infrastructureinsights-solutions"></a>Instalar as soluções ServiceMap e InfrastructureInsights
Esse método inclui um modelo JSON que especifica a configuração para permitir que os componentes da solução para a área de trabalho do Log Analytics.  

Se não estiver familiarizado com o conceito de implementar recursos com um modelo, consulte:
* [Implementar recursos com modelos do Resource Manager e o Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md)
* [Implementar recursos com modelos do Resource Manager e a CLI do Azure](../../azure-resource-manager/resource-group-template-deploy-cli.md) 

Se optar por utilizar a CLI do Azure, tem primeiro de instalar e utilizar a CLI localmente. Tem de executar a CLI do Azure versão 2.0.27 ou posterior. Para identificar a versão, execute `az --version`. Se precisar de instalar ou atualizar a CLI do Azure, veja [instalar a CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). 

1. Copie e cole a seguinte sintaxe JSON no seu ficheiro:

    ```json
    {

    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "WorkspaceName": {
            "type": "string"
        },
        "WorkspaceLocation": {
            "type": "string"
        }
    },
    "resources": [
        {
            "apiVersion": "2017-03-15-preview",
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('WorkspaceName')]",
            "location": "[parameters('WorkspaceLocation')]",
            "resources": [
                {
                    "apiVersion": "2015-11-01-preview",
                    "location": "[parameters('WorkspaceLocation')]",
                    "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    },

                    "plan": {
                        "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                        "publisher": "Microsoft",
                        "product": "[Concat('OMSGallery/', 'ServiceMap')]",
                        "promotionCode": ""
                    }
                },
                {
                    "apiVersion": "2015-11-01-preview",
                    "location": "[parameters('WorkspaceLocation')]",
                    "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    },
                    "plan": {
                        "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                        "publisher": "Microsoft",
                        "product": "[Concat('OMSGallery/', 'InfrastructureInsights')]",
                        "promotionCode": ""
                    }
                }
            ]
        }
    ]
    ```

2. Guarde este ficheiro como **installsolutionsforvminsights.json** para uma pasta local.
3. Edite os valores dos **WorkspaceName**, **ResourceGroupName**, e **WorkspaceLocation**.  O valor para **WorkspaceName** é o ID de recurso completo da sua área de trabalho do Log Analytics, que inclui o nome de área de trabalho e o valor de **WorkspaceLocation** é a região a área de trabalho está definida.
4. Está pronto para implementar este modelo com o seguinte comando do PowerShell:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    A alteração de configuração pode demorar alguns minutos a concluir. Quando for concluído, será apresentada uma mensagem que é semelhante ao seguinte e inclui o resultado:

    ```powershell
    provisioningState       : Succeeded
    ```

### <a name="enable-using-azure-policy"></a>Ativar com o Azure Policy
Para ativar o Azure Monitor para VMs em escala que assegura a conformidade consistente e a ativação automática para novas VMs aprovisionado, [do Azure Policy](../../azure-policy/azure-policy-introduction.md) é recomendado. Estas políticas:

* Implementar o agente do Log Analytics e o agente de dependência 
* Relatório sobre os resultados de compatibilidade 
* Remediar de VMs incompatíveis

Ativar o Azure Monitor para as VMs por meio da diretiva com o seu inquilino exige: 

- Atribuir iniciativa a um âmbito – grupo de gestão, subscrição ou grupo de recursos 
- Verificação e resolução de resultados de compatibilidade  

Para obter mais informações sobre a atribuição de política do Azure, consulte [descrição geral da política do Azure](../../governance/policy/overview.md#policy-assignment) e reveja a [descrição geral de grupos de gestão](../../governance/management-groups/index.md) antes de continuar.  

A tabela seguinte lista as definições de política fornecidas.  

|Nome |Descrição |Tipo |  
|-----|------------|-----|  
|[Pré-visualização]: Ativar o Azure Monitor para VMs |Ative o Azure Monitor para as máquinas virtuais (VMs) no âmbito especificado (grupo de gestão, subscrição ou grupo de recursos). Aceita a área de trabalho do Log Analytics como parâmetro. |Iniciativa |  
|[Pré-visualização]: implementação de agente de dependência de auditoria – imagem de VM (SO) não listados |As VMs são reportadas como não conformes se a Imagem de VM (SO) não estiver na lista definida e o agente não estiver instalado. |Política |  
|[Pré-visualização]: implementação de agente de análise de registo de auditoria – imagem de VM (SO) não listados |As VMs são reportadas como não conformes se a Imagem de VM (SO) não estiver na lista definida e o agente não estiver instalado. |Política |  
|[Pré-visualização]: implementar o agente de dependência para VMs do Linux |Implemente o Dependency Agent para as VM do Linux se a Imagem de VM (SO) estiver na lista definida e o agente não estiver instalado. |Política |  
|[Pré-visualização]: implementar o agente de dependência para VMs do Windows |Implemente o Dependency Agent para as VM do Windows se a Imagem de VM (SO) estiver na lista definida e o agente não estiver instalado. |Política |  
|[Pré-visualização]: implementar o agente de análise de registo para VMs do Linux |Implemente o Agente do Log Analytics para VMs do Linux se a Imagem de VM (SO) estiver na lista definida e o agente não estiver instalado. |Política |  
|[Pré-visualização]: implementar o agente de análise de registo para VMs do Windows |Implemente o Agento do Log Analytics para as VMs do Windows se a Imagem de VM (SO) estiver na lista definida e o agente não estiver instalado. |Política |  

Política autónoma (não incluída com a iniciativa) 

|Nome |Descrição |Tipo |  
|-----|------------|-----|  
|[Pré-visualização]: auditar a área de trabalho do Log Analytics para VM - erro de correspondência de relatório |Relatórios VMs como não conforme se eles não estejam a registar para a área de trabalho de LA especificada na atribuição de iniciativa de política /. |Política |

#### <a name="assign-azure-monitor-initiative"></a>Atribuir iniciativa do Azure Monitor
Com esta versão inicial, só pode criar a atribuição de política do portal do Azure. Para compreender como concluir estes passos, veja [criar uma atribuição de política do portal do Azure](../../governance/policy/assign-policy-portal.md). 

1. Inicie o serviço Azure Policy no portal do Azure ao clicar em **Todos os serviços** e, em seguida, ao pesquisar e selecionar **Policy**. 
2. Selecione **Atribuições** no lado esquerdo da página Azure Policy. Uma atribuição é uma política que foi atribuída para ter lugar num âmbito específico.
3. Selecione **atribuir iniciativa** na parte superior do **política - atribuições** página.
4. Sobre o **atribuir iniciativa** página, selecione a **âmbito** ao clicar nas reticências e selecione o grupo de gestão, ou subscrição e, opcionalmente, um grupo de recursos. Um âmbito limita a atribuição de política no nosso caso, para um agrupamento de máquinas virtuais para a imposição. Clique em **selecionar** na parte inferior da **âmbito** página para guardar as alterações.
5. **Exclusões** permitem que omite um ou mais recursos do âmbito, que é opcional. 
6. Selecione o **definição de iniciativa** reticências para abrir a lista de definições disponíveis e selecione  **[pré-visualização] ativar o Azure Monitor para VMs** na lista e, em seguida, clique **Selecione**.
7. O **nome da atribuição** é automaticamente preenchido com o nome da iniciativa que selecionou, mas pode alterá-lo. Também pode adicionar uma **Descrição** opcional. **Atribuído por** é preenchido automaticamente com base em quem está conectado e este campo é opcional.
8. Selecione um **área de trabalho do Log Analytics** na lista pendente que está disponível na região suportada.

    >[!NOTE]
    >Se a área de trabalho está fora do âmbito da atribuição, tem de conceder **Contribuidor do Log Analytics** permissões para o ID de Principal. a atribuição de política Se não fizer isso poderá ver uma falha de implementação, tais como: `The client '343de0fe-e724-46b8-b1fb-97090f7054ed' with object id '343de0fe-e724-46b8-b1fb-97090f7054ed' does not have authorization to perform action 'microsoft.operationalinsights/workspaces/read' over scope ... ` revisão [como configurar manualmente a identidade gerida](../../governance/policy/how-to/remediate-resources.md#manually-configure-the-managed-identity) para conceder acesso.
    >

9. Observe que o **identidade gerido** opção seja marcada. Isso é verificado quando a iniciativa que está sendo atribuída inclui uma política com o efeito de deployIfNotExists. Do **localização de gerir a identidade** lista pendente, selecione a região adequada.  
10. Clique em **Atribuir**.

#### <a name="review-and-remediate-the-compliance-results"></a>Rever e corrigir os resultados de compatibilidade 

Pode saber como rever os resultados de compatibilidade, lendo [identificar os resultados de não conformidade](../../governance/policy/assign-policy-portal.md#identify-non-compliant-resources). Selecione **conformidade** no lado esquerdo da página e localize a  **[pré-visualização] ativar o Azure Monitor para VMs** iniciativa que não são compatíveis pela atribuição que criou.

![Conformidade com a política para VMs do Azure](./media/vminsights-onboard/policy-view-compliance-01.png)

As VMs com base nos resultados das políticas incluídos com a iniciativa, são comunicadas como não conforme nos seguintes cenários:  
  
1. Log Analytics ou o agente de dependência não é implementada.  
   Esta situação é normal para um âmbito com VMs existentes. Para o resolver, [criar tarefas de atualização](../../governance/policy/how-to/remediate-resources.md) numa política de conformidade para implantar os agentes necessários.    
 
    - [Pré-visualização]: Deploy Dependency Agent for Linux VMs   
    - [Pré-visualização]: Deploy Dependency Agent for Windows VMs  
    - [Pré-visualização]: Deploy Log Analytics Agent for Linux VMs  
    - [Pré-visualização]: Deploy Log Analytics Agent for Windows VMs  

2. Imagem de VM (SO) não está na lista identificada na definição de política.  
   Critérios da política de implementação inclui apenas as VMs que são implementadas a partir de imagens de VM do Azure bem conhecidas. Verifique a documentação, se o SO da VM for suportado ou não. Se não for, precisa duplicar a política de implementação e atualização/modificá-lo para criar a imagem em conformidade. 
  
    - [Pré-visualização]: implementação de agente de dependência de auditoria – imagem de VM (SO) não listados  
    - [Pré-visualização]: implementação de agente de análise de registo de auditoria – imagem de VM (SO) não listados

3. VMs não estejam a registar para a área de trabalho LA especificada.  
É possível que algumas VMs no âmbito da iniciativa estão a iniciar sessão a uma área de trabalho LA diferente a partir de uma vez especificada na atribuição de política. Esta política é uma ferramenta para identificar quais as VMs que comunicam com uma área de trabalho de não conformidade.  
 
    - [Pré-visualização]: Audit Log Analytics Workspace for VM - Report Mismatch  

### <a name="enable-with-powershell"></a>Ativar com o PowerShell
Para ativar o Azure Monitor para as VMs para várias VMs ou conjuntos de dimensionamento de máquina virtual, pode utilizar um script do PowerShell fornecido - [Install-VMInsights.ps1](https://www.powershellgallery.com/packages/Install-VMInsights/1.0) disponível a partir da galeria do PowerShell do Azure para concluir esta tarefa.  Este script irá iterar em cada virtual máquina e a máquina virtual conjunto de dimensionamento na sua subscrição, no grupo de recursos de âmbito especificado pelo *ResourceGroup*, ou a uma único máquina virtual VM ou conjunto de dimensionamento especificado pelo *Nome*.  Para cada conjunto de dimensionamento VM ou numa máquina virtual, o script verifica se a extensão da VM já está instalada e se não a tentativa de reinstalá-lo.  Caso contrário, ele passa a instalar as extensões de VM de agente de dependência e do Log Analytics.   

Este script requer o Azure PowerShell 5.7.0 de versão do módulo ou posterior. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Connect-AzureRmAccount` para criar uma ligação com o Azure.

Para obter ajuda sobre o script, pode executar `Get-Help` para obter uma lista de detalhes do argumento e a utilização de exemplo.   

```powershell
Get-Help .\Install-VMInsights.ps1 -Detailed

SYNOPSIS
    This script installs VM extensions for Log Analytics and Dependency Agent as needed for VM Insights.


SYNTAX
    .\Install-VMInsights.ps1 [-WorkspaceId] <String> [-WorkspaceKey] <String> [-SubscriptionId] <String> [[-ResourceGroup]
    <String>] [[-Name] <String>] [[-PolicyAssignmentName] <String>] [-ReInstall] [-TriggerVmssManualVMUpdate] [-Approve] [-WorkspaceRegion] <String>
    [-WhatIf] [-Confirm] [<CommonParameters>]


DESCRIPTION
    This script installs or re-configures following on VM's and VM Scale Sets:
    - Log Analytics VM Extension configured to supplied Log Analytics Workspace
    - Dependency Agent VM Extension

    Can be applied to:
    - Subscription
    - Resource Group in a Subscription
    - Specific VM/VM Scale Set
    - Compliance results of a policy for a VM or VM Extension

    Script will show you list of VM's/VM Scale Sets that will apply to and let you confirm to continue.
    Use -Approve switch to run without prompting, if all required parameters are provided.

    If the extensions are already installed will not install again.
    Use -ReInstall switch if you need to for example update the workspace.

    Use -WhatIf if you would like to see what would happen in terms of installs, what workspace configured to, and status of the extension.


PARAMETERS
    -WorkspaceId <String>
        Log Analytics WorkspaceID (GUID) for the data to be sent to

    -WorkspaceKey <String>
        Log Analytics Workspace primary or secondary key

    -SubscriptionId <String>
        SubscriptionId for the VMs/VM Scale Sets
        If using PolicyAssignmentName parameter, subscription that VM's are in

    -ResourceGroup <String>
        <Optional> Resource Group to which the VMs or VM Scale Sets belong to

    -Name <String>
        <Optional> To install to a single VM/VM Scale Set

    -PolicyAssignmentName <String>
        <Optional> Take the input VM's to operate on as the Compliance results from this Assignment
        If specified will only take from this source.

    -ReInstall [<SwitchParameter>]
        <Optional> If VM/VM Scale Set is already configured for a different workspace, set this to change to the new workspace

    -TriggerVmssManualVMUpdate [<SwitchParameter>]
        <Optional> Set this flag to trigger update of VM instances in a scale set whose upgrade policy is set to Manual

    -Approve [<SwitchParameter>]
        <Optional> Gives the approval for the installation to start with no confirmation prompt for the listed VM's/VM Scale Sets

    -WorkspaceRegion <String>
        Region the Log Analytics Workspace is in
        Suported values: "East US","eastus","Southeast Asia","southeastasia","West Central US","westcentralus","West Europe","westeurope"
        For Health supported is: "East US","eastus","West Central US","westcentralus"

    -WhatIf [<SwitchParameter>]
        <Optional> See what would happen in terms of installs.
        If extension is already installed will show what workspace is currently configured, and status of the VM extension

    -Confirm [<SwitchParameter>]
        <Optional> Confirm every action

    <CommonParameters>
        This cmdlet supports the common parameters: Verbose, Debug,
        ErrorAction, ErrorVariable, WarningAction, WarningVariable,
        OutBuffer, PipelineVariable, and OutVariable. For more information, see
        about_CommonParameters (https:/go.microsoft.com/fwlink/?LinkID=113216).

    -------------------------- EXAMPLE 1 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup>

    Install for all VM's in a Resource Group in a subscription

    -------------------------- EXAMPLE 2 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup> -ReInstall

    Specify to ReInstall extensions even if already installed, for example to update to a different workspace

    -------------------------- EXAMPLE 3 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -PolicyAssignmentName a4f79f8ce891455198c08736 -ReInstall

    Specify to use a PolicyAssignmentName for source, and to ReInstall (move to a new workspace)
```

O exemplo seguinte demonstra como utilizar os comandos do PowerShell na pasta para ativar o Azure Monitor para VMs e compreender a saída esperada:

```powershell
$WorkspaceId = "<GUID>"
$WorkspaceKey = "<Key>"
$SubscriptionId = "<GUID>"
.\Install-VMInsights.ps1 -WorkspaceId $WorkspaceId -WorkspaceKey $WorkspaceKey -SubscriptionId $SubscriptionId -WorkspaceRegion eastus

Getting list of VM's or VM ScaleSets matching criteria specified

VM's or VM ScaleSets matching criteria:

db-ws-1 VM running
db-ws2012 VM running

This operation will install the Log Analytics and Dependency Agent extensions on above 2 VM's or VM Scale Sets.
VM's in a non-running state will be skipped.
Extension will not be re-installed if already installed. Use -ReInstall if desired, for example to update workspace

Confirm
Continue?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

db-ws-1 : Deploying DependencyAgentWindows with name DAExtension
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Deploying DependencyAgentWindows with name DAExtension
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Summary:

Already Onboarded: (0)

Succeeded: (4)
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Connected to different workspace: (0)

Not running - start VM to configure: (0)

Failed: (0)
```

## <a name="enable-for-hybrid-environment"></a>Ativar para o ambiente híbrido
Esta secção explica como a adicionar máquinas virtuais ou físicas computadores hospedados no seu datacenter ou outro ambiente de cloud para monitorização ao Azure Monitor para as VMs.  

O Azure Monitor para agente de dependência de mapa de VMs não transmitir todos os dados em si, e não requer alterações às firewalls ou portas. Os dados de mapa sempre são transmitidos pelo agente do Log Analytics para o serviço Azure Monitor, diretamente ou através da [Gateway de OMS](../../azure-monitor/platform/gateway.md) se as políticas de segurança de TI não permitir que os computadores na rede para ligar à Internet.

Reveja os requisitos e métodos de implementação para o [agente Linux do Log Analytics e o Windows](../../azure-monitor/platform/log-analytics-agent.md).  

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

Passos resumidos:

1. Instalar o agente do Log Analytics para Windows ou Linux
2. Transfira e instale o Azure Monitor para agente de dependência de mapa de VMs para [Windows](https://aka.ms/dependencyagentwindows) ou [Linux](https://aka.ms/dependencyagentlinux).
3. Ativar a recolha de contadores de desempenho
4. Carregar Monitor do Azure para VMs

### <a name="install-the-dependency-agent-on-windows"></a>Instale o agente de dependência no Windows 
O agente de dependência pode ser instalado manualmente em computadores Windows ao executar `InstallDependencyAgent-Windows.exe`. Se executar esse arquivo executável sem quaisquer opções, inicia um Assistente de configuração que pode seguir para instalar interativamente.  

>[!NOTE]
>São precisos privilégios de administrador para instalar ou desinstalar o agente.

A tabela seguinte realça os parâmetros específicos suportados pelo programa de configuração para o agente a partir da linha de comandos.  

| Parâmetro | Descrição |
|:--|:--|
| /? | Devolve uma lista das opções da linha de comandos. |
| /S | Efetue uma instalação automática sem interação do utilizador. |

Por exemplo, para executar o programa de instalação com o `/?` parâmetro, escreva `InstallDependencyAgent-Windows.exe /?`

Ficheiros para o agente de dependência do Windows estão instalados no `C:\Program Files\Microsoft Dependency Agent` por predefinição.  Se o agente de dependência não conseguir iniciar após a configuração estiver concluída, verifique os registos para obter informações de erro detalhadas. O diretório de registo é `%Programfiles%\Microsoft Dependency Agent\logs`. 

### <a name="install-the-dependency-agent-on-linux"></a>Instalar o agente de dependência no Linux
O agente de dependência é instalado em servidores Linux a partir de `InstallDependencyAgent-Linux64.bin`, um script de shell com um binário auto-extraível. Pode executar o ficheiro utilizando `sh` ou adicionar permissões para o próprio ficheiro de execução.

>[!NOTE]
> Para instalar ou configurar o agente é preciso acesso à raiz.
> 

| Parâmetro | Descrição |
|:--|:--|
| -Ajuda | Obter uma lista das opções da linha de comandos. |
| -s | Realizar uma instalação automática sem que seja solicitada a interação do utilizador. |
| -Verifique | Verificar as permissões e o sistema operativo, mas não instalar o agente. |

Por exemplo, para executar o programa de instalação com o `-help` parâmetro, o tipo `InstallDependencyAgent-Linux64.bin -help`.

Instalar o agente de dependência de Linux como raiz, executando o seguinte comando, `sh InstallDependencyAgent-Linux64.bin`
    
Se o agente de dependência não iniciar, verifique os registos para obter informações de erro detalhadas. Em agentes do Linux, o diretório de registo é `/var/opt/microsoft/dependency-agent/log`.

Ficheiros para o agente de dependência são colocados nos seguintes diretórios:

| Ficheiros | Localização |
|:--|:--|
| Ficheiros principais | /opt/microsoft/dependency-agent |
| Ficheiros de registo | /var/opt/microsoft/dependency-agent/log |
| Ficheiros de configuração | /etc/opt/microsoft/dependency-agent/config |
| Ficheiros executáveis de serviço | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Ficheiros de armazenamento binários | /var/opt/microsoft/dependency-agent/storage |

### <a name="enable-performance-counters"></a>Ativar os contadores de desempenho
Se a área de trabalho do Log Analytics referenciada pela solução não está configurada para recolher já os contadores de desempenho necessários para a solução, terão de ser ativada. Pode fazê-lo manualmente, conforme descrito [aqui](../../azure-monitor/platform/data-sources-performance-counters.md), ou por baixar e executar um script do PowerShell disponível a partir do [galeria do Powershell do Azure](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1).
 
### <a name="onboard-azure-monitor-for-vms"></a>Carregar Monitor do Azure para VMs
Esse método inclui um modelo JSON que especifica a configuração para permitir que os componentes da solução para a área de trabalho do Log Analytics.  

Se não estiver familiarizado com o conceito de implementar recursos com um modelo, consulte:
* [Implementar recursos com modelos do Resource Manager e o Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md)
* [Implementar recursos com modelos do Resource Manager e a CLI do Azure](../../azure-resource-manager/resource-group-template-deploy-cli.md) 

Se optar por utilizar a CLI do Azure, tem primeiro de instalar e utilizar a CLI localmente. Tem de executar a CLI do Azure versão 2.0.27 ou posterior. Para identificar a versão, execute `az --version`. Se precisar de instalar ou atualizar a CLI do Azure, veja [instalar a CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). 

#### <a name="create-and-execute-a-template"></a>Criar e executar um modelo

1. Copie e cole a seguinte sintaxe JSON no seu ficheiro:

    ```json
    {

    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "WorkspaceName": {
            "type": "string"
        },
        "WorkspaceLocation": {
            "type": "string"
        }
    },
    "resources": [
        {
            "apiVersion": "2017-03-15-preview",
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('WorkspaceName')]",
            "location": "[parameters('WorkspaceLocation')]",
            "resources": [
                {
                    "apiVersion": "2015-11-01-preview",
                    "location": "[parameters('WorkspaceLocation')]",
                    "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    },

                    "plan": {
                        "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                        "publisher": "Microsoft",
                        "product": "[Concat('OMSGallery/', 'ServiceMap')]",
                        "promotionCode": ""
                    }
                },
                {
                    "apiVersion": "2015-11-01-preview",
                    "location": "[parameters('WorkspaceLocation')]",
                    "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                    },
                    "plan": {
                        "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                        "publisher": "Microsoft",
                        "product": "[Concat('OMSGallery/', 'InfrastructureInsights')]",
                        "promotionCode": ""
                    }
                }
            ]
        }
    ]
    ```

2. Guarde este ficheiro como **installsolutionsforvminsights.json** para uma pasta local.
3. Edite os valores dos **WorkspaceName**, **ResourceGroupName**, e **WorkspaceLocation**.  O valor para **WorkspaceName** é o ID de recurso completo da sua área de trabalho do Log Analytics, que inclui o nome de área de trabalho e o valor de **WorkspaceLocation** é a região a área de trabalho está definida.
4. Está pronto para implementar este modelo com o seguinte comando do PowerShell:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    A alteração de configuração pode demorar alguns minutos a concluir. Quando for concluído, será apresentada uma mensagem que é semelhante ao seguinte e inclui o resultado:

    ```powershell
    provisioningState       : Succeeded
    ```
Depois de ativar a monitorização, poderá demorar cerca de 10 minutos antes de poder ver o estado de funcionamento e as métricas para o computador híbrida. 

## <a name="performance-counters-enabled"></a>Contadores de desempenho ativadas
Monitor do Azure para VMs configura uma área de trabalho do Log Analytics para recolher contadores de desempenho utilizadas pela solução.  A tabela seguinte lista os objetos e contadores configuradas pela solução que são recolhidas a cada 60 segundos.

### <a name="windows-performance-counters"></a>Contadores de desempenho do Windows

|Nome do objeto |Nome do contador |  
|------------|-------------|  
|Disco lógico |% De espaço livre |  
|Disco lógico |Média Disco seg/leitura |  
|Disco lógico |Média Disco seg/transferência |  
|Disco lógico |Média Disco seg/escritas |  
|Disco lógico |Bytes de disco/seg |  
|Disco lógico |Bytes Lidos de Disco/seg |  
|Disco lógico |Leituras de disco/seg |  
|Disco lógico |As transferências de disco/seg |  
|Disco lógico |Bytes Escritos em Disco/seg |  
|Disco lógico |Escritas de disco/seg |  
|Disco lógico |Megabytes livres |  
|Memória |MBytes disponíveis |  
|Placa de rede |Bytes recebidos/seg |  
|Placa de rede |Bytes enviados/seg |  
|Processador |% Tempo do processador |  

### <a name="linux-performance-counters"></a>Contadores de desempenho do Linux

|Nome do objeto |Nome do contador |  
|------------|-------------|  
|Disco lógico |% De espaço utilizado |  
|Disco lógico |Bytes Lidos de Disco/seg |  
|Disco lógico |Leituras de disco/seg |  
|Disco lógico |As transferências de disco/seg |  
|Disco lógico |Bytes Escritos em Disco/seg |  
|Disco lógico |Escritas de disco/seg |  
|Disco lógico |Megabytes livres |  
|Disco lógico |Bytes de disco lógico/seg |  
|Memória |MBytes de memória disponíveis |  
|Rede |Total de Bytes recebidos |  
|Rede |Total de Bytes transmitidos |  
|Processador |% Tempo do processador |  

## <a name="diagnostic-and-usage-data"></a>Dados de utilização e diagnóstico
A Microsoft recolhe automaticamente dados de utilização e desempenho através da sua utilização do serviço do Azure Monitor. A Microsoft utiliza estes dados para fornecer e melhorar a qualidade, segurança e integridade do serviço. Para fornecer capacidades de resolução de problemas exatas e eficientes, os dados a partir da funcionalidade de mapa incluem informações sobre a configuração de software, como o sistema operativo e o versão, o endereço IP, o nome DNS e o nome da estação de trabalho. Microsoft não recolhe nomes, moradas ou outras informações de contacto.

Para obter mais informações sobre a recolha de dados e a utilização, consulte a [declaração de privacidade do Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]
## <a name="next-steps"></a>Passos Seguintes

Com a monitorização ativada para a máquina virtual, estas informações são disponíveis para análise com o Azure Monitor para as VMs.  Para saber como utilizar a funcionalidade de estado de funcionamento, veja [vista do Azure Monitor de estado de funcionamento de VMs](vminsights-health.md), ou para ver dependências de aplicações detetadas, consulte [vista de Azure Monitor para o mapa de VMs](vminsights-maps.md).  