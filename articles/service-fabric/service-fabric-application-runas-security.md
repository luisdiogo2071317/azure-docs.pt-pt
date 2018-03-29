---
title: Executar um serviço de Azure Service Fabric em sistema e as contas de segurança local | Microsoft Docs
description: Saiba como executar uma aplicação de Service Fabric em sistema e as contas de segurança local.  Crie princípios de segurança e aplicar a política de executar como para executar em segurança os serviços.
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
ms.openlocfilehash: 62917a1d342158ec2114a9204ee1ca9e447284fa
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="run-a-service-as-a-local-user-account-or-local-system-account"></a>Executar um serviço como uma conta de utilizador local ou como conta de sistema local
Ao utilizar o Service Fabric do Azure, pode proteger aplicações em execução no cluster em contas de utilizador diferente. Por predefinição, as aplicações de Service Fabric executam sob a conta que executa o processo de Fabric.exe em. Serviço de recursos de infraestrutura também fornece a capacidade para executar aplicações com uma conta de utilizador local ou a conta de sistema local, o que é feita ao especificar uma política de RunAs no manifesto da aplicação. Tipos de conta de sistema local suportados são **Utilizador_local**, **NetworkService**, **LocalService**, e **LocalSystem**.

Também pode definir e criar grupos de utilizadores que podem ser adicionados um ou mais utilizadores para cada grupo de ser geridos em conjunto. Isto é útil quando existem vários utilizadores para pontos de entrada de serviço diferente e que precisam para tem determinados privilégios comuns que estão disponíveis ao nível do grupo.

> [!NOTE] 
> Se aplicar uma política de RunAs para um serviço e o manifesto do serviço declara recursos de ponto final com o protocolo HTTP, tem de especificar um **SecurityAccessPolicy**.  Para obter mais informações, consulte [atribuir uma política de acesso de segurança para pontos finais HTTP e HTTPS](service-fabric-assign-policy-to-endpoint.md). 
>

## <a name="create-local-user-groups"></a>Criar grupos de utilizadores locais
Pode definir e criar grupos de utilizadores que permitem aos utilizadores de um ou mais ser adicionado a um grupo. Isto é útil se existirem vários utilizadores para pontos de entrada de serviço diferente e que precisam para tem determinados privilégios comuns que estão disponíveis ao nível do grupo. O exemplo seguinte mostra um grupo local chamado **LocalAdminGroup** que tem privilégios de administrador. Dois utilizadores, Customer1 e Customer2, são efetuados os membros deste grupo local neste exemplo de manifesto de aplicação:

```xml
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
```

## <a name="create-local-users"></a>Criar utilizadores locais
Pode criar um utilizador local que pode ser utilizado para ajudar a proteger um serviço na aplicação. Quando um **Utilizador_local** tipo de conta é especificada na secção de principais de manifesto da aplicação, Service Fabric cria contas de utilizador local em máquinas em que a aplicação é implementada. Por predefinição, estas contas não têm os mesmos nomes que as especificadas no manifesto da aplicação (por exemplo, Customer3 no seguinte exemplo de manifesto de aplicação). Em vez disso, são geradas dinamicamente e ter palavras-passe aleatórias.

```xml
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
```

Se uma aplicação requer que a conta de utilizador e palavra-passe ser a mesma em todas as máquinas (por exemplo, para ativar a autenticação NTLM), o manifesto do cluster tem de definir **NTLMAuthenticationEnabled** como true. O manifesto do cluster tem de especificar também um **NTLMAuthenticationPasswordSecret** que é utilizado para gerar a mesma palavra-passe em todas as máquinas.

```xml
<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
```

## <a name="assign-policies-to-the-service-code-packages"></a>Atribuir políticas para os pacotes de código do serviço
O **RunAsPolicy** secção para um **ServiceManifestImport** Especifica a conta da secção principais que deve ser utilizada para executar um pacote do código. Também associa os pacotes de código do manifesto de serviço com contas de utilizador na secção principais. Pode especificar este para o programa de configuração ou pontos de entrada principal, ou pode especificar `All` para aplicá-la para ambos. O exemplo seguinte mostra as políticas diferentes a serem aplicadas:

```xml
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
```

Se **EntryPointType** não for especificado, a predefinição está definida como `EntryPointType=”Main”`. Especificar **SetupEntryPoint** é especialmente útil quando pretender executar determinadas operações de configuração de privilégio elevado com uma conta do sistema. Para obter mais informações, consulte [executar um script de arranque do serviço como uma conta de utilizador ou sistema local](service-fabric-run-script-at-service-startup.md). O código do serviço real pode ser executados com uma conta de menor privilégio.

## <a name="apply-a-default-policy-to-all-service-code-packages"></a>Aplicar uma política predefinida para todos os pacotes de código do serviço
Utilizar o **DefaultRunAsPolicy** secção para especificar uma conta de utilizador predefinido para todo o código de pacotes que não tem um específico **RunAsPolicy** definido. Se a maioria dos pacotes de código que são especificados no manifesto de serviço utilizado por uma aplicação precisa de ser executado sob o mesmo utilizador, a aplicação apenas pode definir uma política de RunAs predefinida com essa conta de utilizador. O exemplo seguinte especifica que, se um pacote do código não tem um **RunAsPolicy** especificado, o pacote do código deve ser executado o **MyDefaultAccount** especificados na secção principais.

```xml
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
```

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Passos Seguintes
* [Compreender o modelo de aplicação](service-fabric-application-model.md)
* [Especificar recursos num manifesto de serviço](service-fabric-service-manifest-resources.md)
* [Implementar uma aplicação](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
