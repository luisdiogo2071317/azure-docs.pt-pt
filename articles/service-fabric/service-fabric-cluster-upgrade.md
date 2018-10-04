---
title: Atualizar um cluster do Azure Service Fabric | Documentos da Microsoft
description: Atualize o código do Service Fabric e/ou a configuração que executa um cluster do Service Fabric, incluindo a definição modo de atualização do cluster, atualizar certificados, adicionando as portas da aplicação, fazendo patches de SO, e assim por diante. O que esperar quando as atualizações são efetuadas?
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: 15190ace-31ed-491f-a54b-b5ff61e718db
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 8/10/2017
ms.author: aljo
ms.openlocfilehash: 2fd62f8709bddfd981f4b1358c97d0acbaf7f12d
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48269109"
---
# <a name="upgrade-an-azure-service-fabric-cluster"></a>Atualizar um cluster do Azure Service Fabric
> [!div class="op_single_selector"]
> * [Cluster do Azure](service-fabric-cluster-upgrade.md)
> * [Cluster autónomo](service-fabric-cluster-upgrade-windows-server.md)
> 
> 

Para qualquer sistema moderno, a criação de uma upgradability é a chave para alcançar o sucesso a longo prazo de seu produto. Um cluster do Azure Service Fabric é um recurso que o proprietário, mas está parcialmente gerida pela Microsoft. Este artigo descreve o que é gerenciado automaticamente e o que pode configurar por conta própria.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Controlar a versão de recursos de infraestrutura que é executado no seu Cluster
Pode definir o seu cluster para receber atualizações de recursos de infraestrutura automático à medida que são lançadas pela Microsoft ou pode selecionar uma versão de recursos de infraestrutura suportadas que pretende que o cluster na.

Para tal, definir a configuração do cluster "upgradeMode" no portal ou com o Resource Manager no momento da criação ou mais tarde um cluster em direto 

> [!NOTE]
> Certifique-se manter o seu cluster sempre a executar uma versão de recursos de infraestrutura suportadas. Como e quando podemos anunciar o lançamento de uma nova versão do service fabric, a versão anterior está marcada para o fim do suporte após um mínimo de 60 dias dessa data. As novas versões sejam anunciadas [no blog da equipe de recursos de infraestrutura do serviço](https://blogs.msdn.microsoft.com/azureservicefabric/). A nova versão está disponível para, em seguida, escolha. 
> 
> 

14 dias antes da expiração da versão do que seu cluster está em execução, estado de funcionamento é gerado um evento que coloca o seu cluster num Estado de funcionamento de aviso. O cluster permanecer num Estado de aviso enquanto não atualizar para uma versão de recursos de infraestrutura suportadas.

### <a name="setting-the-upgrade-mode-via-portal"></a>Definir o modo de atualização através do portal
Pode configurar o cluster para automático ou manual quando estiver a criar o cluster.

![Create_Manualmode][Create_Manualmode]

Pode configurar o cluster para automático ou manual quando num cluster em direto, utilizando a experiência de gerir. 

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal"></a>A atualizar para uma nova versão de um cluster que está definido para o modo Manual através do portal.
Para atualizar para uma nova versão, tudo o que precisa fazer é selecionar a versão disponível no menu pendente e guardar. A atualização de recursos de infraestrutura é iniciada automaticamente. As políticas de estado de funcionamento do cluster (uma combinação de estado de funcionamento do nó e o estado de funcionamento todos os aplicativos em execução no cluster) sejam seguidos durante a atualização.

Se as políticas de estado de funcionamento do cluster não forem cumpridas, a atualização é revertida. Desloque para baixo deste documento para ler mais sobre como configurar as políticas de estado de funcionamento personalizados. 

Depois de corrigir os problemas que resultaram na reversão, terá de iniciar a atualização novamente, ao seguir os mesmos passos.

![Manage_Automaticmode][Manage_Automaticmode]

### <a name="setting-the-upgrade-mode-via-a-resource-manager-template"></a>Definir o modo de atualização por meio de um modelo do Resource Manager
Adicionar a configuração de "upgradeMode" para a definição do recurso Microsoft.ServiceFabric/clusters e defina "clusterCodeVersion" para uma das versões de recursos de infraestrutura suportadas, conforme mostrado abaixo e, em seguida, implementar o modelo. Os valores válidos para "upgradeMode" são "Manual" ou "Automático"

![ARMUpgradeMode][ARMUpgradeMode]

#### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-a-resource-manager-template"></a>A atualizar para uma nova versão de um cluster que está definido para o modo Manual através de um modelo do Resource Manager.
Quando o cluster está no modo Manual, para atualizar para uma nova versão, altere o "clusterCodeVersion" para uma versão suportada e implementá-lo. A implementação do modelo, entra em ação da atualização do Fabric é iniciada automaticamente. As políticas de estado de funcionamento do cluster (uma combinação de estado de funcionamento do nó e o estado de funcionamento todos os aplicativos em execução no cluster) sejam seguidos durante a atualização.

Se as políticas de estado de funcionamento do cluster não forem cumpridas, a atualização é revertida. Desloque para baixo deste documento para ler mais sobre como configurar as políticas de estado de funcionamento personalizados. 

Depois de corrigir os problemas que resultaram na reversão, terá de iniciar a atualização novamente, ao seguir os mesmos passos.

### <a name="get-list-of-all-available-version-for-all-environments-for-a-given-subscription"></a>Obter lista de todas as versões disponíveis para todos os ambientes de uma determinada subscrição
Execute o seguinte comando, e deverá obter um resultado semelhante ao seguinte.

"supportExpiryUtc" informa ao seu quando uma determinada versão está prestes a expirar ou expirou. A versão mais recente não tem uma data válida – ele tem um valor de "9999-12-31T23:59:59.9999999", que significa apenas que a data de expiração não estiver ainda definida.

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/locations/{{location}}/clusterVersions?api-version=2016-09-01

Example: https://management.azure.com/subscriptions/1857f442-3bce-4b96-ad95-627f76437a67/providers/Microsoft.ServiceFabric/locations/eastus/clusterVersions?api-version=2016-09-01

Output:
{
                  "value": [
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
                      "name": "5.0.1427.9490",
                      "type": "Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.0.1427.9490",
                        "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
                      "name": "5.1.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.1.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
                      "name": "4.4.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "4.4.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Linux"
                      }
                    }
                  ]
                }


