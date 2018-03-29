---
title: Executar um serviço do Service Fabric do Azure como um utilizador do AD ou o grupo | Microsoft Docs
description: Saiba como executar um serviço como um grupo ou utilizador do Active Directory num cluster autónomo períodos de recursos de infraestrutura.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/21/2018
ms.author: mfussell
ms.openlocfilehash: 1cf23a8f564553e65ac2c0fd34d44d81fe2327ea
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="run-a-service-as-an-active-directory-user-or-group"></a>Executar um serviço como um grupo ou utilizador do Active Directory
Ao utilizar o Service Fabric do Azure, pode proteger aplicações em execução no cluster em contas de utilizador diferente. Isto faz com que aplicações em execução, mesmo num ambiente alojado partilhado, mais segura entre si. Por predefinição, as aplicações de Service Fabric executam sob a conta que executa o processo de Fabric.exe em. Para um cluster de autónomo do Windows Server, pode executar um serviço como um [grupo conta de serviço gerida (gMSA)](service-fabric-run-service-as-gmsa.md) ou um utilizador do Active Directory ou o grupo através de uma política de RunAs. Tenha em atenção que esta utiliza o Active Directory no local dentro do seu domínio e não do Azure Active Directory (Azure AD).

Através da utilização de um utilizador de domínio ou grupo, em seguida, pode aceder a outros recursos no domínio (por exemplo, as partilhas de ficheiros) tem sido concedidos permissões.

O exemplo seguinte mostra um utilizador do Active Directory chamado *TestUser* com o seu domínio, denominada palavra-passe encriptado utilizando um certificado *MyCert*. Pode utilizar o `Invoke-ServiceFabricEncryptText` comando do PowerShell para criar o texto de cifra secreta. Consulte [gerir segredos em aplicações de Service Fabric](service-fabric-application-secret-management.md) para obter mais detalhes.

Tem de implementar a chave privada do certificado para desencriptar a palavra-passe para o computador local utilizando um método fora de banda (no Azure, este é através do Azure Resource Manager). Em seguida, quando o Service Fabric implementa o pacote de serviço para a máquina, é capaz de desencriptar o segredo e (juntamente com o nome de utilizador) autenticar com o Active Directory para ser executado sob essas credenciais.

```xml
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
```

> [!NOTE] 
> Se aplicar uma política de RunAs para um serviço e o manifesto do serviço declara recursos de ponto final com o protocolo HTTP, também tem de especificar um **SecurityAccessPolicy**.  Para obter mais informações, consulte [atribuir uma política de acesso de segurança para pontos finais HTTP e HTTPS](service-fabric-assign-policy-to-endpoint.md). 
>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Como passo seguinte, leia os artigos seguintes:
* [Compreender o modelo de aplicação](service-fabric-application-model.md)
* [Especificar recursos num manifesto de serviço](service-fabric-service-manifest-resources.md)
* [Implementar uma aplicação](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
