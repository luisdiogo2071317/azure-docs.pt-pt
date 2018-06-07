---
title: Aplicação de orquestração do Service Fabric patch do Azure para linux | Microsoft Docs
description: Aplicação para automatizar a aplicação de patches de sistema operativo num cluster de Linux Service Fabric.
services: service-fabric
documentationcenter: .net
author: novino
manager: timlt
editor: ''
ms.assetid: de7dacf5-4038-434a-a265-5d0de80a9b1d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/22/2018
ms.author: nachandr
ms.openlocfilehash: ea999945ace53099eb9dec15397310c9b5d1b904
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34643129"
---
# <a name="patch-the-linux-operating-system-in-your-service-fabric-cluster"></a>Correção do sistema operativo Linux no seu cluster do Service Fabric

> [!div class="op_single_selector"]
> * [Windows](service-fabric-patch-orchestration-application.md)
> * [Linux](service-fabric-patch-orchestration-application-linux.md)
>
>

A aplicação de orquestração de patch é uma aplicação de Service Fabric do Azure que automatiza o sistema operativo num cluster de Service Fabric sem períodos de indisponibilidade a aplicação de patches.

A aplicação de orquestração de patch fornece as seguintes funcionalidades:

- **Instalação da atualização automática do sistema operativo**. Atualizações do sistema operativo são transferidas e instaladas automaticamente. Nós de cluster são reiniciados, conforme necessário, sem período de indisponibilidade do cluster.

- **Integração de aplicação de patches e estado de funcionamento com suporte para cluster**. Ao aplicar as atualizações, a aplicação de orquestração de patch monitoriza o estado de funcionamento de nós do cluster. Nós de cluster são um nó atualizado uma hora ou um domínio de atualização de cada vez. Se o estado de funcionamento do cluster de ficar inativo devido ao processo de aplicação de patches, a aplicação de patches está parada para impedir a aggravating o problema.

## <a name="internal-details-of-the-app"></a>Internos detalhes da aplicação

A aplicação de orquestração de patch é composta pelos subcomponentes seguintes:

- **Serviço de coordenador**: este serviço de monitorização de estado é responsável por:
    - Coordenar a tarefa de atualização de SO em todo o cluster.
    - Armazenar o resultado das operações de atualização do SO foi concluídas.
- **Serviço de agente de nó**: este serviço sem monitorização de estado é executado em todos os nós de cluster do Service Fabric. O serviço é responsável por:
    - Bootstrapping o daemon de agente de nó no Linux.
    - O serviço do daemon de monitorização.
- **O daemon de agente de nó**: Linux este serviço de daemon é executada num nível mais elevado privilégio (root). Em contrapartida, o serviço de agente de nó e o serviço Coordenador de executam um privilégio de nível inferior. O serviço é responsável por efetuar as seguintes tarefas de atualização em todos os nós do cluster:
    - Desativar a atualização automática do SO no nó.
    - Transferir e instalar a atualização do SO, de acordo com a política de utilizador forneceu.
    - Reiniciar o post máquina instalação da atualização de SO se for necessário.
    - A carregar os resultados das atualizações do SO para o serviço de coordenador.
    - Os relatórios de estado de relatórios no caso de uma operação falhou após esgotamento todas as tentativas.

> [!NOTE]
> A aplicação de orquestração de patch utiliza o serviço de sistema do Service Fabric reparação manager para desativar ou ativar o nó e executar verificações do Estado de funcionamento. A tarefa de reparação criada pela aplicação de orquestração patch controla o progresso da atualização para cada nó.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="ensure-that-your-azure-vms-are-running-ubuntu-1604"></a>Certifique-se de que as suas VMs do Azure estão em execução Ubuntu 16.04
No momento da escrita deste documento, Ubuntu 16.04 (`Xenial Xerus`) é a única versão suportada.

### <a name="ensure-that-the-service-fabric-linux-cluster-is-version-62x-and-above"></a>Certifique-se de que o cluster do service fabric linux versão 6.2.x e acima

