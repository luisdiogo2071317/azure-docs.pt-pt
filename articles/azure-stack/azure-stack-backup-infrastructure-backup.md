---
title: "Dados de cópia de segurança e recuperação para a pilha do Azure com o serviço de cópia de segurança da infraestrutura | Microsoft Docs"
description: "Pode criar cópias de segurança e restaurar a configuração e dados de serviço utilizando o serviço de cópia de segurança da infraestrutura."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 9B51A3FB-EEFC-4CD8-84A8-38C52CFAD2E4
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: 84ce0d72ff826ecb3f5deff165db00a1e50ae89d
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2018
---
# <a name="backup-and-data-recovery-for-azure-stack-with-the-infrastructure-backup-service"></a>Dados de cópia de segurança e recuperação para a pilha do Azure com o serviço de cópia de segurança da infraestrutura

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode criar cópias de segurança e restaurar a configuração e dados de serviço utilizando o serviço de cópia de segurança da infraestrutura. Cada instalação de Azure pilha contém uma instância do serviço. Pode utilizar cópias de segurança criadas pelo serviço para a reimplementação da nuvem de pilha do Azure para restaurar a identidade, segurança e dados do Azure Resource Manager.

Pode ativar a cópia de segurança quando estiver pronto para colocar a sua nuvem em produção. Não ative a cópia de segurança se pretender realizar testes e a validação durante um longo período de tempo.

Antes de ativar o serviço de cópia de segurança, certifique-se de que tem [requisitos no local](#verify-requirements-for-the-infrastructure-backup-service).

> [!Note]  
> O serviço de cópia de segurança de infraestrutura não inclui aplicações e dados do utilizador. Consulte os artigos seguintes para obter instruções sobre a cópia de segurança e restaurar [serviços aplicacionais](https://aka.ms/azure-stack-app-service), [SQL](https://aka.ms/azure-stack-ms-sql), e [MySQL](https://aka.ms/azure-stack-mysql) fornecedores de recursos e os dados de utilizador associados...

## <a name="the-infrastructure-backup-service"></a>O serviço de cópia de segurança da infraestrutura

Os serviços contém as seguintes funcionalidades.

| Funcionalidade                                            | Descrição                                                                                                                                                |
|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Serviços de infraestrutura de cópia de segurança                     | Coordene a cópia de segurança através de um subconjunto de serviços de infraestrutura na pilha do Azure. Se ocorrer um desastre, os dados podem ser restaurados como parte da reimplementação. |
| Compressão e encriptação de dados de cópia de segurança exportados | Cópia de segurança dados são comprimidos e encriptados pelo sistema antes de este é exportado para a localização de armazenamento externo fornecida pelo administrador.                |
| Tarefa de cópia de segurança de monitorização                              | Sistema notifica quando passos falharão e remediação de tarefas de cópia de segurança.                                                                                                |
| Experiência de gestão de cópia de segurança                       | Cópia de segurança RP suporta a ativação da cópia de segurança.                                                                                                                         |
| Nuvem de recuperação                                     | Se existir uma perda catastrófica de dados, as cópias de segurança podem ser utilizadas para restaurar informações de pilha do Azure core como parte da implementação.                                 |

## <a name="verify-requirements-for-the-infrastructure-backup-service"></a>Certifique-se os requisitos para o serviço de cópia de segurança da infraestrutura

- **Localização de armazenamento**  
  Precisa de uma partilha de ficheiros acessível a partir da pilha do Azure que pode conter sete cópias de segurança. Cada cópia de segurança é cerca de 10 GB. A partilha de deve ser capaz de armazenar 140 GB de cópias de segurança. Para obter mais informações sobre como selecionar uma localização de armazenamento para o serviço de cópia de segurança de infraestrutura de pilha do Azure, consulte [requisitos de cópia de segurança controlador](azure-stack-backup-reference.md#backup-controller-requirements).
- **Credenciais**  
  Precisará de uma conta de utilizador de domínio e credenciais, por exemplo, pode utilizar as credenciais de administrador de pilha do Azure.
- **Chave de encriptação**  
  Ficheiros de cópia de segurança estão encriptados com esta chave. Certifique-se armazenar esta chave numa localização segura. Depois de definir esta chave pela primeira vez ou rodar a chave no futuro, não é possível ver esta chave desta interface. Para obter mais instruções gerar uma chave pré-partilhada, siga os scripts em [ativar a cópia de segurança de pilha do Azure com o PowerShell](http://azure-stack-backup-enable-backup-powershell.md).

## <a name="next-steps"></a>Passos Seguintes

- Saiba como [ativar a cópia de segurança do Azure pilha do portal de administração do](azure-stack-backup-enable-backup-console.md).
- Saiba como [ativar cópia de segurança para a pilha do Azure com o PowerShell](azure-stack-backup-enable-backup-powershell.md).
- Saiba como [cópia de segurança pilha do Azure](azure-stack-backup-back-up-azure-stack.md )
- Saiba como [recuperar a partir de perda catastrófica de dados](azure-stack-backup-recover-data.md)
