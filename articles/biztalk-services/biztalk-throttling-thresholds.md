---
title: Saiba mais sobre a limitação dos serviços BizTalk | Documentos da Microsoft
description: Saiba mais sobre os respetivos limiares de limitação e resultante comportamentos de tempo de execução para os serviços BizTalk. Limitação baseia-se no uso de memória e o número de mensagens. MABS, WABS
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: f6663cf2-cda4-4bac-855e-27d2ad5c4fa4
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: daab61a0ea9321b0fb918c60688215c80088e0bc
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51243356"
---
# <a name="biztalk-services-throttling"></a>BizTalk Services: Limitação

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Serviços BizTalk do Azure implementa a otimização do serviço com base nas duas condições: utilização de memória e o número de processamento de mensagens de simultâneas. Este tópico lista os limiares de limitação e descreve o comportamento de tempo de execução quando ocorre uma condição de limitação.

## <a name="throttling-thresholds"></a>Limiares de limitação
A tabela seguinte lista de limitação de origem e de limiares:

|  | Descrição | Limiar inferior | Limiar de alta |
| --- | --- | --- | --- |
| Memória |% do total do sistema disponíveis de memória/PageFileBytes. <p><p>PageFileBytes disponíveis total é de aproximadamente 2 vezes a quantidade de RAM do sistema. |60% |70% |
| Processamento de mensagens |Número de mensagens em simultâneo de processamento |40 * número de núcleos |100 * número de núcleos |

Quando for atingido um limiar elevado, o BizTalk Services do Azure começa a limitar. Limitação termina quando é atingido o limite inferior. Por exemplo, seu serviço está a utilizar 65% de memória de sistema. Nesta situação, o serviço não limitar. Seu serviço é iniciado através de 70% de memória de sistema. Nesta situação, o serviço regula e continua a acelerar até que o serviço utiliza memória de sistema do 60% (threshold baixa).

Serviços BizTalk do Azure controla o estado de limitação (estado normal versus estado limitado) e a duração de limitação.

## <a name="runtime-behavior"></a>Comportamento de tempo de execução
Quando os serviços BizTalk do Azure entra num Estado de limitação, ocorre o seguinte:

* A limitação é por instância de função. Por exemplo:<br/>
  Limitação de RoleInstanceA. Não é limitação RoleInstanceB. Nesta situação, as mensagens no RoleInstanceB são processadas como esperado. As mensagens no RoleInstanceA são descartadas e falharem com o erro seguinte:<br/><br/>
  **O servidor está ocupado. Tente novamente.**<br/><br/>
* Quaisquer origens de extração não consultar ou transferir uma mensagem. Por exemplo:<br/>
  Um pipeline obtém as mensagens de uma origem externa do FTP. Obtém a instância de função fazendo a solicitação num Estado de limitação. Nesta situação, o pipeline para transferir mensagens adicionais até que a instância de função parar de limitação.
* Uma resposta é enviada para o cliente para que o cliente pode submeter novamente a mensagem.
* Tem de aguardar até a limitação está resolvida. Especificamente, tem de aguardar até que seja atingido o limite inferior.

## <a name="important-notes"></a>Notas importantes
* Não é possível desativar a limitação.
* Não é possível modificar o respetivos limiares de limitação.
* A limitação é implementada em todo o sistema.
* O servidor de base de dados SQL do Azure também tem a limitação incorporada.

## <a name="additional-azure-biztalk-services-topics"></a>Tópicos de serviços BizTalk do Azure adicionais
* [Instalar o SDK dos serviços BizTalk do Azure](https://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [Tutoriais: Serviços BizTalk do Azure](https://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [Como posso começar a utilizar o SDK dos Serviços BizTalk do Azure](https://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Serviços BizTalk do Azure](https://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Consultar Também
* [Serviços BizTalk: Programador, básico, Standard e Premium gráfico de edições](https://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [Serviços BizTalk: Gráfico de Estado de Aprovisionamento](https://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [Serviços BizTalk: Separadores Dashboard, Monitorizar e Dimensionar](https://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [Serviços BizTalk: Cópia de segurança e Restauro](https://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [Serviços BizTalk: Nome e Chave do Emissor](https://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>