```

## <a name="fabric-upgrade-behavior-when-the-cluster-upgrade-mode-is-automatic"></a>Comportamento de atualização de recursos de infraestrutura a pedido quando o modo de atualização do cluster é automático
A Microsoft tem o código de recursos de infraestrutura e a configuração que é executado num cluster do Azure. Executamos automática de atualizações monitorizadas do software de forma conforme necessário. Essas atualizações podem ser de código, configuração ou ambos. Para certificar-se de que a sua aplicação sofre sem impacto ou um impacto mínimo devido a essas atualizações, podemos execute as atualizações nas seguintes fases:

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a>Fase 1: Uma atualização é executada com todas as políticas de estado de funcionamento do cluster
Durante esta fase, as atualizações continuar um domínio de atualização por vez e os aplicativos que estavam em execução no cluster continuam a ser executado sem qualquer período de inatividade. As políticas de estado de funcionamento do cluster (uma combinação de estado de funcionamento do nó e o estado de funcionamento todos os aplicativos em execução no cluster) sejam seguidos durante a atualização.

Se as políticas de estado de funcionamento do cluster não forem cumpridas, a atualização é revertida. Em seguida, é enviado um e-mail para o proprietário da subscrição. O e-mail contém as seguintes informações:

* Notificação de que tínhamos que reverter uma atualização do cluster.
* Sugestões de tomar medidas corretivas, se houver.
* O número de dias (n) até que executamos fase 2.

Tentamos executar a mesma atualização algumas mais vezes no caso de falha de quaisquer atualizações por motivos de infraestrutura. Após os n dias a contar da data, que o e-mail foi enviado, podemos avançar para a fase 2.

As políticas de estado de funcionamento do cluster forem cumpridos, a atualização é considerada bem-sucedida e marcada como concluída. Isto pode acontecer durante a atualização inicial ou qualquer um da volta a executar atualização nesta fase. Não há nenhum e-mail de confirmação de uma execução com êxito. Isso é evitar o envio de demasiadas mensagens de correio eletrónico, receber uma mensagem de e-mail deve ser vista como uma exceção ao normal. Esperamos que a maioria das atualizações de cluster tenha êxito sem afetar a disponibilidade de aplicações.

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a>Fase 2: Uma atualização é executada ao utilizar políticas de estado de funcionamento padrão apenas
As políticas de estado de funcionamento nesta fase são definidas de tal forma que o número de aplicativos que estavam em bom estado no início da atualização do permanece igual durante o processo de atualização. Como na fase 1, as atualizações de fase 2 continuar um domínio de atualização por vez e os aplicativos que estavam em execução no cluster continuam a ser executado sem qualquer período de inatividade. As políticas de estado de funcionamento do cluster (uma combinação de estado de funcionamento do nó e o estado de funcionamento todos os aplicativos em execução no cluster) sejam seguidos durante a atualização.

Se as políticas de estado de funcionamento do cluster em vigor não forem cumpridas, a atualização é revertida. Em seguida, é enviado um e-mail para o proprietário da subscrição. O e-mail contém as seguintes informações:

* Notificação de que tínhamos que reverter uma atualização do cluster.
* Sugestões de tomar medidas corretivas, se houver.
* O número de dias (n) até que executamos fase 3.

Tentamos executar a mesma atualização algumas mais vezes no caso de falha de quaisquer atualizações por motivos de infraestrutura. Um e-mail de lembrete é enviado alguns dias antes de n dias estão operacionais. Após os n dias a contar da data, que o e-mail foi enviado, podemos avançar para a fase 3. Os e-mails, que podemos enviar-lhe na fase 2 devem ser levados a sério e é necessário tomar medidas corretivas.

As políticas de estado de funcionamento do cluster forem cumpridos, a atualização é considerada bem-sucedida e marcada como concluída. Isto pode acontecer durante a atualização inicial ou qualquer um da volta a executar atualização nesta fase. Não há nenhum e-mail de confirmação de uma execução com êxito.

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>Fase 3: Uma atualização é executada ao utilizar políticas de estado de funcionamento agressivo
Estas políticas de estado de funcionamento nesta fase são direcionadas para conclusão da atualização em vez do Estado de funcionamento dos aplicativos. Atualizações de cluster muito poucos acabam nesta fase. Se obtiver seu cluster para esta fase, existe uma boa chance de que a aplicação fica em mau estado de funcionamento e/ou perder a disponibilidade.

Assim como as outras duas fases, atualizações de fase 3 continuar um domínio de atualização de cada vez.

Se as políticas de estado de funcionamento do cluster não forem cumpridas, a atualização é revertida. Tentamos executar a mesma atualização algumas mais vezes no caso de falha de quaisquer atualizações por motivos de infraestrutura. Depois disso, o cluster está afixado, para que ele deixará de receber suporte e/ou as atualizações.

É enviado um e-mail com essas informações para o proprietário da subscrição, juntamente com as ações corretivas. Não Esperamos quaisquer clusters para chegar a um Estado em que a fase 3 falhou.

As políticas de estado de funcionamento do cluster forem cumpridos, a atualização é considerada bem-sucedida e marcada como concluída. Isto pode acontecer durante a atualização inicial ou qualquer um da volta a executar atualização nesta fase. Não há nenhum e-mail de confirmação de uma execução com êxito.

## <a name="cluster-configurations-that-you-control"></a>Configurações de cluster que controla
Para além da capacidade para definir o cluster do modo de atualização, aqui estão as configurações que podem ser alteradas num cluster ativo.

### <a name="certificates"></a>Certificados
Pode adicionar novo ou eliminar facilmente os certificados para o cluster e o cliente através do portal. Consulte [neste documento para obter instruções detalhadas](service-fabric-cluster-security-update-certs-azure.md)

![Captura de ecrã que mostra os thumbprints de certificado no portal do Azure.][CertificateUpgrade]

### <a name="application-ports"></a>Portas da aplicação
Pode alterar as portas de aplicação ao alterar as propriedades de recurso do Balanceador de carga que estão associadas com o tipo de nó. Pode utilizar o portal ou pode utilizar o PowerShell do Resource Manager diretamente.

Para abrir uma nova porta em todas as VMs num tipo de nó, efetue o seguinte:

1. Adicione uma nova sonda ao balanceador de carga adequado.
   
    Se tiver implementado o cluster com o portal, os balanceadores de carga com o nome "LB-nome do grupo de recursos-NodeTypename", um para cada tipo de nó. Uma vez que os nomes de Balanceador de carga são exclusivos apenas dentro de um grupo de recursos, é melhor se pesquisar por-los num grupo de recursos específico.
   
    ![Captura de ecrã que mostra a adição de uma sonda para um balanceador de carga no portal.][AddingProbes]
2. Adicione uma nova regra para o Balanceador de carga.
   
    Adicione uma nova regra para o mesmo Balanceador de carga ao utilizar a sonda que criou no passo anterior.
   
    ![Adicionar uma nova regra a um balanceador de carga no portal.][AddingLBRules]

### <a name="placement-properties"></a>Propriedades de colocação
Para cada tipo de nó, pode adicionar propriedades de colocação personalizada que pretende utilizar nas suas aplicações. O NodeType é uma propriedade padrão que pode utilizar sem adicioná-lo explicitamente.

> [!NOTE]
> Para obter detalhes sobre a utilização de restrições de posicionamento, propriedades de nó e como defini-las, consulte a seção "Colocação restrições e propriedades de nó" no documento do Gestor de recursos de Cluster do Service Fabric no [descrevendo o seu Cluster](service-fabric-cluster-resource-manager-cluster-description.md).
> 
> 

### <a name="capacity-metrics"></a>Métricas de capacidade
Para cada tipo de nó, pode adicionar métricas de capacidade personalizado que pretende utilizar nas suas aplicações para comunicar carga. Para obter detalhes sobre o uso de métricas de capacidade para comunicar carga, consulte os documentos de Gestor de recursos de Cluster do Service Fabric no [que descreva o seu Cluster](service-fabric-cluster-resource-manager-cluster-description.md) e [métricas e carga](service-fabric-cluster-resource-manager-metrics.md).

### <a name="fabric-upgrade-settings---health-polices"></a>Definições de atualização de recursos de infraestrutura - as políticas de estado de funcionamento
Pode especificar políticas de estado de funcionamento personalizado para atualização do fabric. Se tiver definido o seu cluster a atualizações de recursos de infraestrutura automática, em seguida, estas políticas são aplicadas a fase 1 das atualizações automáticas de recursos de infraestrutura.
Se tiver definido o cluster para recursos de infraestrutura Manual atualizações, estas políticas são aplicadas sempre que selecione uma nova versão do sistema para iniciar a atualização de recursos de infraestrutura no seu cluster a acionar. Se não substituir as políticas, são utilizadas as predefinições.

Pode especificar as políticas de estado de funcionamento personalizado ou reveja as definições atuais no painel de "atualização do fabric", ao selecionar as definições avançadas de atualização. Reveja a imagem seguinte sobre como. 

![Gerir políticas de estado de funcionamento personalizados][HealthPolices]

### <a name="customize-fabric-settings-for-your-cluster"></a>Personalizar as definições de recursos de infraestrutura para o seu cluster
Consulte a [definições de recursos de infraestrutura de cluster de recursos de infraestrutura do serviço](service-fabric-cluster-fabric-settings.md) sobre o que e como pode personalizá-las.

### <a name="os-patches-on-the-vms-that-make-up-the-cluster"></a>Patches de SO nas VMs que compõem o cluster
Consulte a [Patch Orchestration Application](service-fabric-patch-orchestration-application.md) que podem ser implementadas no seu cluster para instalar os patches do Windows Update de forma organizada, manter os serviços disponíveis, o tempo todo. 

### <a name="os-upgrades-on-the-vms-that-make-up-the-cluster"></a>Atualizações de SO nas VMs que compõem o cluster
Se tem de atualizar a imagem do SO nas máquinas virtuais do cluster, é feita a uma VM ao mesmo tempo. É responsável para esta atualização, não é atualmente sem automatização para isso.

## <a name="next-steps"></a>Passos Seguintes
* Saiba como personalizar algumas do [definições de recursos de infraestrutura de cluster de recursos de infraestrutura do serviço](service-fabric-cluster-fabric-settings.md)
* Saiba como [dimensionar e reduzir o seu cluster](service-fabric-cluster-scale-up-down.md)
* Saiba mais sobre [as atualizações de aplicações](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG
