---
title: Azure de recursos de infraestrutura do serviço ficheiros de controlador de Volume (pré-visualização) | Microsoft Docs
description: Service Fabric suporta utilizando ficheiros do Azure para volumes de cópia de segurança do seu contentor. Esta é atualmente em pré-visualização.
services: service-fabric
documentationcenter: other
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: other
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 4/30/2018
ms.author: subramar
ms.openlocfilehash: 2d98cff1a5869091aa81097bbb34da6e525a2ad5
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="service-fabric-azure-files-volume-driver-preview"></a>Controlador de Volume de ficheiros do Azure do serviço Fabric (pré-visualização)
O plug-in de volume de ficheiros do Azure é um [Plug-in do Docker volume](https://docs.docker.com/engine/extend/plugins_volume/) que fornece [ficheiros do Azure](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) com base em volumes para contentores de Docker. Este plug-in do Docker volume é empacotada como uma aplicação de Service Fabric que pode ser implementada nos clusters de Service Fabric. Objetivo de consiste em fornecer ficheiros do Azure com base em volumes para outras aplicações de contentor do Service Fabric que são implementadas para o cluster.

> [!NOTE]
> A versão 6.255.389.9494 do plug-in de volume de ficheiros do Azure é uma versão de pré-visualização está disponível com este documento. Como uma versão de pré-visualização, é **não** suportados para utilização em ambientes de produção.
>

## <a name="prerequisites"></a>Pré-requisitos
* A versão do Windows do plug-in de volume de ficheiros do Azure funciona em [Windows Server versão 1709](https://docs.microsoft.com/en-us/windows-server/get-started/whats-new-in-windows-server-1709), [Windows 10 versão 1709](https://docs.microsoft.com/en-us/windows/whats-new/whats-new-windows-10-version-1709) ou apenas os sistemas de operativos posteriores. A versão do Linux do plug-in de volume de ficheiros do Azure funciona em todas as versões de sistema operativo suportadas pelo Service Fabric.

* Siga as instruções no [documentação de ficheiros do Azure](https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-create-file-share) para criar uma partilha de ficheiros para a aplicação de contentor do Service Fabric para utilizar como volume.

* Terá de [Powershell com o módulo do Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-get-started) ou [SFCTL](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cli) instalado.

## <a name="deploy-the-service-fabric-azure-files-application"></a>Implementar a aplicação de ficheiros do Service Fabric do Azure

Pode ser transferida a aplicação de Service Fabric fornece os volumes para os contentores seguintes [ligação](https://aka.ms/sfvolume). A aplicação pode ser implementada para o cluster através de [PowerShell](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-deploy-remove-applications), [CLI](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-application-lifecycle-sfctl) ou [FabricClient APIs](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-deploy-remove-applications-fabricclient).

1. Utilizando a linha de comandos, altere o diretório para o diretório de raiz do pacote de aplicações transferido. 

    ```powershell 
    cd .\AzureFilesVolume\
    ```

    ```bash
    cd ~/AzureFilesVolume
    ```

2. Copie o pacote de aplicação para o arquivo de imagens, execute o comando abaixo com o valor adequado para [ApplicationPackagePath] e [ImageStoreConnectionString]:

    ```powershell
    Copy-ServiceFabricApplicationPackage -ApplicationPackagePath [ApplicationPackagePath] -ImageStoreConnectionString [ImageStoreConnectionString] -ApplicationPackagePathInImageStore AzureFilesVolumePlugin
    ```

    ```bash
    sfctl cluster select --endpoint https://testcluster.westus.cloudapp.azure.com:19080 --pem test.pem --no-verify
    sfctl application upload --path [ApplicationPackagePath] --show-progress
    ```

3. Registar o tipo de aplicação

    ```powershell
    Register-ServiceFabricApplicationType -ApplicationPathInImageStore AzureFilesVolumePlugin
    ```

    ```bash
    sfctl application provision --application-type-build-path [ApplicationPackagePath]
    ```

4. Criar a aplicação no comando para criar a aplicação abaixo, tenha em atenção o **ListenPort** parâmetro de aplicação. Este valor especificado para este parâmetro de aplicação é a porta em que o plug-in de volume de ficheiros do Azure escuta pedidos do daemon de Docker. É importante certificar-se de que a porta fornecida para a aplicação não estão em conflito com qualquer porta que utilizam o cluster ou das suas aplicações.

    ```powershell
    New-ServiceFabricApplication -ApplicationName fabric:/AzureFilesVolumePluginApp -ApplicationTypeName AzureFilesVolumePluginType -ApplicationTypeVersion 6.255.389.9494 -ApplicationParameter @{ListenPort='19100'}
    ```

    ```bash
    sfctl application create --app-name fabric:/AzureFilesVolumePluginApp --app-type AzureFilesVolumePluginType --app-version 6.255.389.9494 --parameter '{"ListenPort":"19100"}'
    ```

> [!NOTE]

> O Centro de dados do Windows Server 2016 não suporta o mapeamento SMB monta para contentores ([que só é suportada no Windows Server versão 1709](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/container-storage)). Esta restrição impede o mapeamento do volume de rede e controladores de volume de ficheiros do Azure nas versões mais antigas do que 1709. 
>   

### <a name="deploy-the-application-on-a-local-development-cluster"></a>Implementar a aplicação num cluster de desenvolvimento local
A contagem de instâncias de serviço predefinido para a aplicação de plug-in do volume de ficheiros do Azure é -1, o que significa que existe uma instância do serviço implementado para cada nó do cluster. No entanto, quando implementar a aplicação de plug-in do volume de ficheiros do Azure num cluster de desenvolvimento local, a contagem de instâncias de serviço deve ser especificada como 1. Isto pode ser feito através de **InstanceCount** parâmetro de aplicação. Por conseguinte, o comando para implementar a aplicação de plug-in do volume de ficheiros do Azure num cluster de desenvolvimento local é:

```powershell
New-ServiceFabricApplication -ApplicationName fabric:/AzureFilesVolumePluginApp -ApplicationTypeName AzureFilesVolumePluginType -ApplicationTypeVersion 6.255.389.9494 -ApplicationParameter @{ListenPort='19100';InstanceCount='1'}
```

```bash
sfctl application create --app-name fabric:/AzureFilesVolumePluginApp --app-type AzureFilesVolumePluginType --app-version 6.255.389.9494 --parameter '{"ListenPort": "19100","InstanceCount": "1"}'
```
## <a name="configure-your-applications-to-use-the-volume"></a>Configurar as suas aplicações para utilizar o volume
O fragmento seguinte mostra como pode ser especificado um volume de ficheiros do Azure com base no manifesto da aplicação da sua aplicação. O elemento específico de interesse é o **Volume** etiquetas:

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
            <Volume Source="azfiles" Destination="c:\VolumeTest\Data" Driver="sfazurefile">
                <DriverOption Name="shareName" Value="" />
                <DriverOption Name="storageAccountName" Value="" />
                <DriverOption Name="storageAccountKey" Value="" />
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

O nome de controlador para o plug-in de volume de ficheiros do Azure é **sfazurefile**. Este valor é definido para o **controlador** atributo o **Volume** elemento no manifesto da aplicação.

No **Volume** elemento no fragmento acima, o plug-in de volume de ficheiros do Azure requer as seguintes tags: 
- **Origem** -Isto refere-se para a pasta de origem que pode ser uma pasta na VM que aloja os contentores ou um arquivo persistente remoto
- **Destino** -esta etiqueta é a localização que o **origem** está mapeado para no contentor em execução. Assim, o seu destino não pode ser uma localização que já existe no contentor de

Conforme mostrado no **DriverOption** elementos no fragmento acima, o plug-in de volume de ficheiros do Azure suporta as seguintes opções de controlador:

- **shareName** -nome da partilha de ficheiros de ficheiros do Azure que fornece o volume para o contentor
- **storageAccountName** – nome da conta do storage do Azure que contém o ficheiro de ficheiros do Azure partilhar
- **storageAccountKey** -chave de acesso para a conta de armazenamento do Azure que contém a partilha de ficheiros de ficheiros do Azure

Todas as opções de controlador acima são necessárias. 

## <a name="using-your-own-volume-or-logging-driver"></a>Utilizar os seus próprios volume ou controlador de registo
Recursos de infraestrutura de serviço também permite a utilização do seu próprio volume personalizado ou de controladores de registo. Se o controlador de volume/registo de Docker não está instalado no cluster, pode instalá-lo manualmente utilizando os protocolos RDP/SSH. Pode efetuar a instalação com estes protocolos através de um [conjunto de dimensionamento de máquina virtual de script de arranque](https://azure.microsoft.com/resources/templates/201-vmss-custom-script-windows/) ou um [SetupEntryPoint script](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-model#describe-a-service).

Um exemplo de script para instalar o [controladores de volume do Docker para o Azure](https://docs.docker.com/docker-for-azure/persistent-data-volumes/) é o seguinte:

```bash
docker plugin install --alias azure --grant-all-permissions docker4x/cloudstor:17.09.0-ce-azure1  \
    CLOUD_PLATFORM=AZURE \
    AZURE_STORAGE_ACCOUNT="[MY-STORAGE-ACCOUNT-NAME]" \
    AZURE_STORAGE_ACCOUNT_KEY="[MY-STORAGE-ACCOUNT-KEY]" \
    DEBUG=1
```

Nas suas aplicações, ao utilizar o volume ou controlador de registo instalado, terá de especificar os valores adequados no **Volume** e **LogConfig** elementos em  **ContainerHostPolicies** no seu manifesto de aplicação. 

```xml
<ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv"> 
    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
    <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
    <LogConfig Driver="[YOUR_LOG_DRIVER]" >
        <DriverOption Name="test" Value="vale"/>
    </LogConfig>
    <Volume Source="c:\workspace" Destination="c:\testmountlocation1" IsReadOnly="false"></Volume>
    <Volume Source="[MyStorageVar]" Destination="c:\testmountlocation2" IsReadOnly="true"> </Volume>
    <Volume Source="myvolume1" Destination="c:\testmountlocation2" Driver="[YOUR_VOLUME_DRIVER]" IsReadOnly="true">
        <DriverOption Name="[name]" Value="[value]"/>
    </Volume>
</ContainerHostPolicies>
```

## <a name="next-steps"></a>Passos Seguintes
* Para ver as amostras de contentor, incluindo o controlador de volume, visite o [amostras de contentor do Service Fabric](https://github.com/Azure-Samples/service-fabric-containers)
* Para implementar um cluster do Service Fabric contentores, consulte o artigo [implementar um contentor no Service Fabric](service-fabric-deploy-container.md)


