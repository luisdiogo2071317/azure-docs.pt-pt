---
title: Atualizar um cluster autónomo de Azure Service Fabric no Windows Server | Documentos da Microsoft
description: Atualize o código do Azure Service Fabric e/ou a configuração que executa um cluster do Service Fabric autónomo, incluindo a definir o modo de atualização do cluster.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: 66296cc6-9524-4c6a-b0a6-57c253bdf67e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 93b79b7adacdec18912d28bb9725e2dc77737d59
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43050768"
---
# <a name="upgrade-your-standalone-azure-service-fabric-cluster-on-windows-server"></a>Atualizar o seu cluster do Azure Service Fabric autónomo no Windows Server 
> [!div class="op_single_selector"]
> * [Cluster do Azure](service-fabric-cluster-upgrade.md)
> * [Cluster autónomo](service-fabric-cluster-upgrade-windows-server.md)
>
>

Para qualquer sistema moderno, a capacidade de atualizar é fundamental para o sucesso de longo prazo de seu produto. Um cluster do Azure Service Fabric é um recurso que é proprietário. Este artigo descreve como pode tornar-se de que o cluster executa sempre as versões suportadas do código do Service Fabric e configurações.

## <a name="control-the-service-fabric-version-that-runs-on-your-cluster"></a>Controlar a versão de Service Fabric que é executado no seu cluster
Para configurar o seu cluster para transferir as atualizações do Service Fabric, quando a Microsoft lança uma nova versão, definir a configuração de cluster fabricClusterAutoupgradeEnabled *true*. Para selecionar uma versão suportada do Service Fabric que pretende que o seu cluster estar no, defina a configuração de cluster fabricClusterAutoupgradeEnabled como *false*.

