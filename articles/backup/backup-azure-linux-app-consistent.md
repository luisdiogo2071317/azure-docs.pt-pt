---
title: "Cópia de segurança do Azure: consistentes com aplicações as cópias de segurança de VMs do Linux | Microsoft Docs"
description: "Crie cópias de segurança consistentes com aplicações das máquinas virtuais Linux no Azure. Este artigo explica a configurar o framework de script para efetuar cópias de segurança do Azure implementadas VMs do Linux. Este artigo também inclui informações de resolução de problemas."
services: backup
documentationcenter: dev-center-name
author: anuragmehrotra
manager: shivamg
keywords: "cópia de segurança consistentes da aplicação; cópia de segurança de VM do Azure com consistência de aplicações; Cópia de segurança de VM com Linux; Cópia de segurança do Azure"
ms.assetid: bbb99cf2-d8c7-4b3d-8b29-eadc0fed3bef
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 1/12/2018
ms.author: anuragm;markgal
ms.openlocfilehash: c2437b4cd90deda3e7239d87837a47a072f52835
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/13/2018
---
# <a name="application-consistent-backup-of-azure-linux-vms"></a>Cópia de segurança consistentes com aplicações das VMs do Linux do Azure

Quando criar instantâneos de cópia de segurança das suas VMs, consistência de aplicação significa que as suas aplicações iniciar quando as VMs de arranque depois de a ser restaurado. Como pode imagine, consistência de aplicação é extremamente importante. Para garantir que as VMs do Linux são consistente, que pode utilizar a estrutura de pré- script de e script pós-implementação do Linux para fazer cópias de segurança consistentes com aplicações da aplicação. A estrutura de pré- script de e script pós-implementação suporta máquinas virtuais do Linux implementadas no Azure Resource Manager. Scripts de consistência de aplicações não suportam máquinas de virtuais implementadas no Service Manager ou máquinas virtuais do Windows.

## <a name="how-the-framework-works"></a>Como funciona o framework

A estrutura fornece uma opção para executar pré-scripts de personalizados e scripts pós-cópia enquanto estiver a ser instantâneos VM. Executar pré-scripts de o antes de tirar o instantâneo VM e scripts pós-implementação executadas imediatamente depois de tirar o instantâneo VM. Scripts de pré e pós-implementação scripts fornecem a flexibilidade de controlar a sua aplicação e o ambiente, enquanto estiver a ser instantâneos VM.

Pré-scripts de invocam APIs, que consegue silenciar de aplicação nativa do IOs e descarregar conteúdo dentro da memória para o disco. Estas ações Certifique-se o instantâneo consistente da aplicação. Scripts pós-implementação utilizarem APIs de aplicação nativa para thaw IOs, o que permite que a aplicação retomar as operações normais após o instantâneo VM.

## <a name="steps-to-configure-pre-script-and-post-script"></a>Passos para configurar o pré- script de e scripts pós-implementação

1. Inicie sessão como utilizador raiz para a VM com Linux que pretende criar cópias de segurança.

2. De [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig), transferir **VMSnapshotScriptPluginConfig.json** e copie-o para o **etc/azure** pasta para todas as VMs que pretende criar cópias de segurança. Se o **etc/azure** pasta não existe, criá-la.

3. Copie o pré- script de e scripts pós-implementação para a sua aplicação em todas as VMs que pretende criar cópias de segurança. Pode copiar os scripts para qualquer local na VM. Não se esqueça de atualizar o caminho completo dos ficheiros de script no **VMSnapshotScriptPluginConfig.json** ficheiro.

4. Certifique-se as seguintes permissões para estes ficheiros:

   - **VMSnapshotScriptPluginConfig.json**: permissão "600." Por exemplo, apenas os utilizadores "raiz" devem ter permissões "leitura" e "escrita" para este ficheiro e nenhum utilizador deve ter permissões de "executar".

   - **Ficheiro de pré-script de**: permissão "700."  Por exemplo, o utilizador de "raiz" só deve ter "ler", "escrita" e "executar" permissões para este ficheiro.
  
   - **Script de pós-implementação** permissão "700." Por exemplo, o utilizador de "raiz" só deve ter "ler", "escrita" e "executar" permissões para este ficheiro.

   > [!Important]
   > A estrutura fornece aos utilizadores uma grande quantidade de energia. Proteja o framework e certifique-se apenas a "raiz" utilizador tem acesso para JSON crítico e os ficheiros de script.
   > Se não são cumpridos os requisitos, o script não são executadas, resultando numa falha de sistema de ficheiro e cópia de segurança inconsistente.
   >

