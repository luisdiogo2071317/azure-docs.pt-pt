---
title: Os serviços BizTalk utilizando registos de operações de resolução de problemas | Documentos da Microsoft
description: Resolver problemas relacionados com os serviços BizTalk utilizando registos de operações. MABS, WABS
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: 1081a9c6-58cc-4a76-8ac8-11e5e7a6ea27
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: f69035202a3358af38ebaf8e94abdd3b030e633f
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51256073"
---
# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>BizTalk Services: Resolver problemas com os registos de operações

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

## <a name="what-are-the-operation-logs"></a>Quais são os registos de operação
Os registos de operações é uma funcionalidade de serviços de gestão que permite-lhe ver o histórico de registos de operações executadas nos seus serviços do Azure, incluindo os serviços BizTalk. Isto permite-lhe ver os dados históricos relacionados com operações de gestão na sua subscrição do BizTalk Service até 180 dias.

> [!NOTE]
> Esta funcionalidade só captura os registos para operações de gestão nos serviços do BizTalk, como quando o serviço foi iniciado, de segurança até, e assim por diante. Essas operações são controladas usando o [APIs de REST do serviço BizTalk](https://msdn.microsoft.com/library/azure/dn232347.aspx). Para obter uma lista completa das operações que são controlados usando os serviços de gestão, consulte [operações controladas através do Azure serviços de gestão](#bizops).<br/><br/>
> Isso não captura os registos de atividades relacionadas ao tempo de execução do BizTalk Service (por exemplo, a mensagem processada pelo pontes e assim por diante.). Para ver estes registos, utilize a vista de controlo do portal dos serviços BizTalk. Para obter mais informações, consulte [mensagens de controlo](https://msdn.microsoft.com/library/azure/hh949805.aspx).
> 
> 

## <a name="view-biztalk-services-operation-logs"></a>Ver os registos de operações dos serviços BizTalk
1. No portal, selecione **dos serviços de gestão**e, em seguida, selecione a **os registos de operações** separador.
2. Pode filtrar os registos com base nos parâmetros diferentes, como a subscrição, intervalo de datas, o tipo de serviço (por exemplo, os serviços BizTalk), nome do serviço ou estado da operação (com êxito, falha).
3. Selecione a marca de verificação para ver a lista filtrada. A imagem seguinte mostra as atividades relacionadas ao testbiztalkservice: ![ver os registos de operações][ViewLogs] 
4. Para ver mais sobre uma operação específica, selecione a linha e clique em **detalhes** na barra de tarefas na parte inferior.

## <a name="bizops"></a>Operações controladas utilizando os serviços de gestão do Azure
A tabela seguinte lista as operações que são controladas usando os serviços de gestão do Azure:

| Nome da Operação | Tarefa |
| --- | --- |
| CreateBizTalkService |Operação para criar um novo BizTalk Service |
| DeleteBizTalkService |Operação para eliminar um serviço BizTalk |
| RestartBizTalkService |Operação de reiniciar um serviço BizTalk |
| StartBizTalkService |Operação para iniciar um serviço BizTalk |
| StopBizTalkService |Operação para parar um serviço BizTalk |
| DisableBizTalkService |Operação para desativar um BizTalk Service |
| EnableBizTalkService |Operação para ativar um BizTalk Service |
| BackupBizTalkService |Operação de cópia de segurança de um serviço BizTalk |
| RestoreBizTalkService |Operação para restaurar um BizTalk Service a partir de cópia de segurança especificada |
| SuspendBizTalkService |Operação para suspender um BizTalk Service |
| ResumeBizTalkService |Operação para retomar um serviço BizTalk |
| ScaleBizTalkService |Operação para dimensionar um serviço BizTalk ou reduzir verticalmente |
| ConfigUpdateBizTalkService |Operação para atualizar a configuração de um serviço BizTalk |
| ServiceUpdateBizTalkService |Operação para aumentar ou diminuir um BizTalk Service para uma versão diferente |
| PurgeBackupBizTalkService |Operação para remover as cópias de segurança do serviço BizTalk fora do período de retenção |

## <a name="see-also"></a>Consultar Também
* [Serviço de cópia de segurança do BizTalk](https://go.microsoft.com/fwlink/p/?LinkID=325584)
* [Restaurar o serviço BizTalk a partir de cópia de segurança](https://go.microsoft.com/fwlink/p/?LinkID=325582)
* [Serviços BizTalk: Programador, básico, Standard e Premium gráfico de edições](https://go.microsoft.com/fwlink/p/?LinkID=302279)
* [Os serviços BizTalk: aprovisionamento](https://go.microsoft.com/fwlink/p/?LinkID=302280)
* [Serviços BizTalk: Gráfico de Estado de Aprovisionamento](https://go.microsoft.com/fwlink/p/?LinkID=329870)
* [Serviços BizTalk: Separadores Dashboard, Monitorizar e Dimensionar](https://go.microsoft.com/fwlink/p/?LinkID=302281)
* [Serviços BizTalk: limitação](https://go.microsoft.com/fwlink/p/?LinkID=302282)
* [Serviços BizTalk: Nome e Chave do Emissor](https://go.microsoft.com/fwlink/p/?LinkID=303941)
* [Como posso começar a utilizar o SDK dos Serviços BizTalk do Azure](https://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png

