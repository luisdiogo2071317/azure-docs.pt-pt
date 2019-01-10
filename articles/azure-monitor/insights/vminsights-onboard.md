---
title: Implementar o Azure Monitor para VMs (pré-visualização) | Documentos da Microsoft
description: Este artigo descreve como implementar e configurar o Azure Monitor para as VMs para que possa começar a entender sobre a execução da sua aplicação distribuída e problemas de estado de funcionamento do que tenham sido identificadas.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/07/2018
ms.author: magoedte
ms.openlocfilehash: 3c1caa2485437768781ada2c7271445ccd3c19e1
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2019
ms.locfileid: "54190334"
---
# <a name="deploy-azure-monitor-for-vms-preview"></a>Implementar o Azure Monitor para VMs (pré-visualização)
Este artigo descreve como configurar o Azure Monitor para as VMs. O serviço monitoriza o estado de funcionamento do sistema operativo de máquinas de virtuais do Azure (VMs) e os conjuntos de dimensionamento de máquinas virtuais e máquinas virtuais no seu ambiente. Esta monitorização inclui a deteção e o mapeamento de dependências de aplicações que podem ser hospedados nos mesmos. 

Ativar o Azure Monitor para VMs com um dos seguintes métodos:

* Ativar uma única máquina virtual do Azure, selecionando **Insights (pré-visualização)** diretamente a partir da VM.
* Ative a duas ou mais VMs do Azure utilizando a política do Azure. Através deste método, as dependências necessárias de VMs novas e existentes instaladas e configuradas corretamente. VMs não compatíveis são comunicadas, para que possa decidir se pretende ativá-los e como deseja resolvê-los.
* Ativar dois ou conjuntos de dimensionamento de VMs do Azure ou numa máquina virtual mais através de uma subscrição especificada ou o grupo de recursos com o PowerShell.

São fornecidas informações adicionais sobre cada método posteriormente neste artigo.

## <a name="prerequisites"></a>Pré-requisitos
Antes de começar, certifique-se de que compreende as informações nas secções seguintes.

### <a name="log-analytics"></a>Log Analytics

Uma área de trabalho do Log Analytics é atualmente suportada nas seguintes regiões:

- EUA Centro-Oeste
- EUA Leste
- Europa Ocidental
- Sudeste asiático<sup>1</sup>

<sup>1</sup> nesta região atualmente não suporta a funcionalidade de estado de funcionamento do Monitor do Azure para VMs.

>[!NOTE]
>Máquinas virtuais do Azure podem ser implementadas a partir de qualquer região e não fica limitadas para as regiões suportadas para a área de trabalho do Log Analytics.
>

Se não tiver uma área de trabalho, pode criar uma com um dos seguintes métodos:
* [A CLI do Azure](../../azure-monitor/learn/quick-create-workspace-cli.md)
* [PowerShell](../../azure-monitor/learn/quick-create-workspace-posh.md)
* [O portal do Azure](../../azure-monitor/learn/quick-create-workspace.md)
* [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md)

Se estiver a ativar a monitorização para uma única VM do Azure no portal do Azure, pode criar uma área de trabalho durante este processo.

Para ativar a solução para o cenário de à escala, comece por configurar o seguinte na sua área de trabalho do Log Analytics:

* Instale as soluções ServiceMap e InfrastructureInsights. Pode concluir esta instalação apenas através de um modelo do Azure Resource Manager fornecido neste artigo.
* Configure a área de trabalho do Log Analytics para recolher contadores de desempenho.