5. Configurar **VMSnapshotScriptPluginConfig.json** conforme descrito aqui:
    - **pluginName**: deixe este campo como está, ou os scripts poderão não funcionar conforme esperado.

    - **preScriptLocation**: forneça o caminho completo do pré-script na VM que vai fazer cópias de segurança.

    - **postScriptLocation**: forneça o caminho completo do script de pós-implementação na VM que vai fazer cópias de segurança.

    - **preScriptParams**: forneça os parâmetros opcionais que precisam de ser transmitidas ao pré script de. Devem ter todos os parâmetros entre aspas. Se utilizar vários parâmetros, separe os parâmetros com uma vírgula.

    - **postScriptParams**: forneça os parâmetros opcionais que precisam de ser transmitidas ao script de pós-implementação. Devem ter todos os parâmetros entre aspas. Se utilizar vários parâmetros, separe os parâmetros com uma vírgula.

    - **preScriptNoOfRetries**: definir o número de vezes que o pré-script de deve ser repetido se não houver qualquer erro antes de terminar. Experimente de apenas um zero significa e sem repetição se ocorrer uma falha.

    - **postScriptNoOfRetries**: definir o número de vezes que o script de pós-implementação deve ser repetido se não houver qualquer erro antes de terminar. Experimente de apenas um zero significa e sem repetição se ocorrer uma falha.
    
    - **timeoutInSeconds**: especificar os tempos limite individuais para o pré- script de e o script de pós-implementação.

    - **continueBackupOnFailure**: defina este valor como **verdadeiro** se pretende que o Backup do Azure para reverter para uma cópia de segurança ficheiros do consistente/falhas consistentes do sistema se o pré- script de ou pós-script falha. Definir este como **falso** falhar a cópia de segurança em caso de falha de script (exceto se tiver de VM de disco único que retrocede para cópia de segurança consistentes com falhas independentemente desta definição).

    - **fsFreezeEnabled**: Especifique se Linux fsfreeze deve ser chamado enquanto estiver a criar o instantâneo de VM para garantir a consistência do sistema de ficheiros. É recomendável manter esta definição definida como **verdadeiro** , a menos que a aplicação tem uma dependência na desativação fsfreeze.

6. A estrutura de script está agora configurada. Se a cópia de segurança da VM já está configurada, a cópia de segurança seguinte invoca os scripts e aciona uma cópia de segurança consistentes com aplicações. Se a cópia de segurança VM não está configurada, configure-a utilizando [cópia de segurança do Azure que as máquinas virtuais cofres dos serviços de recuperação.](https://docs.microsoft.com/azure/backup/backup-azure-vms-first-look-arm)

## <a name="troubleshooting"></a>Resolução de problemas

Certifique-se de adicionar o registo adequado ao escrever o pré- script de e scripts pós-implementação e reveja os registos de script para corrigir quaisquer problemas de script. Se ainda tiver problemas a execução de scripts, consulte a tabela seguinte para obter mais informações.

| Erro | Mensagem de erro | Ação recomendada |
| ------------------------ | -------------- | ------------------ |
| Pré-ScriptExecutionFailed |O pré-script de devolveu um erro, pelo que a cópia de segurança poderá não ser consistentes com aplicações.   | Consulte os registos de falha para o script corrigir o problema.|  
|   Post ScriptExecutionFailed |    O script de pós-implementação devolveu um erro que pode afetar o estado da aplicação. |    Consulte os registos de falha para o script corrigir o problema e verifique o estado da aplicação. |
| Pré-ScriptNotFound |  O pré-script de não foi encontrado na localização especificada no **VMSnapshotScriptPluginConfig.json** ficheiro de configuração. |   Certifique-se que ao script de disponibilizar está presente no caminho especificado no ficheiro de configuração para garantir que a cópia de segurança consistentes com aplicações.|
| Post ScriptNotFound | O pós-script de não foi encontrado na localização especificada no **VMSnapshotScriptPluginConfig.json** ficheiro de configuração. |   Certifique-se que posterior ao script de disponibilizar está presente no caminho especificado no ficheiro de configuração para garantir que a cópia de segurança consistentes com aplicações.|
| IncorrectPluginhostFile | O **Pluginhost** ficheiro, que é fornecido com a extensão de VmSnapshotLinux, está danificado, pelo que não é possível executar pré- scripts de e scripts pós-implementação e a cópia de segurança, não será consistentes com aplicações. | Desinstalar o **VmSnapshotLinux** extensão e serão automaticamente reinstalado com a cópia de segurança seguinte para corrigir o problema. |
| IncorrectJSONConfigFile | O **VMSnapshotScriptPluginConfig.json** ficheiro incorreto, por isso, a pré-script e não é possível executar o script de pós-implementação e a cópia de segurança, não será consistentes com aplicações. | Transferir a cópia de [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig) e configurá-la novamente. |
| Script de InsufficientPermissionforPre | Para executar scripts, utilizador de "raiz" deve ser o proprietário do ficheiro e o ficheiro deve ter permissões "700" (ou seja, deve ter apenas "proprietário" "ler", "escrita" e "executar" permissões). | Certifique-se o utilizador de "raiz" é o "proprietário" do ficheiro de script e que apenas "proprietário" tem "ler", "escrita" e "executar" permissões. |
| Script de InsufficientPermissionforPost | Para executar scripts, o utilizador raiz deve ser o proprietário do ficheiro e o ficheiro deve ter permissões "700" (ou seja, deve ter apenas "proprietário" "ler", "escrita" e "executar" permissões). | Certifique-se o utilizador de "raiz" é o "proprietário" do ficheiro de script e que apenas "proprietário" tem "ler", "escrita" e "executar" permissões. |
| Pré-ScriptTimeout | A execução do pré-script cópia de segurança consistentes com aplicações excedido. | Verifique o script e aumentar o tempo limite no **VMSnapshotScriptPluginConfig.json** ficheiro que está localizado em **etc/azure**. |
| Post ScriptTimeout | A execução do pós-script de cópia de segurança consistentes com aplicações excedeu o tempo limite. | Verifique o script e aumentar o tempo limite no **VMSnapshotScriptPluginConfig.json** ficheiro que está localizado em **etc/azure**. |

## <a name="next-steps"></a>Passos Seguintes
[Configurar cópia de segurança VM para um cofre dos serviços de recuperação](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms)
