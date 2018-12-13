---
title: Início rápido - criar um registo privado do Docker no Azure - PowerShell
description: Aprenda rapidamente a criar um registo de contentor do Docker no Azure com o PowerShell.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: quickstart
ms.date: 05/08/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: 7dcdca594949a3b20000b31db681370ee4f9eac3
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2018
ms.locfileid: "53255290"
---
# <a name="quickstart-create-a-private-container-registry-using-azure-powershell"></a>Início rápido: Criar um registo de contentor privado com o Azure PowerShell

O Azure Container Registry é um serviço de registo de contentor do Docker privado e gerido que serve para criar, armazenar e fornecer imagens de contentor do Docker. Neste guia de início rápido, irá aprender a criar um registo de contentor do Azure com o PowerShell. Depois de criar o registo, envie uma imagem de contentor para o mesmo e, em seguida, implemente o contentor a partir do seu registo no Azure Container Instances (ACI).

## <a name="prerequisites"></a>Pré-requisitos

Este guia de início rápido requer a versão 5.7.0 ou posterior do módulo Azure PowerShell. Execute `Get-Module -ListAvailable AzureRM` para determinar a versão instalada. Se precisar de instalar ou atualizar, veja [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps)(Instalar o módulo do Azure PowerShell).

Também tem de ter o Docker instalado localmente. O Docker fornece pacotes para os sistemas [macOS][docker-mac], [Windows][docker-windows] e [Linux][docker-linux].

Uma vez que o Azure Cloud Shell não inclui todos os componentes do Docker necessários (o daemon `dockerd`), não é possível utilizar o Cloud Shell para este guia de início rápido.

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão na sua subscrição do Azure com o comando [Connect-AzureRmAccount][Connect-AzureRmAccount] e siga as instruções no ecrã.

```powershell
Connect-AzureRmAccount
```

## <a name="create-resource-group"></a>Criar grupo de recursos

Depois de efetuar a autenticação no Azure, crie um grupo de recursos com [New-AzureRmResourceGroup][New-AzureRmResourceGroup]. Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos.

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-container-registry"></a>Criar registo de contentor

Em seguida, crie um registo de contentor no novo grupo de recursos com o comando [New-AzureRMContainerRegistry][New-AzureRMContainerRegistry].

O nome do registo tem de ser exclusivo no Azure e pode incluir de 5 a 50 carateres alfanuméricos. O exemplo seguinte cria um registo com o nome "myContainerRegistry007". Substitua *myContainerRegistry007* no comando seguinte e execute-o para criar o registo:

