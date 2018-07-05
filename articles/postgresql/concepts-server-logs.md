---
title: Registos do servidor na base de dados do Azure para PostgreSQL
description: Este artigo descreve como base de dados do Azure para PostgreSQL gera registos de consulta e o erro e, como retenção de registo está configurado.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: bcca8ce8d11482dd8517992297b7e8a5b94ac8b1
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37435495"
---
# <a name="server-logs-in-azure-database-for-postgresql"></a>Registos do servidor na base de dados do Azure para PostgreSQL 
Base de dados do Azure para PostgreSQL gera erro de consulta e registos. No entanto, o acesso aos logs de transação não é suportado. Registos de consulta e o erro podem ser utilizados para identificar, resolução de problemas e reparar erros de configuração e de desempenho inferior ao ideal. Para obter mais informações, consulte [relatório de erros e o registo](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html).

## <a name="access-server-logs"></a>Aceder a registos do servidor
Pode listar e transferir registos de erros de servidor PostgreSQL do Azure com o portal do Azure, [CLI do Azure](howto-configure-server-logs-using-cli.md)e APIs REST do Azure.

## <a name="log-retention"></a>Retenção do registo
Pode definir o período de retenção para registos de sistema com o **log\_retenção\_período** parâmetro associado ao servidor. A unidade para este parâmetro é dias. O valor predefinido é 3 dias. O valor máximo é de 7 dias. O servidor tem de ter suficiente alocado de armazenamento para incluir os ficheiros de registo retida.
Rodar os ficheiros de registo para a cada uma hora ou 100 MB de tamanho, o que ocorrer primeiro.

## <a name="configure-logging-for-azure-postgresql-server"></a>Configurar o registo para o servidor PostgreSQL do Azure
Pode ativar o registo de consulta e o registo de erro para o seu servidor. Registos de erros podem conter informações aspirador automática, ligação e os pontos de verificação.

Pode ativar o registo de consulta para a sua instância de BD de PostgreSQL ao definir os dois parâmetros de servidor: `log_statement` e `log_min_duration_statement`.

O **log\_instrução** parâmetro controla quais instruções SQL são registadas. É recomendável definir esse parâmetro como ***todos os*** para registar todas as instruções; o valor predefinido é none.

O **log\_min\_duração\_instrução** parâmetro define o limite em milissegundos de uma instrução de ter sessão iniciada. Todas as instruções SQL que são executadas mais do que a definição de parâmetro são registadas. Este parâmetro é desativado e definido para menos 1 (-1) por predefinição. Ativar este parâmetro pode ser útil para rastreamento de consultas não otimizadas em seus aplicativos.

O **log\_min\_mensagens** permite-lhe controlar que níveis de mensagens é escritos no registo de servidor. A predefinição é aviso. 

Para obter mais informações sobre estas definições, consulte [relatório de erros e o registo](https://www.postgresql.org/docs/9.6/static/runtime-config-logging.html) documentação. Para configurar particularmente o banco de dados do Azure para parâmetros do servidor PostgreSQL, consulte [personalizar os parâmetros de configuração de servidor com a CLI do Azure](howto-configure-server-parameters-using-cli.md).

## <a name="next-steps"></a>Passos Seguintes
- Para aceder aos registos utilizando a interface de linha de comandos da CLI do Azure, veja [acesso e configurar os registos do servidor com a CLI do Azure](howto-configure-server-logs-using-cli.md).
- Para obter mais informações sobre parâmetros do servidor, consulte [personalizar os parâmetros de configuração de servidor com a CLI do Azure](howto-configure-server-parameters-using-cli.md).
