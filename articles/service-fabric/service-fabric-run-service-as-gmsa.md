---
title: Executar um serviço de Azure Service Fabric sob uma conta gMSA | Microsoft Docs
description: Saiba como executar um serviço como um gMSA num cluster autónomo períodos de recursos de infraestrutura.
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
ms.date: 03/29/2018
ms.author: mfussell
ms.openlocfilehash: 5f93285061708172b9b6ac40dc97fce08f7b2a86
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34206718"
---
# <a name="run-a-service-as-a-group-managed-service-account"></a>Executar um serviço como uma Conta de Serviço Gerida de grupo
Num cluster do Windows Server autónomo, pode executar um serviço como um grupo de conta de serviço gerida (gMSA) através de uma política de RunAs.  Por predefinição, as aplicações de Service Fabric executam sob a conta que executa o processo de Fabric.exe em. A execução de aplicações em diferentes contas, mesmo num ambiente alojado partilhado, torna-os mais segura entre si. Tenha em atenção que esta utiliza o Active Directory no local dentro do seu domínio e não do Azure Active Directory (Azure AD). Ao utilizar uma gMSA, não há nenhuma palavra-passe ou a palavra-passe encriptada armazenados no manifesto da aplicação.  Também pode executar um serviço como um [grupo ou utilizador do Active Directory](service-fabric-run-service-as-ad-user-or-group.md).

O exemplo seguinte mostra como criar uma conta de gMSA denominada *svc teste$*; como implementar essa conta de serviço geridas para os nós de cluster e como configurar o principal de utilizador.

Pré-requisitos:
- O domínio tem uma chave de raiz KDS.
- O domínio tem de estar num Windows Server 2012 ou posterior nível funcional.

1. Solicite a um administrador de domínio do Active Directory, criar um geridas de grupo conta de serviço utilizando o `New-ADServiceAccount` commandlet e certifique-se de que o `PrincipalsAllowedToRetrieveManagedPassword` inclui todos os nós de cluster de recursos de infraestrutura de serviço. `AccountName`, `DnsHostName`, e `ServicePrincipalName` tem de ser exclusivo.

    ```poweshell
    New-ADServiceAccount -name svc-Test$ -DnsHostName svc-test.contoso.com  -ServicePrincipalNames http/svc-test.contoso.com -PrincipalsAllowedToRetrieveManagedPassword SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$
    ```

2. Em cada um dos recursos de infraestrutura do serviço de nós de cluster (por exemplo, `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`), instalar e testar a gMSA.
    
    ```powershell
    Add-WindowsFeature RSAT-AD-PowerShell
    Install-AdServiceAccount svc-Test$
    Test-AdServiceAccount svc-Test$
    ```

3. Configure o principal de utilizador e a RunAsPolicy para fazer referência ao utilizador.
    
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <ServiceManifestImport>
          <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
          <ConfigOverrides />
          <Policies>
              <RunAsPolicy CodePackageRef="Code" UserRef="DomaingMSA"/>
          </Policies>
        </ServiceManifestImport>
      <Principals>
        <Users>
          <User Name="DomaingMSA" AccountType="ManagedServiceAccount" AccountName="domain\svc-Test$"/>
        </Users>
      </Principals>
    </ApplicationManifest>
    ```

> [!NOTE] 
> Se aplicar uma política de RunAs para um serviço e o manifesto do serviço declara recursos de ponto final com o protocolo HTTP, tem de especificar um **SecurityAccessPolicy**.  Para obter mais informações, consulte [atribuir uma política de acesso de segurança para pontos finais HTTP e HTTPS](service-fabric-assign-policy-to-endpoint.md). 
>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Como passo seguinte, leia os artigos seguintes:
* [Compreender o modelo de aplicação](service-fabric-application-model.md)
* [Especificar recursos num manifesto de serviço](service-fabric-service-manifest-resources.md)
* [Implementar uma aplicação](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
