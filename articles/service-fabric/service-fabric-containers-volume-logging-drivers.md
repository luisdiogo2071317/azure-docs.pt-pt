---
title: Azure Service Fabric Docker Compose (pré-visualização) | Microsoft Docs
description: Azure Service Fabric aceita o formato Docker Compose para tornar mais fácil orquestrar contentores existentes utilizando o Service Fabric. Suporte para o Docker Compose está atualmente em pré-visualização.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: 72ff8e99453d3067492d50c3e35396dba9f23c12
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="use-docker-volume-plug-ins-and-logging-drivers-in-your-container"></a>Utilizar o volume de Docker plug-ins e controladores de registo no contentor de
Recursos de infraestrutura de serviço do Azure suporta a especificação de [Docker volume plug-ins](https://docs.docker.com/engine/extend/plugins_volume/) e [controladores de registo de Docker](https://docs.docker.com/engine/admin/logging/overview/) para o seu serviço de contentor. Pode manter os dados no [ficheiros do Azure](https://azure.microsoft.com/services/storage/files/) quando o contentor é movido ou reiniciado num anfitrião diferente.

Apenas os controladores de volume para contentores de Linux são atualmente suportados. Se estiver a utilizar os contentores do Windows, pode mapear um volume a um Azure ficheiros [SMB3 partilha](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/) sem um controlador de volume. Para que este mapeamento, Atualize as máquinas virtuais (VMs) do cluster para a versão mais recente do Windows Server 1709.


## <a name="install-the-docker-volumelogging-driver"></a>Instale o controlador de volume/registo de Docker

Se o controlador de volume/registo de Docker não está instalado no computador, pode instalá-lo manualmente utilizando os protocolos RDP/SSH. Pode efetuar a instalação com estes protocolos através de um [conjunto de dimensionamento de máquina virtual de script de arranque](https://azure.microsoft.com/resources/templates/201-vmss-custom-script-windows/) ou um [SetupEntryPoint script](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-model#describe-a-service).

Um exemplo de script para instalar o [controladores de volume do Docker para o Azure](https://docs.docker.com/docker-for-azure/persistent-data-volumes/) é o seguinte:

```bash
docker plugin install --alias azure --grant-all-permissions docker4x/cloudstor:17.09.0-ce-azure1  \
    CLOUD_PLATFORM=AZURE \
    AZURE_STORAGE_ACCOUNT="[MY-STORAGE-ACCOUNT-NAME]" \
    AZURE_STORAGE_ACCOUNT_KEY="[MY-STORAGE-ACCOUNT-KEY]" \
    DEBUG=1
```

> [!NOTE]
> O Centro de dados do Windows Server 2016 não suporta o mapeamento SMB monta para contentores ([que só é suportada no Windows Server versão 1709](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/container-storage)). Esta restrição impede o mapeamento do volume de rede e controladores de volume de ficheiros do Azure nas versões mais antigas do que 1709. 
>   


## <a name="specify-the-plug-in-or-driver-in-the-manifest"></a>Especifique o plug-in ou controlador no manifesto
Os plug-ins são especificados no manifesto da aplicação da seguinte forma:

```xml
?xml version="1.0" encoding="UTF-8"?>
<ApplicationManifest ApplicationTypeName="WinNodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <Description>Calculator Application</Description>
    <Parameters>
      <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
      <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
      <Parameter Name="MyStorageVar" DefaultValue="c:\tmp"></Parameter>
    </Parameters>
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeServicePackage" ServiceManifestVersion="1.0"/>
     <Policies>
       <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv"> 
        <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
        <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
        <LogConfig Driver="etwlogs" >
          <DriverOption Name="test" Value="vale"/>
        </LogConfig>
        <Volume Source="c:\workspace" Destination="c:\testmountlocation1" IsReadOnly="false"></Volume>
        <Volume Source="[MyStorageVar]" Destination="c:\testmountlocation2" IsReadOnly="true"> </Volume>
        <Volume Source="myvolume1" Destination="c:\testmountlocation2" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
        </Volume>
       </ContainerHostPolicies>
   </Policies>
    </ServiceManifestImport>
    <ServiceTemplates>
        <StatelessService ServiceTypeName="StatelessNodeService" InstanceCount="5">
            <SingletonPartition></SingletonPartition>
        </StatelessService>
    </ServiceTemplates>
</ApplicationManifest>
```

O **origem** etiqueta para o **Volume** elemento refere-se para a pasta de origem. A pasta de origem pode ser uma pasta na VM que aloja os contentores ou um arquivo persistente remoto. O **destino** etiquetas é a localização que o **origem** está mapeado para no contentor em execução. Assim, o seu destino não pode ser uma localização que já existe no contentor de.

São suportados parâmetros de aplicação para volumes, conforme ilustrado no fragmento de manifesto anterior (procure `MyStoreVar` para obter um exemplo utilizar).

Quando especificar um plug-in de volume, o Service Fabric cria automaticamente o volume utilizando os parâmetros especificados. O **origem** etiqueta é o nome do volume e o **controlador** tag Especifica o plug-in de controlador de volume. Podem ser especificadas opções utilizando o **DriverOption** tag da seguinte forma:

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```
Se não for especificado um controlador de registo do Docker, tem de implementar agentes (ou contentores) para processar os registos no cluster. O **DriverOption** etiqueta pode ser utilizada para especificar as opções do controlador de registo.

## <a name="next-steps"></a>Passos Seguintes
Para implementar um cluster do Service Fabric contentores, consulte o artigo [implementar um contentor no Service Fabric](service-fabric-deploy-container.md).