Para configurar a sua área de trabalho para o cenário de à escala, consulte [configurar a área de trabalho do Log Analytics para uma implementação à escala](#setup-log-analytics-workspace).

### <a name="supported-operating-systems"></a>Sistemas operativos suportados

A tabela seguinte lista os sistemas operativos Windows e Linux que são suportados com o Azure Monitor para as VMs. Uma lista completa, que fornece detalhes sobre a versão de SO Linux principal e secundária e versões de kernel suportadas será fornecida posteriormente nesta secção.

|Versão do SO |Desempenho |Maps |Estado de Funcionamento |
|-----------|------------|-----|-------|
|Windows Server 2016 1803 | X | X | X |
|Windows Server 2016 | X | X | X |
|Windows Server 2012 R2 | X | X | |
|Windows Server 2012 | X | X | |
|Windows Server 2008 R2 | X | X| |
|Red Hat Enterprise Linux (RHEL) 7, 6| X | X| X |
|Ubuntu 18.04, 16.04, 14.04 | X | X | X |
|CentOS Linux 7, 6 | X | X | X |
|SUSE Linux Enterprise Server (SLES) 12 | X | X | X |
|Oracle Linux 7 | X<sup>1</sup> | | X |
|Oracle Linux 6 | X | X | X |
|Debian 9.4, 8 | X<sup>1</sup> | | X |

<sup>1</sup> funcionalidade do desempenho do Azure Monitor para VMs está disponível apenas a partir do Azure Monitor. Não está disponível quando acessá-lo diretamente a partir do painel do lado esquerdo da VM do Azure.

>[!NOTE]
>As seguintes informações aplicam-se para oferecer suporte a do sistema operativo Linux:
> - Apenas as versões de kernel padrão e SMP Linux são suportadas.
> - Versões kernel não padrão, como extensão de endereço físico (PAE) e Xen, não são suportadas para qualquer distribuição de Linux. Por exemplo, um sistema com a cadeia de versão do *2.6.16.21-0.8-xen* não é suportada.
> - Kernels personalizados, incluindo recompilações de kernels padrão, não são suportados.
> - CentOSPlus kernel não é suportada.

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

### <a name="the-microsoft-dependency-agent"></a>O agente do Microsoft Dependency
O Azure Monitor para a funcionalidade de mapa de VMs obtém seus dados do agente do Microsoft Dependency. O agente de dependência depende do agente do Log Analytics para estabelecer ligação com o Log Analytics. Por conseguinte, o sistema tem de ter o agente do Log Analytics instalada e configurada com o agente de dependência.

Se ativar o Azure Monitor para as VMs para uma única VM do Azure ou utilizar o método de implementação à escala, terá de utilizar a extensão de agente de dependência de VM do Azure para instalar o agente como parte da experiência.

Num ambiente híbrido, pode transferir e instalar o agente de dependência de qualquer uma das duas formas: Manualmente, ou utilizando um método de implantação automatizado para máquinas virtuais que estão alojados fora do Azure.

A tabela seguinte descreve as origens ligadas que a funcionalidade de mapa suporta num ambiente híbrido.

| Origem ligada | Suportadas | Descrição |
|:--|:--|:--|
| Agentes do Windows | Sim | Para além da [agente do Log Analytics para Windows](../../azure-monitor/platform/log-analytics-agent.md), agentes do Windows exigem o agente do Microsoft Dependency. Para obter uma lista completa das versões de sistema operativo, consulte [sistemas operativos suportados](#supported-operating-systems). |
| Agentes do Linux | Sim | Para além da [agente do Log Analytics para Linux](../../azure-monitor/platform/log-analytics-agent.md), o agente do Microsoft Dependency necessitam de agentes do Linux. Para obter uma lista completa das versões de sistema operativo, consulte [sistemas operativos suportados](#supported-operating-systems). |
| Grupo de gestão do System Center Operations Manager | Não | |

O agente de dependência pode ser transferido a partir das seguintes localizações:

| Ficheiro | SO | Versão | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.7.4 | A111B92AB6CF28EB68B696C60FE51F980BFDFF78C36A900575E17083972989E0 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.7.4 | AB58F3DB8B1C3DEE7512690E5A65F1DFC41B43831543B5C040FCCE8390F2282C |

## <a name="role-based-access-control"></a>Controlo de acesso baseado em funções
Para ativar e acessar os recursos no Azure Monitor para as VMs, terá de ser atribuídas as seguintes funções de acesso:

- Para ativar a solução, tem de ter o *Contribuidor do Log Analytics* função.

- Para ver o desempenho, o estado de funcionamento e mapear dados, tem de ter o *leitor monitorização* função para a VM do Azure. A área de trabalho do Log Analytics tem de ser configurada para o Azure Monitor para as VMs.

Para obter mais informações sobre como controlar o acesso a uma área de trabalho do Log Analytics, consulte [gerir áreas de trabalho](../../azure-monitor/platform/manage-access.md).

## <a name="enable-monitoring-in-the-azure-portal"></a>Ativar a monitorização no portal do Azure
Para ativar a monitorização da sua VM do Azure no portal do Azure, efetue o seguinte:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

1. Selecione **máquinas virtuais**.

1. Na lista, selecione uma VM.

1. Na página de VM, na **monitorização** secção, selecione **Insights (pré-visualização)**.

1. Sobre o **Insights (pré-visualização)** página, selecione **Experimente agora o**.

    ![Ativar o Azure Monitor para as VMs para uma VM](./media/vminsights-onboard/enable-vminsights-vm-portal-01.png)
1. Sobre o **integração de informações do Azure Monitor** página, se tiver um existentes do Log Analytics área de trabalho na mesma subscrição, selecione-o na lista pendente.  
    A lista preselects a área de trabalho predefinida e a localização que a máquina virtual é implementada na subscrição. 

    >[!NOTE]
    >Se quiser criar uma nova área de trabalho do Log Analytics para armazenar os dados de monitorização da VM, siga as instruções em [criar uma área de trabalho do Log Analytics](../../azure-monitor/learn/quick-create-workspace.md) em uma das regiões suportadas listadas anteriormente.

Depois de ativar a monitorização, poderá demorar cerca de 10 minutos antes de poder visualizar as métricas de estado de funcionamento para a máquina virtual.

![Ativar o Azure Monitor para monitorização de processamento da implementação de VMs](./media/vminsights-onboard/onboard-vminsights-vm-portal-status.png)


## <a name="deploy-at-scale"></a>Implementar em escala
Nesta secção, vai implementar Azure Monitor para as VMs em escala, utilizando a política do Azure ou do Azure PowerShell.

Antes de implementar as suas VMs, configure previamente a sua área de trabalho do Log Analytics, fazendo o seguinte:

1. Se ainda não tiver uma área de trabalho, crie um que pode suportar o Azure Monitor para as VMs.  
    Antes de continuar, consulte [gerir áreas de trabalho](../../log-analytics/log-analytics-manage-access.md?toc=/azure/azure-monitor/toc.json) para compreender as considerações de custo, gestão e conformidade.

1. Criar uma nova área de trabalho se não ainda existir que pode ser utilizada para suportar o Azure Monitor de VMs. Revisão [gerir áreas de trabalho](../../azure-monitor/platform/manage-access.md?toc=/azure/azure-monitor/toc.json) antes de criar uma nova área de trabalho para compreender as considerações de custo, gestão e conformidade antes de continuar.

1. Ative os contadores de desempenho na área de trabalho para a coleção em Linux e VMs do Windows.

1. Instalar e ativar a solução ServiceMap e InfrastructureInsights na sua área de trabalho.

### <a name="set-up-a-log-analytics-workspace"></a>Configurar uma área de trabalho do Log Analytics
Se não tiver uma área de trabalho do Log Analytics, criar uma ao rever os métodos que são sugeridos na ["Pré-requisitos"](#log-analytics) secção.

#### <a name="enable-performance-counters"></a>Ativar os contadores de desempenho
Se a área de trabalho do Log Analytics que é referenciada pela solução já não está configurada para recolher os contadores de desempenho necessários para a solução, terá de ativá-las. Pode fazê-lo em qualquer uma das seguintes formas:
* Manualmente, conforme descrito em [Windows e Linux origens de dados de desempenho do Log Analytics](../../azure-monitor/platform/data-sources-performance-counters.md)
* Ao transferir e executar um script do PowerShell que está disponível a partir [galeria do PowerShell do Azure](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1)

#### <a name="install-the-servicemap-and-infrastructureinsights-solutions"></a>Instalar as soluções ServiceMap e InfrastructureInsights
Esse método inclui um modelo JSON que especifica a configuração para permitir que os componentes da solução na sua área de trabalho do Log Analytics.

Se não estiver familiarizado com a implantação de recursos utilizando um modelo, consulte:
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
    }
    ```

1. Guarde este ficheiro como *installsolutionsforvminsights.json* para uma pasta local.

1. Edite os valores dos *WorkspaceName*, *ResourceGroupName*, e *WorkspaceLocation*. O valor para *WorkspaceName* é a ID de recurso completo da sua área de trabalho do Log Analytics, que inclui o nome de área de trabalho. O valor para *WorkspaceLocation* é a região a área de trabalho está definida.

1. Está pronto para implementar este modelo, utilizando o seguinte comando do PowerShell:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    A alteração de configuração pode demorar alguns minutos a concluir. Quando estiver concluída, é apresentada uma mensagem que é semelhante ao seguinte e inclui o resultado:

    ```powershell
    provisioningState       : Succeeded
    ```

### <a name="enable-by-using-azure-policy"></a>Ativar através da política do Azure
Para ativar o Azure Monitor para VMs em escala de forma que ajuda a garantir a conformidade consistente e a ativação automática das VMs recentemente aprovisionadas, recomendamos [do Azure Policy](../../azure-policy/azure-policy-introduction.md). Estas políticas:

* Implemente o agente Log Analytics e o agente de dependência.
* Relatório sobre os resultados de compatibilidade.
* Remediar para VMs incompatíveis.

Para ativar o Azure Monitor para VMs com a política do Azure no seu inquilino:

- Atribuir iniciativa a um âmbito: grupo de gestão, subscrição ou grupo de recursos
- Rever e corrigir os resultados de compatibilidade

Para obter mais informações sobre a atribuição de política do Azure, consulte [descrição geral da política do Azure](../../governance/policy/overview.md#policy-assignment) e reveja a [descrição geral de grupos de gestão](../../governance/management-groups/index.md) antes de continuar.

As definições de política estão listadas na tabela a seguir:

|Nome |Descrição |Tipo |
|-----|------------|-----|
|[Pré-visualização]: Ativar o Azure Monitor para VMs |Ative o Azure Monitor para as máquinas virtuais (VMs) no âmbito especificado (grupo de gestão, subscrição ou grupo de recursos). Aceita a área de trabalho do Log Analytics como um parâmetro. |Iniciativa |
|[Pré-visualização]: Implementação de agente de dependência de auditoria – imagem de VM (SO) não listados |Relatórios de VMs como não conforme se a imagem de VM (SO) não está definido na lista e o agente não está instalado. |Política |
|[Pré-visualização]: Implementação de agente da análise de registo de auditoria – imagem de VM (SO) não listados |Relatórios de VMs como não conforme se a imagem de VM (SO) não está definido na lista e o agente não está instalado. |Política |
|[Pré-visualização]: Implementar o agente de dependência para VMs do Linux |Implemente o agente de dependência para VMs do Linux, se a imagem de VM (SO) é definido na lista e o agente não está instalado. |Política |
|[Pré-visualização]: Implementar o agente de dependência para VMs do Windows |Implemente o agente de dependência para as VMs do Windows se a imagem de VM (SO) é definido na lista e o agente não está instalado. |Política |
|[Pré-visualização]: Implementar o agente de análise de registo para VMs do Linux |Implemente agente do Log Analytics para VMs do Linux, se a imagem de VM (SO) é definido na lista e o agente não está instalado. |Política |
|[Pré-visualização]: Implementar o agente de análise de registo para VMs do Windows |Implemente o agente de análise de registo para Windows VMs se a imagem de VM (SO) é definido na lista e o agente não está instalado. |Política |

Política de autónomo (não incluída com a iniciativa) é descrita aqui:

|Nome |Descrição |Tipo |
|-----|------------|-----|
|[Pré-visualização]: Área de trabalho do auditoria Log Analytics para VM - erro de correspondência de relatório |Relatórios VMs como não conforme se eles não estão a iniciar a área de trabalho do Log Analytics especificada na atribuição de iniciativa de política /. |Política |

#### <a name="assign-the-azure-monitor-initiative"></a>Atribuir a iniciativa de Azure Monitor
Com esta versão inicial, pode criar a atribuição de política apenas no portal do Azure. Para compreender como concluir estes passos, veja [criar uma atribuição de política do portal do Azure](../../governance/policy/assign-policy-portal.md).

1. Para iniciar o serviço de política do Azure no portal do Azure, selecione **todos os serviços**e, em seguida, procure e selecione **política**.

1. No painel esquerdo da página política do Azure, selecione **atribuições**.  
    Uma atribuição é uma política que foi atribuída para ter lugar num âmbito específico.
    
1. Na parte superior a **política - atribuições** página, selecione **atribuir iniciativa**.

1. Sobre o **atribuir iniciativa** página, selecione a **âmbito** ao clicar no botão de reticências (...) e selecione uma subscrição ou grupo de gestão.  
    No nosso exemplo, um âmbito limita a atribuição de política para um agrupamento de máquinas virtuais para a imposição.
    
1. Na parte inferior a **âmbito** página, guarde as alterações ao selecionar **selecione**.

1. (Opcional) Para remover um ou mais recursos do âmbito, selecione **exclusões**.

1. Selecione o **definição de iniciativa** reticências (...) para apresentar a lista de definições disponíveis, selecione  **[pré-visualização] ativar o Azure Monitor para VMs**e, em seguida, selecione  **Selecione**.  
    O **nome da atribuição** caixa é preenchida automaticamente com o nome da iniciativa que selecionou, mas pode alterá-lo. Também pode adicionar uma descrição opcional. O **atribuído pelo** caixa é preenchida automaticamente com base em quem está conectado e este valor é opcional.
    
1. Na **área de trabalho do Log Analytics** pendente lista para a região suportada, selecione uma área de trabalho.

    >[!NOTE]
    >Se a área de trabalho está além do escopo da atribuição, conceder *Contribuidor do Log Analytics* permissões para o ID de Principal. a atribuição de política Se não fizer isso, poderá ver uma falha de implementação, tais como: `The client '343de0fe-e724-46b8-b1fb-97090f7054ed' with object id '343de0fe-e724-46b8-b1fb-97090f7054ed' does not have authorization to perform action 'microsoft.operationalinsights/workspaces/read' over scope ... ` Para conceder acesso, reveja [como configurar manualmente a identidade gerida](../../governance/policy/how-to/remediate-resources.md#manually-configure-the-managed-identity).
    >  
    O **identidade gerido** caixa de verificação está selecionada, porque a iniciativa que está sendo atribuída inclui uma política com o *deployIfNotExists* efeito.
    
1. Na **localização de gerir a identidade** pendente, selecione a região adequada.

1. Selecione **Atribuir**.

#### <a name="review-and-remediate-the-compliance-results"></a>Rever e corrigir os resultados de compatibilidade

Pode saber como rever os resultados de compatibilidade, lendo [identificar os resultados de não conformidade](../../governance/policy/assign-policy-portal.md#identify-non-compliant-resources). No painel esquerdo, selecione **conformidade**e, em seguida, localize a  **[pré-visualização] ativar o Azure Monitor para VMs** para as VMs que não estão em conformidade, de acordo com a atribuição de iniciativa que criou.

![Conformidade com a política para VMs do Azure](./media/vminsights-onboard/policy-view-compliance-01.png)

As VMs com base nos resultados das políticas incluídos com a iniciativa, são comunicadas como não conforme nos seguintes cenários:

* O log Analytics ou o agente de dependência não está implementado.  
    Este cenário é comum para um âmbito com VMs existentes. Para o resolver, implantar os agentes necessários pelo [criar tarefas de atualização](../../governance/policy/how-to/remediate-resources.md) numa política de conformidade.  
    - [Pré-visualização]: Deploy Dependency Agent for Linux VMs
    - [Pré-visualização]: Deploy Dependency Agent for Windows VMs
    - [Pré-visualização]: Deploy Log Analytics Agent for Linux VMs
    - [Pré-visualização]: Deploy Log Analytics Agent for Windows VMs

* Imagem de VM (SO) não está identificado na definição de política.  
    Os critérios da política de implementação incluem apenas VMs que são implementadas a partir de imagens de VM do Azure bem conhecidas. Verifique a documentação para ver se o SO da VM é suportado. Se não é suportada, duplicar a política de implementação e a atualização ou modificá-lo para criar a imagem em conformidade.  
    - [Pré-visualização]: Implementação de agente de dependência de auditoria – imagem de VM (SO) não listados
    - [Pré-visualização]: Implementação de agente da análise de registo de auditoria – imagem de VM (SO) não listados

* VMs não são de iniciar sessão para a área de trabalho do Log Analytics especificada.  
    É possível que algumas VMs no âmbito da iniciativa estão a iniciar sessão a uma área de trabalho do Log Analytics diferente daquele que é especificada na atribuição de política. Esta política é uma ferramenta para identificar quais as VMs que comunicam com uma área de trabalho de não conformidade.  
    - [Pré-visualização]: Audit Log Analytics Workspace for VM - Report Mismatch

### <a name="enable-with-powershell"></a>Ativar com o PowerShell
Para ativar o Azure Monitor para as VMs para várias VMs ou conjuntos de dimensionamento de máquina virtual, pode utilizar o script do PowerShell [Install-VMInsights.ps1](https://www.powershellgallery.com/packages/Install-VMInsights/1.0), disponível a partir da galeria do PowerShell do Azure. Este script itera em cada virtual máquina e a máquina virtual conjunto de dimensionamento na sua subscrição, no grupo de recursos de âmbito especificado pelo *ResourceGroup*, ou para um conjunto de dimensionamento VM ou numa máquina virtual único que é especificado por *Nome*. Para cada conjunto de dimensionamento VM ou numa máquina virtual, o script verifica se a extensão da VM já está instalada. Se a extensão de VM não estiver instalada, o script tenta reinstalá-lo. Se a extensão da VM estiver instalada, o script instala as extensões VM de agente do Log Analytics e de dependência.

Este script requer o Azure PowerShell 5.7.0 de versão do módulo ou posterior. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Se estiver executando o PowerShell localmente, terá também de executar `Connect-AzureRmAccount` para criar uma ligação com o Azure.

Para obter uma lista de detalhes do argumento e a utilização de exemplo do script, execute `Get-Help`.

```powershell
Get-Help .\Install-VMInsights.ps1 -Detailed

SYNOPSIS
    This script installs VM extensions for Log Analytics and the Dependency agent as needed for VM Insights.


SYNTAX
    .\Install-VMInsights.ps1 [-WorkspaceId] <String> [-WorkspaceKey] <String> [-SubscriptionId] <String> [[-ResourceGroup]
    <String>] [[-Name] <String>] [[-PolicyAssignmentName] <String>] [-ReInstall] [-TriggerVmssManualVMUpdate] [-Approve] [-WorkspaceRegion] <String>
    [-WhatIf] [-Confirm] [<CommonParameters>]


DESCRIPTION
    This script installs or re-configures following on VMs and VM Scale Sets:
    - Log Analytics VM Extension configured to supplied Log Analytics Workspace
    - Dependency Agent VM Extension

    Can be applied to:
    - Subscription
    - Resource Group in a Subscription
    - Specific VM/VM Scale Set
    - Compliance results of a policy for a VM or VM Extension

    Script will show you list of VMs/VM Scale Sets that will apply to and let you confirm to continue.
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
        If using PolicyAssignmentName parameter, subscription that VMs are in

    -ResourceGroup <String>
        <Optional> Resource Group to which the VMs or VM Scale Sets belong

    -Name <String>
        <Optional> To install to a single VM/VM Scale Set

    -PolicyAssignmentName <String>
        <Optional> Take the input VMs to operate on as the Compliance results from this Assignment
        If specified will only take from this source.

    -ReInstall [<SwitchParameter>]
        <Optional> If VM/VM Scale Set is already configured for a different workspace, set this to change to the new workspace

    -TriggerVmssManualVMUpdate [<SwitchParameter>]
        <Optional> Set this flag to trigger update of VM instances in a scale set whose upgrade policy is set to Manual

    -Approve [<SwitchParameter>]
        <Optional> Gives the approval for the installation to start with no confirmation prompt for the listed VMs/VM Scale Sets

    -WorkspaceRegion <String>
        Region the Log Analytics Workspace is in
        Supported values: "East US","eastus","Southeast Asia","southeastasia","West Central US","westcentralus","West Europe","westeurope"
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

    Install for all VMs in a Resource Group in a subscription

    -------------------------- EXAMPLE 2 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup> -ReInstall

    Specify to reinstall extensions even if already installed, for example to update to a different workspace

    -------------------------- EXAMPLE 3 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -PolicyAssignmentName a4f79f8ce891455198c08736 -ReInstall

    Specify to use a PolicyAssignmentName for source, and to reinstall (move to a new workspace)
```

O exemplo seguinte demonstra como utilizar os comandos do PowerShell na pasta para ativar o Azure Monitor para VMs e compreender a saída esperada:

```powershell
$WorkspaceId = "<GUID>"
$WorkspaceKey = "<Key>"
$SubscriptionId = "<GUID>"
.\Install-VMInsights.ps1 -WorkspaceId $WorkspaceId -WorkspaceKey $WorkspaceKey -SubscriptionId $SubscriptionId -WorkspaceRegion eastus

Getting list of VMs or VM ScaleSets matching criteria specified

VMs or VM ScaleSets matching criteria:

db-ws-1 VM running
db-ws2012 VM running

This operation will install the Log Analytics and Dependency agent extensions on above 2 VMs or VM Scale Sets.
VMs in a non-running state will be skipped.
Extension will not be reinstalled if already installed. Use -ReInstall if desired, for example to update workspace

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

## <a name="enable-for-a-hybrid-environment"></a>Ativar para um ambiente híbrido
Esta secção explica como implementar máquinas virtuais ou computadores físicos que estão alojados no seu datacenter ou em outros ambientes de cloud para monitorização pelo Azure Monitor para as VMs.

O Azure Monitor para agente de dependência de mapa de VMs não transmitir todos os dados em si, e ele não requer alterações às firewalls ou portas. Os dados de mapa sempre são transmitidos pelo agente do Log Analytics para o serviço Azure Monitor, diretamente ou através da [Gateway de OMS](../../azure-monitor/platform/gateway.md) se as políticas de segurança de TI não permitir que os computadores na rede para ligar à internet.

Reveja os requisitos e métodos de implementação para o [agente Linux do Log Analytics e o Windows](../../log-analytics/log-analytics-agent-overview.md).

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

Os passos são resumidos da seguinte forma:

1. Instale o agente do Log Analytics para Windows ou Linux.

1. Transfira e instale o Azure Monitor para agente de dependência de mapa de VMs para [Windows](https://aka.ms/dependencyagentwindows) ou [Linux](https://aka.ms/dependencyagentlinux).

1. Ative a recolha de contadores de desempenho.

1. Implemente o Azure Monitor para as VMs.

### <a name="install-the-dependency-agent-on-windows"></a>Instale o agente de dependência no Windows
Pode instalar o agente de dependência manualmente em computadores Windows executando `InstallDependencyAgent-Windows.exe`. Se executar esse arquivo executável sem quaisquer opções, inicia um Assistente de configuração que pode seguir para instalar o agente de forma interativa.

>[!NOTE]
>*Administrador* privilégios são necessários para instalar ou desinstalar o agente.

A tabela seguinte realça os parâmetros que são suportados pelo programa de configuração para o agente a partir da linha de comandos.

| Parâmetro | Descrição |
|:--|:--|
| /? | Devolve uma lista das opções da linha de comandos. |
| /S | Executa uma instalação silenciosa, sem interação do utilizador. |

Por exemplo, para executar o programa de instalação com o `/?` parâmetro, escreva **InstallDependencyAgent Windows.exe /?**.

Ficheiros para o agente de dependência do Windows estão instalados no *agente de dependência do C:\Program Files\Microsoft* por predefinição. Se o agente de dependência não conseguir iniciar após a configuração estiver concluída, verifique os registos para obter informações de erro detalhadas. É o diretório de registo *%Programfiles%\Microsoft dependência Agent\logs*.

### <a name="install-the-dependency-agent-on-linux"></a>Instalar o agente de dependência no Linux
O agente de dependência é instalado em servidores Linux a partir de *InstallDependencyAgent Linux64.bin*, um script de shell com um binário auto-extraível. Pode executar o ficheiro utilizando `sh` ou adicionar permissões para o próprio ficheiro de execução.

>[!NOTE]
> Para instalar ou configurar o agente é preciso acesso à raiz.
>

| Parâmetro | Descrição |
|:--|:--|
| -Ajuda | Obter uma lista das opções da linha de comandos. |
| -s | Realizar uma instalação automática sem que seja solicitada a interação do utilizador. |
| -Verifique | Verifique as permissões e o sistema operativo, mas não instale o agente. |

Por exemplo, para executar o programa de instalação com o `-help` parâmetro, escreva **InstallDependencyAgent Linux64.bin-ajudar**.

Instalar o agente de dependência de Linux como raiz, executando o seguinte comando, `sh InstallDependencyAgent-Linux64.bin`.

Se o agente de dependência não iniciar, verifique os registos para obter informações de erro detalhadas. Em agentes do Linux, é o diretório de registo */var/opt/microsoft/dependency-agent/log*.

Ficheiros para o agente de dependência são colocados nos seguintes diretórios:

| Ficheiros | Localização |
|:--|:--|
| Ficheiros principais | /opt/microsoft/dependency-agent |
| Ficheiros de registo | /var/opt/microsoft/dependency-agent/log |
| Ficheiros de configuração | /etc/opt/microsoft/dependency-agent/config |
| Ficheiros executáveis de serviço | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Ficheiros de armazenamento binários | /var/opt/microsoft/dependency-agent/storage |

### <a name="enable-performance-counters"></a>Ativar os contadores de desempenho
Se a área de trabalho do Log Analytics que é referenciada pela solução já não está configurada para recolher os contadores de desempenho necessários para a solução, terá de ativá-las. Pode fazê-lo em qualquer uma das seguintes formas:
* Manualmente, conforme descrito em [Windows e Linux origens de dados de desempenho do Log Analytics](../../azure-monitor/platform/data-sources-performance-counters.md)
* Ao transferir e executar um script do PowerShell que está disponível a partir [galeria do PowerShell do Azure](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1)

### <a name="deploy-azure-monitor-for-vms"></a>Implementar o Azure Monitor para VMs
Esse método inclui um modelo JSON que especifica a configuração para permitir que os componentes da solução na sua área de trabalho do Log Analytics.

Se não estiver familiarizado com a implantação de recursos utilizando um modelo, consulte:
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
    }
    ```

1. Guarde este ficheiro como *installsolutionsforvminsights.json* para uma pasta local.

1. Edite os valores dos *WorkspaceName*, *ResourceGroupName*, e *WorkspaceLocation*. O valor para *WorkspaceName* é a ID de recurso completo da sua área de trabalho do Log Analytics, que inclui o nome de área de trabalho. O valor para *WorkspaceLocation* é a região a área de trabalho está definida.

1. Está pronto para implementar este modelo, utilizando o seguinte comando do PowerShell:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    A alteração de configuração pode demorar alguns minutos a concluir. Quando estiver concluída, é apresentada uma mensagem que é semelhante ao seguinte e inclui o resultado:

    ```powershell
    provisioningState       : Succeeded
    ```
Depois de ativar a monitorização, poderá demorar cerca de 10 minutos antes de poder ver o estado de funcionamento e métricas para o computador híbrida.

## <a name="performance-counters-enabled"></a>Contadores de desempenho ativadas
Monitor do Azure para VMs configura uma área de trabalho do Log Analytics para recolher os contadores de desempenho que são utilizados pela solução. A tabela seguinte lista os objetos e contadores configuradas pela solução que são recolhidas a cada 60 segundos.

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
A Microsoft recolhe automaticamente dados de utilização e desempenho através da sua utilização do serviço do Azure Monitor. A Microsoft utiliza estes dados para fornecer e melhorar a qualidade, segurança e integridade do serviço. Para fornecer capacidades de resolução de problemas exatas e eficientes, os dados a partir da funcionalidade de mapa incluem informações sobre a configuração do seu software, como o sistema operativo e o versão, o endereço IP, o nome DNS e o nome de estação de trabalho. Microsoft não recolhe nomes, moradas ou outras informações de contacto.

Para obter mais informações sobre a recolha de dados e a utilização, consulte a [declaração de privacidade do Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]
## <a name="next-steps"></a>Passos Seguintes

Agora que a monitorização estiver ativada para a máquina virtual, estas informações são disponíveis para análise com o Azure Monitor para as VMs. Para saber como utilizar a funcionalidade de estado de funcionamento, veja [vista do Azure Monitor de estado de funcionamento de VMs](vminsights-health.md). Para ver dependências de aplicações detetadas, consulte [vista do Azure Monitor para o mapa de VMs](vminsights-maps.md).
