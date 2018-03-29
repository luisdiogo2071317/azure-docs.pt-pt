---
title: Configurar a gMSA para serviços de contentor do Azure Service Fabric | Microsoft Docs
description: Saiba como configurar a gMSA para um contentor em execução no Azure Service Fabric.
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
ms.date: 03/23/2018
ms.author: subramar
ms.openlocfilehash: 862716f7bffb2dd0ab962efacc38777c981463b2
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="set-up-gmsa-for-windows-containers-running-on-service-fabric"></a>Configurar a gMSA para contentores de Windows em execução no Service Fabric

Para configurar a gMSA (grupo de contas de serviço gerida), um ficheiro de especificação de credenciais (`credspec`) é colocada em todos os nós do cluster. O ficheiro pode ser copiado em todos os nós a utilizar uma extensão VM.  O `credspec` ficheiro tem de conter as informações de conta gMSA. Para mais informações sobre o `credspec` de ficheiros, consulte [contas de serviço](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts). A especificação de credenciais e o `Hostname` etiquetas são especificado no manifesto da aplicação. O `Hostname` etiqueta tem de corresponder ao nome da conta de gMSA o contentor é executado.  O `Hostname` etiquetas permite que o contentor para se autenticar perante outros serviços no domínio utilizando a autenticação Kerberos.  Um exemplo sobre como especificar o `Hostname` e `credspec` na aplicação do manifesto é mostrado no seguinte fragmento:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
Como passo seguinte, leia os artigos seguintes:

* [Implementar um contentor do Windows para o Service Fabric no Windows Server 2016](service-fabric-get-started-containers.md)
* [Implementar um contentor de Docker Service Fabric no Linux](service-fabric-get-started-containers-linux.md)