Patch orchestration aplicação linux utiliza determinadas funcionalidades do tempo de execução que só estão disponíveis na versão de tempo de execução do service fabric 6.2.x e superior.

### <a name="enable-the-repair-manager-service-if-its-not-running-already"></a>Ativar o serviço do Gestor de reparação (se este não está em execução já)

A aplicação de orquestração de patch requer o serviço de sistema de manager reparação esteja ativada no cluster.

#### <a name="azure-clusters"></a>Clusters do Azure

Clusters do linux do Azure na prata e o escalão de durabilidade gold tem o serviço do Gestor de reparação ativado por predefinição. Clusters do Azure no escalão bronze durabilidade, por predefinição, é necessário o serviço do Gestor de reparação ativado. Se o serviço já está ativado, pode ver funcionar na secção de serviços de sistema no Service Fabric Explorer.

##### <a name="azure-portal"></a>Portal do Azure
Pode ativar o Gestor de reparação a partir do portal do Azure no momento da configuração de cluster. Selecione **incluem o Gestor de reparação** opção em **funcionalidades de suplemento** no momento da configuração de cluster.
![Imagem de ativar o Gestor de reparação a partir do portal do Azure](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)

##### <a name="azure-resource-manager-deployment-model"></a>Modelo de implementação do Azure Resource Manager
Em alternativa, pode utilizar o [modelo de implementação Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) para ativar o serviço do Gestor de reparação em clusters de Service Fabric novas e existentes. Obter o modelo para o cluster que pretende implementar. Pode utilizar os modelos de exemplo ou criar um modelo de modelo de implementação Azure Resource Manager personalizado. 

Para ativar o Gestor de reparação serviço utilizando [modelo de implementação Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm):

1. Primeiro, verifique se o `apiversion` está definido como `2017-07-01-preview` para o `Microsoft.ServiceFabric/clusters` recursos. Se for diferente, em seguida, tem de atualizar o `apiVersion` ao valor `2017-07-01-preview` ou superior:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Ativar agora o serviço do Gestor de reparação adicionando o seguinte `addonFeatures` secção após o `fabricSettings` secção:

    ```json
    "fabricSettings": [
        ...      
    ],
    "addonFeatures": [
        "RepairManager"
    ],
    ```

3. Depois de atualizar o modelo de cluster com estas alterações, aplicá-las e permita que a atualização concluída. Agora, pode ver o serviço de sistema reparação manager em execução no seu cluster. É denominado `fabric:/System/RepairManagerService` na secção de serviços de sistema no Service Fabric Explorer. 

### <a name="standalone-on-premises-clusters"></a>Autónomo no local clusters

Clusters do Linux de recursos de infraestrutura de serviço autónomo não são suportadas no momento da escrita deste documento.

### <a name="disable-automatic-os-update-on-all-nodes"></a>Desativar a atualização automática do SO em todos os nós