```powershell
$registry = New-AzureRMContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

## <a name="log-in-to-registry"></a>Iniciar sessão no registo

Antes de enviar e solicitar imagens de contentor, tem de iniciar sessão no registo. Utilize o comando [Get-AzureRmContainerRegistryCredential][Get-AzureRmContainerRegistryCredential] para obter primeiro as credenciais de administrador do registo:

```powershell
$creds = Get-AzureRmContainerRegistryCredential -Registry $registry
```

Em seguida, execute o [docker login][docker-login] para iniciar sessão:

```powershell
$creds.Password | docker login $registry.LoginServer -u $creds.Username --password-stdin
```

Um início de sessão bem-sucedido devolve `Login Succeeded`:

```console
PS Azure:\> $creds.Password | docker login $registry.LoginServer -u $creds.Username --password-stdin
Login Succeeded
```

## <a name="push-image-to-registry"></a>Enviar imagem para o registo

Agora que iniciou sessão no registo, pode enviar imagens de contentor para o mesmo. Para obter uma imagem que possa enviar para o registo, solicite a imagem [aci-helloworld][aci-helloworld-image] pública a partir do Hub do Docker. A imagem [aci-helloworld][aci-helloworld-github] é uma pequena aplicação Node.js que fornece uma página HTML estática que apresenta o logótipo do Azure Container Instances.

```powershell
docker pull microsoft/aci-helloworld
```

Quando a imagem for solicitada, o resultado é semelhante ao seguinte:

```console
PS Azure:\> docker pull microsoft/aci-helloworld
Using default tag: latest
latest: Pulling from microsoft/aci-helloworld
88286f41530e: Pull complete
84f3a4bf8410: Pull complete
d0d9b2214720: Pull complete
3be0618266da: Pull complete
9e232827e52f: Pull complete
b53c152f538f: Pull complete
Digest: sha256:a3b2eb140e6881ca2c4df4d9c97bedda7468a5c17240d7c5d30a32850a2bc573
Status: Downloaded newer image for microsoft/aci-helloworld:latest
```

Antes de enviar uma imagem para o registo de contentor do Azure, tem de a etiquetar com o nome de domínio completamente qualificado (FQDN) do registo. Os FQDNs dos registos de contentor do Azure estão no formato *\<registry-name\>.azurecr.io*.

Preencha uma variável com a etiqueta da imagem completa. Inclua o servidor de início de sessão, o nome do repositório ("aci-helloworld") e a versão ("v1") da imagem:

```powershell
$image = $registry.LoginServer + "/aci-helloworld:v1"
```

Em seguida, etiquete a imagem com [docker tag][docker-tag]:

```powershell
docker tag microsoft/aci-helloworld $image
```

Por fim, aplique o [docker push][docker-push] para enviar a imagem para o registo:

```powershell
docker push $image
```

Quando o cliente do Docker enviar a sua imagem, o resultado deverá ser semelhante ao seguinte:

```console
PS Azure:\> docker push $image
The push refers to repository [myContainerRegistry007.azurecr.io/aci-helloworld]
31ba1ebd9cf5: Pushed
cd07853fe8be: Pushed
73f25249687f: Pushed
d8fbd47558a8: Pushed
44ab46125c35: Pushed
5bef08742407: Pushed
v1: digest: sha256:565dba8ce20ca1a311c2d9485089d7ddc935dd50140510050345a1b0ea4ffa6e size: 1576
```

Parabéns! Acabou de enviar a sua primeira imagem de contentor para o seu registo.

## <a name="deploy-image-to-aci"></a>Implementar a imagem no ACI

Com a imagem agora no seu registo, implemente um contentor diretamente no Azure Container Instances para que seja executado no Azure.

Em primeiro lugar, converta a credencial do registo numa *PSCredential*. O comando `New-AzureRmContainerGroup`, que utiliza para criar a instância de contentor, exige este formato.

```powershell
$secpasswd = ConvertTo-SecureString $creds.Password -AsPlainText -Force
$pscred = New-Object System.Management.Automation.PSCredential($creds.Username, $secpasswd)
```

Além disso, a etiqueta de nome DNS do contentor tem de ser exclusiva na região do Azure em que a criar. Execute o seguinte comando para preencher uma variável com um nome gerado:

```powershell
$dnsname = "aci-demo-" + (Get-Random -Maximum 9999)
```

Por fim, execute [New-AzureRmContainerGroup][New-AzureRmContainerGroup] para implementar um contentor a partir da imagem no seu registo, com 1 núcleo de CPU e 1 GB de memória:

```powershell
New-AzureRmContainerGroup -ResourceGroup myResourceGroup -Name "mycontainer" -Image $image -RegistryCredential $pscred -Cpu 1 -MemoryInGB 1 -DnsNameLabel $dnsname
```

Deverá obter uma resposta inicial do Azure com os detalhes sobre o contentor e, inicialmente, o estado do mesmo é "Pendente":

```console
PS Azure:\> New-AzureRmContainerGroup -ResourceGroup myResourceGroup -Name "mycontainer" -Image $image -RegistryCredential $pscred -Cpu 1 -MemoryInGB 1 -DnsNameLabel $dnsname
ResourceGroupName        : myResourceGroup
Id                       : /subscriptions/<subscriptionID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerInstance/containerGroups/mycontainer
Name                     : mycontainer
Type                     : Microsoft.ContainerInstance/containerGroups
Location                 : eastus
Tags                     :
ProvisioningState        : Creating
Containers               : {mycontainer}
ImageRegistryCredentials : {myContainerRegistry007}
RestartPolicy            : Always
IpAddress                : 40.117.255.198
DnsNameLabel             : aci-demo-8751
Fqdn                     : aci-demo-8751.eastus.azurecontainer.io
Ports                    : {80}
OsType                   : Linux
Volumes                  :
State                    : Pending
Events                   : {}
```

Para monitorizar o estado do contentor e determinar quando está a ser executado, execute o comando [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup] algumas vezes. O contentor deverá demorar menos de um minuto a iniciar.

```powershell
(Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).ProvisioningState
```

Aqui, pode ver que, inicialmente, o ProvisioningState do contentor é *A Criar* e que o estado passa para *Com Êxito* quando o contentor estiver a funcionar:

```console
PS Azure:\> (Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).ProvisioningState
Creating
PS Azure:\> (Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).ProvisioningState
Succeeded
```

## <a name="view-running-application"></a>Ver a aplicação em execução

Quando a implementação for concluída com êxito no ACI e o seu contentor estiver a funcionar, navegue para o nome de domínio completamente qualificado (FQDN) do contentor no seu browser para ver a aplicação em execução no Azure.

Obtenha o FQDN do contentor ao executar o comando [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup]:


```powershell
(Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).Fqdn
```

O resultado do comando é o FQDN da instância do seu contentor:

```console
PS Azure:\> (Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).Fqdn
aci-demo-8571.eastus.azurecontainer.io
```

Depois de obter o FQDN, navegue para o mesmo no seu browser:

![Aplicação Hello World no browser][qs-psh-01-running-app]

Parabéns! Tem um contentor que está a executar uma aplicação no Azure, com implementação direta a partir de uma imagem de contentor no seu registo de contentor do Azure privado.

## <a name="clean-up-resources"></a>Limpar recursos

Depois de concluir as tarefas com os recursos que criou neste guia de início rápido, utilize o comando [Remove-AzureRmResourceGroup][Remove-AzureRmResourceGroup] para remover o grupo de recursos, o registo de contentor e a instância de contentor:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Passos Seguintes

Neste início rápido, criou um Azure Container Registry com a CLI do Azure e iniciou uma instância do mesmo no Azure Container Instances. Avance para o tutorial do Azure Container Instances para ver mais detalhadamente o ACI.

> [!div class="nextstepaction"]
> [Tutorial do Azure Container Instances](../container-instances/container-instances-tutorial-prepare-app.md)

<!-- LINKS - external -->
[aci-helloworld-github]: https://github.com/Azure-Samples/aci-helloworld
[aci-helloworld-image]: https://hub.docker.com/r/microsoft/aci-helloworld/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- Links - internal -->
[Connect-AzureRmAccount]: /powershell/module/azurerm.profile/connect-azurermaccount
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Get-AzureRmContainerRegistryCredential]: /powershell/module/azurerm.containerregistry/get-azurermcontainerregistrycredential
[Get-Module]: /powershell/module/microsoft.powershell.core/get-module
[New-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[New-AzureRMContainerRegistry]: /powershell/module/azurerm.containerregistry/New-AzureRMContainerRegistry
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[Remove-AzureRmResourceGroup]: /powershell/module/azurerm.resources/remove-azurermresourcegroup

<!-- IMAGES> -->
[qs-psh-01-running-app]: ./media/container-registry-get-started-powershell/qs-psh-01-running-app.png
