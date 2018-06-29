---
title: Resolução de problemas com o Automation DSC do Azure pretendido Estado Configuration)
description: Este artigo fornece informações sobre resolução de problemas de configuração de estado pretendido (DSC)
services: automation
ms.service: automation
ms.component: ''
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 5d2eae67fcff74a7016f7f6125e31a9c8c2bda97
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064124"
---
# <a name="troubleshoot-desired-state-configuration-dsc"></a>Resolver problemas de configuração do estado pretendido (DSC)

Este artigo fornece informações sobre como resolver problemas com configuração de estado pretendido (DSC).

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>Erros comuns ao trabalhar com configuração de estado pretendido (DSC)

### <a name="failed-not-found"></a>Cenário: Nó está num Estado de falha com um erro "Não encontrada"

#### <a name="issue"></a>Problema

O nó tem um relatório com **falha** estado e de que contém o erro:

```
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

#### <a name="cause"></a>Causa

Este erro normalmente ocorre quando o nó está atribuído a um nome de configuração (por exemplo, ABC) em vez de um nome de configuração do nó (por exemplo, ABC. Servidor Web).

#### <a name="resolution"></a>Resolução

* Certifique-se de que está a atribuir o nó com o "nome do nó de configuração" e não o "nome de configuração".
* Pode atribuir uma configuração de nó a um nó através do portal do Azure ou com um cmdlet do PowerShell.

  * Para atribuir uma configuração de nó a um nó através do portal do Azure, abra o **nós de DSC** página, em seguida, selecione um nó e clique em **atribuir a configuração do nó** botão.  
  * Para atribuir uma configuração de nó a um nó através do cmdlet do PowerShell, utilize **conjunto AzureRmAutomationDscNode** cmdlet

### <a name="no-mof-files"></a>Cenário: Não existem configurações de nó (ficheiros MOF) foram produzidas quando uma configuração é compilada

#### <a name="issue"></a>Problema

Suspende a tarefa de compilação de DSC com o erro:

```
Compilation completed successfully, but no node configuration.mofs were generated.
```

#### <a name="cause"></a>Causa

Quando as seguintes expressão o **nó** palavra-chave na configuração de DSC avalia como `$null`, e não configurações de nó são produzidas.

#### <a name="resolution"></a>Resolução

Qualquer uma das seguintes soluções corrigir o problema:

* Certifique-se de que a expressão junto ao **nó** palavra-chave na definição de configuração não está a avaliar para $null.
* Se está a transmitir ConfigurationData ao compilar a configuração, certifique-se de que está a transmitir os valores esperados, que requer a configuração do [ConfigurationData](../automation-dsc-compile.md#configurationdata).

### <a name="dsc-in-progress"></a>Cenário: O relatório de nó DSC fica presa "em curso" Estado

#### <a name="issue"></a>Problema

O agente de DSC produz:

```
No instance found with given property values
```

#### <a name="cause"></a>Causa

O utilizador atualizou a sua versão do WMF e ter danificado WMI.

#### <a name="resolution"></a>Resolução

Para corrigir a problema siga as instruções de [DSC problemas e limitações conhecidos](https://msdn.microsoft.com/powershell/wmf/5.0/limitation_dsc) artigo.

### <a name="issue-using-credential"></a>Cenário: Não é possível utilizar uma credencial numa configuração de DSC

#### <a name="issue"></a>Problema

A tarefa de compilação de DSC foi suspenso com o erro:

```
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

#### <a name="cause"></a>Causa

Utilizou uma credencial numa configuração mas não fornecer adequada **ConfigurationData** definir **PSDscAllowPlainTextPassword** como true para cada configuração de nó.

#### <a name="resolution"></a>Resolução

* Certifique-se passar o adequado **ConfigurationData** definir **PSDscAllowPlainTextPassword** como true para cada configuração de nó mencionada na configuração. Para obter mais informações, consulte [ativos no Automation DSC do Azure](../automation-dsc-compile.md#assets).

## <a name="next-steps"></a>Passos Seguintes

Se não foi possível ver o seu problema ou não conseguir resolver o problema, visite uma das seguintes canais de suporte mais:

* Obtenha respostas de especialistas do Azure através dos [fóruns do Azure](https://azure.microsoft.com/support/forums/)
* Ligue-se a [@AzureSupport](https://twitter.com/azuresupport) – a conta oficial do Microsoft Azure para melhorar a experiência do cliente ao ligar a comunidade do Azure aos recursos certos: respostas, suporte e especialistas.
* Se precisar de mais ajuda, pode ficheiro um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione **obter suporta**.