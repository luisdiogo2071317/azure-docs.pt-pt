---
title: Atribuir políticas de acesso a pontos finais de serviço do Service Fabric do Azure | Microsoft Docs
description: Saiba como atribuir segurança políticas de acesso para pontos finais HTTP ou HTTPS no seu serviço do Service Fabric.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/21/2018
ms.author: mfussell
ms.openlocfilehash: f9de8d213d11a8ccb3ffff484a67560d9e2abe77
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a>Atribuir uma política de acesso de segurança para pontos finais HTTP e HTTPS
Se aplicar uma política de executar como e o manifesto do serviço declara recursos de ponto final HTTP, tem de especificar um **SecurityAccessPolicy**.  **SecurityAccessPolicy** assegura que as portas atribuídas com estes pontos finais corretamente estão limitadas à conta de utilizador que o serviço é executado como. Caso contrário, **http.sys** não tem acesso ao serviço e obter falhas com as chamadas do cliente. O exemplo seguinte aplica-se a conta de Customer1 para um ponto final chamado **EndpointName**, que fornece direitos de acesso total.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

Para um ponto final HTTPS, também indica o nome do certificado para devolver ao cliente. Referência a utilizar o certificado **EndpointBindingPolicy**.  O certificado está definido no **certificados** secção de manifesto da aplicação.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
```

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Para os passos seguintes, leia os artigos seguintes:
* [Compreender o modelo de aplicação](service-fabric-application-model.md)
* [Especificar recursos num manifesto de serviço](service-fabric-service-manifest-resources.md)
* [Implementar uma aplicação](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