As atualizações automáticas do SO podem levar a perda de disponibilidade e ou alterar o comportamento das aplicações em execução. A aplicação de patch de orquestração, por predefinição, tenta desativar o atualização de SO automático em cada nó de cluster para impedir que tais cenários.
Para Ubuntu [autónoma atualizações](https://help.ubuntu.com/community/AutomaticSecurityUpdates) estão desativados por aplicações de orquestração do patch.

## <a name="download-the-app-package"></a>Transferir o pacote de aplicação

Aplicação juntamente com os scripts de instalação pode ser transferida do [hiperligação do arquivo](https://go.microsoft.com/fwlink/?linkid=867984).

Aplicação em formato sfpkg pode ser transferida do [sfpkg ligação](https://go.microsoft.com/fwlink/?linkid=867984&pc=sfpkg). Esta informação ser proveniente útil para [do Azure Resource Manager com implementação de aplicação base](service-fabric-application-arm-resource.md).

## <a name="configure-the-app"></a>Configurar a aplicação

O comportamento da aplicação de orquestração patch pode ser configurado para satisfazer as suas necessidades. Substitua os valores predefinidos mediante a transmissão no parâmetro de aplicação durante a criação de aplicação ou atualização. Podem ser fornecidos parâmetros de aplicação, especificando `ApplicationParameter` para o `Start-ServiceFabricApplicationUpgrade` ou `New-ServiceFabricApplication` cmdlets.

|**Parâmetro**        |**Tipo**                          | **Detalhes**|
|:-|-|-|
|MaxResultsToCache    |Longo                              | Número máximo de resultados de atualização, que devem ser colocados em cache. <br>Valor predefinido é 3000 partindo do princípio de: <br> -Número de nós é 20. <br> -O número de atualizações a acontecer num nó por mês é cinco. <br> -Número de resultados por operação pode ter 10. <br> -Devem ser armazenados resultados durante os últimos três meses. |
|TaskApprovalPolicy   |Enum <br> {NodeWise, UpgradeDomainWise}                          |TaskApprovalPolicy indica a política que está a ser utilizado pelo serviço de coordenador para instalar atualizações em todos os nós de cluster do Service Fabric.<br>                         Valores permitidos são: <br>                                                           <b>NodeWise</b>. As atualizações são instalado um nó de cada vez. <br>                                                           <b>UpgradeDomainWise</b>. As atualizações são instalado um domínio de atualização de cada vez. (No máximo, podem aceder todos os nós que pertencem a um domínio de atualização para atualização.)
| UpdateOperationTimeOutInMinutes | Int <br>(Predefinição: 180)                   | Especifica o tempo limite em nenhuma operação de atualização (transferir ou instalar). Se não for possível concluir a operação dentro do tempo limite especificado, foi abortada.       |
| RescheduleCount      | Int <br> (Predefinição: 5).                  | O número máximo de vezes que o serviço reschedules SO atualizar no caso de uma operação falha de forma permanente.          |
| RescheduleTimeInMinutes  | Int <br>(Predefinição: 30). | O intervalo no qual o serviço reschedules SO atualizar no caso de falha persistir. |
| UpdateFrequency           | Cadeia separada por vírgulas (predefinição: "Semanal, quarta-feira, 7:00:00")     | A frequência para a instalação de atualizações do SO no cluster. Os valores de formato e possíveis são: <br>-Mensais, DD, hh: mm:, por exemplo, mensalmente, 5, 12:22:32. <br> -Semanal, dia, hh: mm:, de exemplo, semanalmente, Terça-feira, 12:22:32.  <br> -Diárias, hh: mm:, por exemplo, diariamente, 12:22:32.  <br> -None indica que a atualização não deve ser efetuada.  <br><br> Todas as horas são em UTC.|
| UpdateClassification | Cadeia separada por vírgulas (predefinição: "securityupdates") | Tipo de atualizações que devem ser instalados em nós do cluster. Os valores aceitáveis são securityupdates, todos os. <br> -securityupdates - deverá instalar apenas as atualizações de segurança <br> -todos os - instaladas todas as atualizações disponíveis do apt.|
| ApprovedPatches | Cadeia separada por vírgulas (predefinição: "") | Esta é a lista de atualizações aprovadas que devem ser instalados em nós de cluster. A lista de valores separados por vírgulas contém pacotes aprovados e a versão de destino, opcionalmente, pretendido.<br> Por exemplo: "apt utils = 1.2.10ubuntu1, python3 jwt, https apt transporte < 1.2.194, libsystemd0 > = 229 4ubuntu16" <br> O procedimento acima instaladas <br> -apt utils 1.2.10ubuntu1 de versão, se estiver disponível na apt cache. Se essa versão específico não estiver disponível, em seguida, fica sem operações. <br> -atualizações python3 jwt para a versão mais recente disponível. Se o pacote não estiver presente, em seguida, fica sem operações. <br> -apt transporte https atualizações para a versão mais recente que é inferior a 1.2.194. Se esta versão não estiver presente, em seguida, fica sem operações. <br> -libsystemd0 atualizações para a versão mais recente que seja maior ou igual a 229 4ubuntu16. Se esses uma versão não existir, em seguida, fica sem operações.|
| RejectedPatches | Cadeia separada por vírgulas (predefinição: "") | Esta é a lista de atualizações que não deve ser instalado em nós de cluster <br> Por exemplo: "bash, sudo" <br> Precedente filtra bash, sudo de receber quaisquer atualizações. |


> [!TIP]
> Se pretender que a atualização de SO para ocorrer imediatamente, defina `UpdateFrequency` relativamente ao momento da implementação de aplicação. Por exemplo, suponha que tem um cluster de teste de cinco nós e planeie a implementar a aplicação em aproximadamente 5:00 PM UTC. Se parte do princípio de que a atualização da aplicação ou implementação demora 30 minutos no máximo, defina o UpdateFrequency como "Diariamente, 17:30:00."

## <a name="deploy-the-app"></a>Implementar a aplicação

1. Prepare o cluster por concluir todos os passos dos pré-requisitos.
2. Implemente a aplicação de orquestração de patch como qualquer outra aplicação de Service Fabric. Pode implementar a aplicação utilizando o PowerShell ou a CLI do Azure Service Fabric. Siga os passos no [implementar e remover aplicações utilizando o PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications) ou [implementar a aplicação utilizar a CLI do Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/scripts/cli-deploy-application)
3. Para configurar a aplicação no momento da implementação, passar o `ApplicationParamater` para o `New-ServiceFabricApplication` os scripts ou o cmdlet fornecidos. Para sua comodidade, powershell (Deploy.ps1) e os scripts de bash (Deploy.sh) são fornecidas juntamente com a aplicação. Para utilizar o script:

    - Ligar a um cluster do Service Fabric.
    - Execute o script de implementar. Opcionalmente, transmita o parâmetro de aplicação para o script. Por exemplo:.\Deploy.ps1 - ApplicationParameter @{UpdateFrequency = "Diariamente, 11:00:00"}./Deploy.sh ou "{\"UpdateFrequency\":\"diariamente, 11:00:00\"}" 

> [!NOTE]
> Mantenha o script e a pasta de aplicação PatchOrchestrationApplication no mesmo diretório.

## <a name="upgrade-the-app"></a>Atualizar a aplicação

Para atualizar uma aplicação de orquestração de patch existente, siga os passos no [atualização da aplicação de Service Fabric com o PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell) ou [atualização da aplicação de Service Fabric com a CLI do Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-upgrade)

## <a name="remove-the-app"></a>Remova a aplicação

Para remover a aplicação, siga os passos no [implementar e remover aplicações utilizando o PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications) ou [remover uma aplicação utilizar a CLI do Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-delete)

Para sua comodidade, powershell (Undeploy.ps1) e os scripts de bash (Undeploy.sh) são fornecidas juntamente com a aplicação. Para utilizar o script:

  - Ligar a um cluster do Service Fabric.
  - Executar o script Undeploy.ps1 ou Undeploy.sh

> [!NOTE]
> Mantenha o script e a pasta de aplicação PatchOrchestrationApplication no mesmo diretório.

## <a name="view-the-update-results"></a>Ver os resultados de atualização

A aplicação de orquestração de patch expõe as APIs REST para apresentar os resultados históricos ao utilizador. Segue-se um resultado de exemplo: ```testadm@bronze000001:~$ curl -X GET http://10.0.0.5:20002/PatchOrchestrationApplication/v1/GetResults```
```json
[ 
  { 
    "NodeName": "_bronze_0", 
    "UpdateOperationResults": [ 
      { 
        "OperationResult": "succeeded", 
        "NodeName": "_bronze_0", 
        "OperationTime": "2017-11-21T12:39:29.0435917Z", 
        "UpdateDetails": [ 
          { 
            "UpdateId": "linux-cloud-tools-azure:amd64=4.11.0.1015.15", 
            "ResultCode": "succeeded" 
          }, 
          { 
            "UpdateId": "linux-headers-azure:amd64=4.11.0.1015.15", 
            "ResultCode": "succeeded" 
          }, 
          { 
            "UpdateId": "linux-image-azure:amd64=4.11.0.1015.15", 
            "ResultCode": "succeeded" 
          }, 
          { 
            "UpdateId": "linux-tools-azure:amd64=4.11.0.1015.15", 
            "ResultCode": "succeeded" 
          }, 
          { 
            "UpdateId": "python3-apport:amd64=2.20.1-0ubuntu2.13", 
            "ResultCode": "succeeded" 
          }, 
        ], 
        "OperationType": "installation", 
        "UpdateClassification": "securityupdates", 
        "UpdateFrequency": "Daily, 7:00:00", 
        "RebootRequired": true, 
        "ApprovedList": "", 
        "RejectedList": "" 
      } 
    ] 
  } 
] 
```

Os campos de JSON são descritos da seguinte forma:

Campo | Valores | Detalhes
-- | -- | --
OperationResult | 0 - foi concluída com êxito<br> 1 - foi concluída com êxito com erros<br> 2 - falha<br> 3 - abortada<br> 4 - abortado com tempo limite | Indica o resultado da operação global (geralmente que envolve a instalação de atualizações de um ou mais).
resultCode | Mesmo que OperationResult | Este campo indica o resultado da operação de instalação para a atualização individual.
OperationType | 1 - instalação<br> 0 - procurar e transferir.| A instalação é a única OperationType que deverá ser apresentada nos resultados por predefinição.
UpdateClassification | Predefinição é "securityupdates" | Tipo de atualizações que é instalado durante a operação de atualização
UpdateFrequency | Predefinição é "Semanal, quarta-feira, 7:00:00" | Atualização de frequência configurada para esta atualização.
RebootRequired | VERDADEIRO - foi necessário reiniciar o computador<br> FALSO - não foi necessário reiniciar o computador | Indica que uma reinicialização não foi necessária para concluir a instalação das atualizações.
ApprovedList | Predefinição é "" | Lista de patches aprovados para esta atualização
RejectedList | Predefinição é "" | Lista de patches rejeitados para esta atualização

Se não existe nenhuma atualização está agendada ainda, o resultado JSON está vazio.

Inicie sessão no cluster para resultados de atualização de consulta. Em seguida, determinar o endereço de réplica para o principal do serviço de coordenador e atingiu o URL do browser: http://&lt;IP de RÉPLICA&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1/GetResults .

O ponto final REST para o serviço de coordenador possui uma porta dinâmica. Para verificar o URL exato, consulte o Service Fabric Explorer. Por exemplo, os resultados estão disponíveis em `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetResults`.

![Imagem do ponto final REST](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)

## <a name="diagnosticshealth-events"></a>Eventos de diagnóstico/estado de funcionamento

### <a name="diagnostic-logs"></a>Registos de diagnósticos

Os registos de aplicação de orquestração de patch são recolhidos como parte dos registos de tempo de execução do Service Fabric.

No caso de pretender capturar registos através de diagnóstico ferramenta por pipeline à sua escolha. Aplicação de orquestração de patch utilizará os seguintes IDs de fornecedor fixo para registar eventos através de [eventsource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracing.eventsource?view=netstandard-2.0)

- e39b723c-590c-4090-abb0-11e3e6616346
- fc0028ff-bfdc-499f-80dc-ed922c52c5e9
- 24afa313-0d3b-4c7c-b485-1047fd964b60
- 05dc046c-60e9-4ef7-965e-91660adffa68

### <a name="health-reports"></a>Relatórios de estado de funcionamento

A aplicação de orquestração de correção também publica relatórios de estado de funcionamento contra o serviço de coordenador ou o serviço de agente de nó nos seguintes casos:

#### <a name="an-update-operation-failed"></a>Uma operação de atualização falhou

Se uma operação de atualização falhar um nó, é gerado um relatório de estado de funcionamento contra o serviço de agente de nó. Detalhes do relatório de estado de funcionamento contém o nome de nó problemáticas.

Aplicação de patches esteja concluída com êxito no nó problemático, o relatório é automaticamente eliminado.

#### <a name="the-node-agent-daemon-service-is-down"></a>O serviço do Daemon de agente de nó está inativo

Se o serviço do Daemon de agente de nó para baixo num nó, é gerado um relatório de estado de funcionamento do nível de aviso contra o serviço de agente de nó.

#### <a name="the-repair-manager-service-is-not-enabled"></a>O serviço do Gestor de reparação não está ativado

É gerado um relatório de estado de funcionamento do nível de aviso para o serviço de coordenador se o serviço do Gestor de reparação não foi encontrado no cluster.

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

Q. **Por que vejo o meu cluster num Estado de erro quando a aplicação de orquestração de correção está em execução?**

A. Durante o processo de instalação, a aplicação de orquestração de patch desativa ou reinicia nós. Esta operação pode resultar temporariamente no estado de funcionamento do cluster ficar.

Com base na política para a aplicação, o ou um nó pode aceder durante uma operação de aplicação de patches *ou* um domínio de atualização completo pode aceder em simultâneo para baixo.

No final da instalação, os nós são reenabled após o reinício.

No exemplo seguinte, o cluster correu para um Estado de erro temporariamente porque foram dois nós para baixo e a política de MaxPercentageUnhealthyNodes foi violada. O erro é temporário até que a aplicação de patches operação está em curso.

![Imagem do cluster mau estado de funcionamento](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

Se o problema persistir, consulte a secção de resolução de problemas.

Q. **Aplicação de orquestração de patch se encontra num Estado de aviso**

A. Certifique-se de um relatório de estado de funcionamento publicado contra a aplicação estiver a causa raiz. Normalmente, o aviso contém detalhes do problema. Se o problema é transitório, a aplicação deverá auto-recuperar deste Estado.

Q. **O que posso fazer se a minha cluster está danificada e necessário efetuar uma atualização do sistema de operativo urgente?**

A. A aplicação de orquestração de patch não instala atualizações enquanto o cluster está danificado. Para desbloquear o fluxo de trabalho de aplicação de orquestração patch, traga o seu cluster para um bom estado de funcionamento.

Q. **Por que motivo aplicação de patches em clusters de tomar tempo para executar?**

A. O tempo necessário para a aplicação de orquestração de patch principalmente está dependente os seguintes fatores:

- A política do serviço de coordenador. 
  - A política predefinida, `NodeWise`, resulta numa aplicação de patches apenas um nó de cada vez. Especialmente se houver um cluster maior, recomendamos que utilize o `UpgradeDomainWise` política para alcançar a aplicação de patches mais rápida do cluster.
- O número de atualizações disponíveis para transferência e instalação. 
- O tempo médio necessário para transferir e instalar uma atualização, que não deve ter mais duas horas.
- O desempenho da VM e rede de largura de banda.

Q. **Como does patch orchestration aplicação decide as atualizações são atualizações de segurança.**

A. Aplicação de orquestração de patch utiliza lógica específica distro para determinar que atualizações entre as atualizações disponíveis são as atualizações de segurança. Por exemplo: no ubuntu a aplicação procura de atualizações a partir de arquivos $RELEASE-segurança, $RELEASE-atualizações ($RELEASE = xenial ou a versão de lançamento base padrão de linux). 

 
Q. **Como bloquear para uma versão específica do pacote**

A. Utilize as definições de ApprovedPatches bloquear os pacotes para uma versão específica. 


Q. **O que acontece para as atualizações automáticas ativadas no Ubuntu?**

A. Assim que instalar o patch orchestration aplicação no seu cluster, seriam desativadas autónoma atualizações no nó de cluster. Todos os o fluxo de trabalho de atualização periódica seria conduzido pela aplicação de orquestração do patch.
Para consistência do ambiente em cluster, recomendamos que instale as atualizações através de aplicações de orquestração de patch apenas. 
 
Q. **Post atualização aplicar o patch efetue de aplicação de orquestração a limpeza de pacotes não utilizados?**

A. Sim, a limpeza ocorre como parte dos passos de pós-instalação. 

Q. **Aplicação de Patch Orchestration pode ser utilizada para aplicar o patch meu cluster de desenvolvimento (um nó de cluster)?**

A. Não, Patch orchestration aplicação não pode ser utilizado para o cluster de um nó de patch. Esta limitação é por predefinição, como [serviços do sistema de recursos de infraestrutura de serviço](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-technical-overview#system-services) ou todas as aplicações cliente irão sentir alguns períodos de indisponibilidade e, por conseguinte, qualquer tarefa de reparação para aplicação de patches nunca iria obter aprovada pelo Gestor de reparação.

## <a name="troubleshooting"></a>Resolução de problemas

### <a name="a-node-is-not-coming-back-to-up-state"></a>Um nó é não regressam ao estado de segurança

**O nó pode estar bloqueado no estado de Desativação porque**:

Uma verificação de segurança está pendente. Para resolver esta situação, certifique-se de que estão disponíveis suficientes nós em bom estado.

**O nó pode estar bloqueado no estado desativado porque**:

- O nó foi desativado manualmente.
- O nó foi desativado devido a uma tarefa de infraestrutura do Azure em curso.
- O nó foi desativado temporariamente pela aplicação de orquestração do patch para corrigir o nó.

**O nó pode estar bloqueado no estado de indisponibilidade porque**:

- O nó foi colocado em estado de indisponibilidade manualmente.
- O nó está sob um reinício (o que poderá ser acionado pela aplicação de orquestração do patch).
- O nó não está disponível devido a uma VM defeituosa ou problemas de conectividade de rede ou de máquina.

### <a name="updates-were-skipped-on-some-nodes"></a>As atualizações foram ignoradas em alguns nós

A aplicação de orquestração de patch tenta instalar uma atualização, de acordo com a política rescheduling. O serviço tenta recuperar o nó e ignorar a atualização de acordo com as políticas de aplicação.

Nesse caso, é gerado um relatório de estado de funcionamento do nível de aviso contra o serviço de agente de nó. O resultado para a atualização contém também o motivo possível da falha.

### <a name="the-health-of-the-cluster-goes-to-error-while-the-update-installs"></a>O estado de funcionamento do cluster entra em erro durante a instalação da atualização

Pode colocar uma atualização defeituosa para baixo do Estado de funcionamento de uma aplicação ou um cluster num determinado nó ou domínio de atualização. A aplicação de orquestração de patch discontinues quaisquer operações de atualização subsequentes até que o cluster está em bom estado de novo.

Um administrador tem intervene e determinar o motivo pelo qual a aplicação ou o cluster ter ficado danificado devido a uma atualização anteriormente instalada.

## <a name="disclaimer"></a>Exclusão de Responsabilidade

A aplicação de orquestração de patch recolhe telemetria para controlar a utilização e desempenho. A telemetria da aplicação segue a definição da definição de telemetria o runtime Service Fabric (que está ativada por predefinição).

## <a name="release-notes"></a>Notas de Versão

### <a name="version-010"></a>Versão 0.1.0
- Versão de pré-visualização privada

### <a name="version-200"></a>Versão 2.0.0
- Versão pública

### <a name="version-201-latest"></a>Versão 2.0.1 (mais recentes)
- Recompilar a aplicação utilizando o SDK mais recente de Service Fabric
