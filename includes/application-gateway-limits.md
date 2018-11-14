---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 11/09/2018
ms.author: victorh
ms.openlocfilehash: 44f9400a1ecebd3c204ec0b891c5d7fe01bdfd25
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51628189"
---
| Recurso | Limite predefinido | Nota |
| --- | --- | --- |
| Gateway de Aplicação |1000 por subscrição | |
| Configurações IP de Front-end |2 |1 pública e 1 privada |
| Portas de Front-end |40 | |
| Conjuntos de Endereços de Back-end |40 | |
| Servidores de Back-end por conjunto |1200 | |
| Serviços de Escuta HTTP |40 | |
| Regras de balanceamento de carga HTTP |400 |n. º de serviços de escuta HTTP * n |
| Definições de HTTP de back-end |40 | |
| Instâncias por gateway |75 | |
| Certificados SSL |40 |1 por Serviço de Escuta HTTP |
| Certificados de autenticação |40 | |
| Pedir o mín de tempo limite |1 segundo | |
| Tempo limite máximo de pedidos |24 horas | |
| Número de sites |20 |1 por Serviço de Escuta HTTP |
| Mapas de URL por serviço de escuta |1 | |
| Mapeiam as regras com base no caminho máximas, por URL|100|
| Configurações de redirecionamento |40| |
| Ligações de WebSocket em simultâneo |5000| |
|Comprimento máximo do URL|8000|
| Tamanho máximo do ficheiro de carregamento padrão |2 GB | |
| WAF de tamanho de carregamento de ficheiro máximo |Gateways do WAF médio - 100 MB<br>Gateways do WAF grandes - 500 MB| |
|Limite de tamanho do corpo de WAF (sem ficheiros)|128 KB|
