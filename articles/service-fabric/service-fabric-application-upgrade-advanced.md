---
title: Avançadas tópicos de atualização de aplicação | Microsoft Docs
description: Este artigo abrange alguns tópicos avançados relativas à atualização de uma aplicação de Service Fabric.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: e29585ff-e96f-46f4-a07f-6682bbe63281
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: 168d944f72c1409b5b69c9ab7c07f7fcfa04c7c8
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="service-fabric-application-upgrade-advanced-topics"></a>Atualização da aplicação de Service Fabric: avançadas tópicos
## <a name="adding-or-removing-service-types-during-an-application-upgrade"></a>Adicionar ou remover tipos de serviço durante uma atualização da aplicação
Se for adicionado um novo tipo de serviço para uma aplicação publicada como parte de uma atualização, é adicionado o novo tipo de serviço para a aplicação implementada. Essa uma atualização não afeta a qualquer uma das instâncias do serviço que já faziam parte da aplicação, mas tem de ser criada uma instância do tipo de serviço que foi adicionado para o novo tipo de serviço ser ativos (consulte [New-ServiceFabricService](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricservice?view=azureservicefabricps)).

Da mesma forma, os tipos de serviço podem ser removidos de uma aplicação como parte de uma atualização. No entanto, todas as instâncias de serviço a to-be-removido do tipo de serviço tem de ser removidas antes de continuar com a atualização (consulte [remover ServiceFabricService](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricservice?view=azureservicefabricps)).

## <a name="manual-upgrade-mode"></a>Modo de atualização manual
> [!NOTE]
> O *monitorizado* modo de atualização é recomendado para todas as atualizações de Service Fabric.
> O *UnmonitoredManual* modo de atualização só deve ser considerado para atualizações de falha ou suspensos. 
>
>

No *monitorizado* modo, o Service Fabric aplica políticas de estado de funcionamento para se certificar de que a aplicação está em bom estada como avança de ser atualização. Se as políticas de estado de funcionamento são violadas, em seguida, a atualização está suspenso ou automaticamente revertida consoante especificado *FailureAction*.

