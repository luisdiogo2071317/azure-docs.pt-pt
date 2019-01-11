---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 11/09/2018
ms.author: victorh
ms.openlocfilehash: 66dea07a1ff725c6707b19bc6ebdc5563f1b158b
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2019
ms.locfileid: "54211894"
---
| Recurso | Limite predefinido | Nota |
| --- | --- | --- |
| Gateway de Aplicação |1000 por subscrição | |
| Configurações de IP Front-end |2 |1 pública e 1 privada |
| Portas de front-end |100<sup>1</sup> | |
| Conjuntos de endereços de back-end |100<sup>1</sup> | |
| Servidores de back-end por conjunto |1200 | |
| Serviços de Escuta HTTP |100<sup>1</sup> | |
| Regras de balanceamento de carga HTTP |100<sup>1</sup> | |
| Definições de HTTP de back-end |100<sup>1</sup> | |
| Instâncias por gateway |32 | |
| Certificados SSL |100<sup>1</sup> |1 por Serviço de Escuta HTTP |
| Certificados de autenticação |100 | |
| Certificados de raiz fidedigna |100 | |
| Pedir o mín de tempo limite |1 segundo | |
| Tempo limite máximo de pedidos |24 horas | |
| Número de sites |100<sup>1</sup> |1 por Serviço de Escuta HTTP |
| Mapas de URL por serviço de escuta |1 | |
| Mapeiam as regras com base no caminho máximas, por URL|100||
| Configurações de redirecionamento |100<sup>1</sup>| |
| Ligações simultâneas do WebSocket |5000| |
| Comprimento máximo do URL|8000||
| Tamanho máximo do ficheiro de carregamento padrão |2 GB | |
| WAF de tamanho de carregamento de ficheiro máximo |Gateways do WAF médio - 100 MB<br>Gateways do WAF grandes - 500 MB| |
| Limite de tamanho do corpo de WAF (sem ficheiros)|128 KB||

<sup>1</sup> em caso de capacidade de WAF SKU é recomendável que limite o número de recursos para 40 para um desempenho ideal.
