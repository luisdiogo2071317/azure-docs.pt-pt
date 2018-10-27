---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: e18d0a6a01a86f844edc213fc95003cf4f4b46c9
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165477"
---
* Ligue à máquina virtual do Servidor de Processos através da Ligação de Ambiente de Trabalho Remoto.
* Pode iniciar o cspsconfigtool.exe, ao clicar no atalho disponível no ambiente de trabalho. (A ferramenta será iniciada de forma automática se isso pela primeira vez inicia sessão no servidor de processos).
  - Nome completamente qualificado do Servidor de Configuração (FQDN) ou Endereço IP
  - Porta em que o servidor de Configuração está a ouvir. O valor deve ser 443
  - Frase de Acesso de ligação para ligar ao servidor de configuração.
  - Porta de Transferência de Dados a ser configurada para este Servidor de Processos. Deixe o valor predefinido conforme está, a menos que tenha alterado para um número de porta diferente no seu ambiente.

    ![Registar Servidor de Processos](./media/site-recovery-vmware-register-process-server/register-ps.png)
* Clique no botão Guardar para guardar a configuração e registar o Servidor de Processos.