> [!NOTE]
> Certifique-se de que o cluster é sempre executada uma versão suportada do Service Fabric. Quando a Microsoft anuncia o lançamento de uma nova versão do Service Fabric, a versão anterior está marcada para o fim do suporte após um mínimo de 60 dias a contar da data do anúncio. Novas versões sejam anunciados [no blog da equipe de recursos de infraestrutura do serviço](https://blogs.msdn.microsoft.com/azureservicefabric/). A nova versão está disponível para escolher nesse ponto.
>
>

Pode atualizar o seu cluster para a nova versão apenas se estiver a utilizar uma configuração de nó de estilo de produção, em que cada nó do Service Fabric é alocado numa máquina virtual ou físico separado. Se tiver um cluster de desenvolvimento, onde mais de um nó do Service Fabric está numa única máquina física ou virtual, tem de voltar a criar o cluster com a nova versão.

Dois fluxos de trabalho distintos podem atualizar o seu cluster para a versão mais recente ou uma versão suportada do Service Fabric. Um fluxo de trabalho destina-se a clusters que tem conectividade para transferir a versão mais recente automaticamente. O outro fluxo de trabalho destina-se a clusters que não tem ligação para transferir a versão mais recente do Service Fabric.

### <a name="upgrade-clusters-that-have-connectivity-to-download-the-latest-code-and-configuration"></a>Atualizar clusters que tem conectividade para transferir o código e a configuração mais recente
Utilize estes passos para atualizar o cluster para uma versão suportada, se os nós do cluster tem conectividade de internet para o [Microsoft Download Center](http://download.microsoft.com).

Para os clusters que tem conectividade para o [Microsoft Download Center](http://download.microsoft.com), Microsoft verifica periodicamente a disponibilidade de novas versões de Service Fabric.

Quando uma nova versão do Service Fabric está disponível, o pacote é transferido localmente para o cluster e provisionado para atualização. Além disso, para informar o cliente dessa nova versão, o sistema mostra um aviso de estado de funcionamento do cluster explícito que é semelhante ao seguinte:

"A versão do cluster atual [versão n. º] o suporte termina [date]."

Depois do cluster está a executar a versão mais recente, o aviso desaparece.

#### <a name="cluster-upgrade-workflow"></a>Fluxo de trabalho de atualização de cluster
Quando vir o aviso de estado de funcionamento do cluster, efetue o seguinte:

1. Ligar ao cluster a partir de qualquer máquina que tenha acesso de administrador para todas as máquinas que estão listadas como nós do cluster. A máquina que este script é executado não tem a fazer parte do cluster.

    ```powershell

    ###### connect to the secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3"
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Obter a lista de versões de Service Fabric, que pode atualizar para o.

    ```powershell

    ###### Get the list of available Service Fabric versions
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    Deverá obter um resultado semelhante ao seguinte:

    ![Obtenha versões de Service Fabric][getfabversions]
3. Iniciar uma atualização de cluster para uma versão disponível com o [Start-ServiceFabricClusterUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterupgrade) comando do Windows PowerShell.

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   Para monitorizar o progresso da atualização, pode utilizar o Service Fabric Explorer ou execute o seguinte comando do PowerShell:

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    Se não forem cumpridas as políticas de estado de funcionamento do cluster, a atualização é revertida. Para especificar políticas de estado de funcionamento personalizados para o comando de início ServiceFabricClusterUpgrade, consulte a documentação para [Start-ServiceFabricClusterUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterupgrade).

    Depois de corrigir os problemas que resultaram na reversão, inicie novamente a atualização ao seguir os mesmos passos conforme descrito anteriormente.

### <a name="upgrade-clusters-that-have-no-connectivity-to-download-the-latest-code-and-configuration"></a>Atualizar clusters que tenham *sem conectividade* para transferir o código e a configuração mais recente
Utilize estes passos para atualizar o cluster para uma versão suportada, se os nós do cluster não tem ligação à internet para o [Microsoft Download Center](http://download.microsoft.com).

> [!NOTE]
> Se estiver a executar um cluster que não está ligado à internet, terá de monitorizar o blogue da equipa do Service Fabric para saber mais sobre novas versões. O sistema não mostra um aviso de estado de funcionamento do cluster para o alertar de novas versões.  
>
>

#### <a name="auto-provisioning-vs-manual-provisioning"></a>Provisionamento versus manual de aprovisionamento automático
Para ativar a transferência automática e de registo para a versão mais recente do código, configure o serviço de atualização do Service Fabric. Para obter instruções, consulte Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt dentro de [pacote autónomo](service-fabric-cluster-standalone-package-contents.md).
Para o processo manual, siga estas instruções.

Modificar a configuração do cluster para definir a seguinte propriedade como *false* antes de iniciar uma atualização de configuração:

        "fabricClusterAutoupgradeEnabled": false,

Para obter detalhes de utilização, consulte a [Start-ServiceFabricClusterConfigurationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade) comando do PowerShell. Certifique-se atualizar clusterConfigurationVersion no JSON antes de iniciar a atualização de configuração.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```

#### <a name="cluster-upgrade-workflow"></a>Fluxo de trabalho de atualização de cluster

1. Execute [Get-ServiceFabricClusterUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterupgrade) a partir de um de nós no cluster e tenha em atenção a *TargetCodeVersion*.

2. Execute o seguinte a partir de uma máquina ligada à internet para listar todas as versões de atualização compatível com a versão atual e transferir o pacote correspondente a partir de ligações de transferência associados:

    ```powershell

    ###### Get list of all upgrade compatible packages  
    Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion <TargetCodeVersion as noted in Step 1> 
    ```

3. Ligar ao cluster a partir de qualquer máquina que tenha acesso de administrador para todas as máquinas que estão listadas como nós do cluster. A máquina que este script é executado não tem a fazer parte do cluster.

    ```powershell

   ###### Get the list of available Service Fabric versions
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

   ###### Here is a filled-out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"

    ```
4. Copie o pacote transferido para o arquivo de imagem do cluster.

5. Registe o pacote copiado.

    ```powershell

    ###### Get the list of available Service Fabric versions
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file>

    ###### Here is a filled-out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```
6. Inicie uma atualização de cluster para uma versão disponível.

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   Pode monitorizar o progresso da atualização no Service Fabric Explorer, ou pode executar o seguinte comando do PowerShell:

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    Se não forem cumpridas as políticas de estado de funcionamento do cluster, a atualização é revertida. Para especificar políticas de estado de funcionamento personalizados para o comando de início ServiceFabricClusterUpgrade, consulte a documentação para [Start-ServiceFabricClusterUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterupgrade).

    Depois de corrigir os problemas que resultaram na reversão, inicie novamente a atualização ao seguir os mesmos passos conforme descrito anteriormente.


## <a name="upgrade-the-cluster-configuration"></a>Atualizar a configuração de cluster
Antes de iniciar a atualização de configuração, pode testar a nova configuração de cluster JSON ao executar o seguinte script do PowerShell no pacote autónomo:

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File>

```
Em alternativa, utilize este script:

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File> -FabricRuntimePackagePath <Path to the .cab file which you want to test the configuration against>

```

Algumas configurações não podem ser atualizado, como pontos de extremidade, nome do cluster, IP de nó, etc. A nova configuração de cluster JSON é testada em relação ao antigo e gera erros na janela do PowerShell, se houver um problema.

Para atualizar a atualização de configuração do cluster, execute [Start-ServiceFabricClusterConfigurationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade). A atualização de configuração é o domínio de atualização processado por domínio de atualização.

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```

### <a name="cluster-certificate-config-upgrade"></a>Atualização de configuração do certificado de cluster  
Um certificado de cluster é utilizado para autenticação entre nós de cluster. O rollover de certificado deve ser realizado com cuidado extra porque falha bloqueia a comunicação entre nós de cluster.

Tecnicamente, são suportadas quatro opções:  

* Atualização do certificado único: O caminho de atualização é o certificado de um site (primário) -> B de certificado (primário) -> C de certificado (primário) ->...

* Atualização do certificado Double: O caminho de atualização é um site (primário) de certificado do certificado (primário) -> e B (secundário) -> B de certificado (primário) -> (primário) do certificado B e C (secundário) -> C de certificado (primário) ->...

* Atualização do tipo de certificado: configuração de certificado com base em CommonName de configuração <> – baseado na Thumbprint do certificado. Por exemplo, o Thumbprint do certificado (primário) e Thumbprint B (secundário) -> C. CommonName do certificado

* Atualização de thumbprint do emissor de certificado: O caminho de atualização é o CN do certificado = A, IssuerThumbprint = IT1 (principal) -> CN do certificado = A, IssuerThumbprint = IT1, IT2 (principal) -> certificado CN = A, IssuerThumbprint = IT2 (principal).


## <a name="next-steps"></a>Passos Seguintes
* Saiba como personalizar algumas [definições de cluster do Service Fabric](service-fabric-cluster-fabric-settings.md).
* Saiba como [dimensionar o seu cluster e reduzir](service-fabric-cluster-scale-up-down.md).
* Saiba mais sobre [as atualizações de aplicações](service-fabric-application-upgrade.md).

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