No *UnmonitoredManual* modo, o administrador da aplicação tem controlo total sobre a evolução da atualização. Este modo é útil quando aplicar políticas de avaliação do Estado de funcionamento personalizado ou efetuar atualizações não convencionais para ignorar completamente a monitorização de estado de funcionamento (por exemplo, a aplicação já está a ser perda de dados). Uma atualização executado no modo suspender-se automaticamente após a conclusão de cada UD e tem de ser explicitamente foi retomada com [retomar ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/resume-servicefabricapplicationupgrade?view=azureservicefabricps). Quando uma atualização está suspenso e estiver pronto para ser retomada pelo utilizador, apresentará o estado de atualização *RollforwardPending* (consulte [UpgradeState](https://docs.microsoft.com/dotnet/api/system.fabric.applicationupgradestate?view=azure-dotnet)).

Por fim, o *UnmonitoredAuto* modo é útil para efetuar iterações rápido atualização durante a programação de serviço ou testes, uma vez que não for necessária nenhuma intervenção do utilizador e não existem políticas de estado de funcionamento da aplicação são avaliadas.

## <a name="upgrade-with-a-diff-package"></a>Atualizar com um pacote de diff
Em vez de aprovisionamento de um pacote de aplicação completa, as atualizações também podem ser efetuadas pela aprovisionamento diff pacotes que contenham apenas os pacotes de código/configuração/dados atualizados, juntamente com o manifesto da aplicação concluída e concluir manifestos de serviço. Pacotes de aplicações completos são apenas necessário para a instalação inicial de uma aplicação para o cluster. As atualizações subsequentes podem ser de pacotes de aplicação completa ou verificar as diferenças entre pacotes.  

Qualquer referência no manifesto da aplicação ou manifestos de serviço de um pacote de diferenciação não é possível encontrar o pacote de aplicação é automaticamente substituída com a versão atualmente aprovisionada.

Cenários para utilizar um pacote de diff são:

* Quando tiver um pacote de aplicações de grande que faça referência a vários ficheiros de manifesto do serviço e/ou vários pacotes de código, pacotes de configuração ou pacotes de dados.
* Quando tiver um sistema de implementação que gera o esquema de compilação diretamente a partir da sua aplicação criar processo. Neste caso, apesar do código ainda não tiver sido alterada, assemblagens recentemente incorporadas obter uma soma de verificação diferente. Utilizar um pacote de aplicação completa seria necessários atualizar a versão em todos os pacotes de código. Utilizar um pacote de diferenciação, fornece apenas os ficheiros que foram alterados e os ficheiros de manifesto em que a versão foi alterada.

Quando uma aplicação é atualizada com o Visual Studio, um pacote de diff é publicado automaticamente. Para criar um pacote de diff manualmente, o manifesto da aplicação e os manifestos de serviço tem de ser atualizados, mas apenas os pacotes alterados devem ser incluídos no pacote de aplicação final.

Por exemplo, vamos começar com a seguinte aplicação (números de versão fornecidas para facilitar a compreensão):

```text
app1           1.0.0
  service1     1.0.0
    code       1.0.0
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

Vamos supor que pretendia atualizar apenas o pacote do código de service1 utilizando um pacote de diferenciação. A aplicação atualizada tem as seguintes alterações de versão:

```text
app1           2.0.0      <-- new version
  service1     2.0.0      <-- new version
    code       2.0.0      <-- new version
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

Neste caso, atualize o manifesto da aplicação para 2.0.0 e o manifesto de serviço para service1 refletir a atualização de pacote do código. A pasta do seu pacote de aplicação teria a estrutura seguinte:

```text
app1/
  service1/
    code/
```

Por outras palavras, criar um pacote de aplicação completa normalmente, em seguida, remova quaisquer pastas de pacote do código/configuração/dados para o qual a versão não tenha sido alterado.

## <a name="rolling-back-application-upgrades"></a>A reverter as atualizações de aplicações

Enquanto as atualizações podem ser efetuadas o rollforward em um de três modos (*monitorizado*, *UnmonitoredAuto*, ou *UnmonitoredManual*), podem apenas ser revertidas ou *UnmonitoredAuto* ou *UnmonitoredManual* modo. Graduais regresso *UnmonitoredAuto* modo funciona da mesma forma que a reencaminhar com a exceção que o valor predefinido de *UpgradeReplicaSetCheckTimeout* é diferente - consulte [aplicação Atualizar parâmetros](service-fabric-application-upgrade-parameters.md). Graduais regresso *UnmonitoredManual* modo funciona da mesma forma que a reencaminhar - a reversão suspende-se automaticamente após a conclusão de cada UD e tem de ser explicitamente foi retomada com [ Retomar ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/resume-servicefabricapplicationupgrade?view=azureservicefabricps) para continuar com a reversão.

Reverte pode ser acionada automaticamente quando as políticas de estado de funcionamento de uma atualização na *monitorizado* modo com um *FailureAction* de *reversão* são violou (consulte [Aplicação atualizar parâmetros](service-fabric-application-upgrade-parameters.md)) ou explicitamente utilizando [início ServiceFabricApplicationRollback](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationrollback?view=azureservicefabricps).

Durante a reversão, o valor de *UpgradeReplicaSetCheckTimeout* e o modo ainda pode ser alterado em qualquer altura utilizando [atualização ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/update-servicefabricapplicationupgrade?view=azureservicefabricps).

## <a name="next-steps"></a>Passos Seguintes
[Atualizar a aplicação utilizando o Visual Studio](service-fabric-application-upgrade-tutorial.md) orienta-o através de uma atualização da aplicação com o Visual Studio.

[Atualizar a sua aplicação através do Powershell](service-fabric-application-upgrade-tutorial-powershell.md) orienta-o através de uma atualização da aplicação através do PowerShell.

Controlar a forma como a aplicação atualiza utilizando [atualizar parâmetros](service-fabric-application-upgrade-parameters.md).

Faça as atualizações de aplicações compatíveis aprender a utilizar [dados serialização](service-fabric-application-upgrade-data-serialization.md).

Resolva problemas comuns nas atualizações de aplicações ao referir-se os passos descritos para [resolução de problemas de atualizações de aplicação](service-fabric-application-upgrade-troubleshooting.md).
