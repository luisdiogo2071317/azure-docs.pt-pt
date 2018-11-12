---
title: Limites de tamanho de pedido de firewall de aplicação Web e listas de exclusão no Gateway de aplicação do Azure - portal do Azure
description: Este artigo fornece informações sobre limites de tamanho de pedido de firewall de aplicação web e configuração no Gateway de aplicação com o portal do Azure apresenta uma lista de exclusão.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.workload: infrastructure-services
ms.date: 11/6/2018
ms.author: victorh
ms.openlocfilehash: f89841c7712737d2d55601c6525e975274b4a103
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51036722"
---
# <a name="web-application-firewall-request-size-limits-and-exclusion-lists-public-preview"></a>Limites de tamanho de pedido de firewall de aplicação Web e de exclusão contém uma lista (pré-visualização pública)

A firewall de aplicações do Gateway de aplicação Azure web (WAF) fornece proteção para aplicativos web. Este artigo descreve os limites de tamanho do pedido de WAF e configuração de listas de exclusão.

> [!IMPORTANT]
> Configuração de limites de tamanho do pedido de WAF e listas de exclusão está atualmente em pré-visualização pública. Esta pré-visualização é fornecida sem um contrato de nível de serviço e não é recomendada para cargas de trabalho de produção. Algumas funcionalidades poderão não ser suportadas ou poderão ter capacidades limitadas. Veja os [Termos Suplementares de Utilização para Pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para obter mais informações.

## <a name="waf-request-size-limits"></a>Limites de tamanho do pedido de WAF

![Limites de tamanho do pedido](media/application-gateway-waf-configuration/waf-requestsizelimit.png)

Firewall de aplicações Web permite aos usuários configurar limites de tamanho do pedido dentro inferiores e superiores. As seguintes configurações de limites de dois tamanho estão disponíveis:

- O campo de tamanho do corpo de pedido máximo é especificado em KBs e controles carrega de limite de tamanho pedido geral, excluindo todos os ficheiros. Este campo pode variar de 1 KB mínimo ao valor máximo de 128 KB. O valor predefinido para o tamanho do corpo de pedido é 128 KB.
- O campo de limite de carregamento do ficheiro é especificado em MB e rege o tamanho de carregamento de ficheiro máximo permitido. Este campo pode ter um valor mínimo de 1 MB e um máximo de 500 MB. O valor predefinido para o limite de carregamento de ficheiro é 100 MB.

WAF também oferece um botão configurável para ativar ou desativar à inspeção de corpo de pedido. Por predefinição, a inspeção de corpo de pedido é ativada. Se a inspeção de corpo do pedido estiver desativada, o WAF não avalia o conteúdo do corpo da mensagem HTTP. Nesses casos, o WAF continua para impor as regras de WAF em cabeçalhos, cookies e URI. Se a inspeção de corpo do pedido estiver desativada, o campo de tamanho do corpo de pedido máximo não é aplicável e não é possível definir. Desativar a inspeção de corpo de pedido permite que as mensagens maior do que 128 KB sejam enviados para o WAF, mas o corpo da mensagem não é inspecionado relativamente a vulnerabilidades.

## <a name="waf-exclusion-lists"></a>Listas de exclusão de WAF

![waf exclusion.png](media/application-gateway-waf-configuration/waf-exclusion.png)

Listas de exclusão de WAF permitem aos utilizadores omitir determinados atributos de pedido de uma avaliação de WAF. Um exemplo comum é o que Active Directory inserido tokens que são utilizados para autenticação ou campos de palavra-passe. Esses atributos são suscetíveis a conter os carateres especiais que podem disparar um falso positivo das regras WAF. Assim que um atributo é adicionado à lista de exclusão de WAF, não é levada em consideração por qualquer regra de WAF configurada e Active Directory. Listas de exclusão são globais em escopo.

Os seguintes atributos podem ser adicionados às listas de exclusão:

* Cabeçalhos de pedido
* Pedido de Cookies
* Corpo do Pedido

   * Dados de várias partes do formulário
   * XML
   * JSON

Pode especificar um cabeçalho de pedido exata, body, cookie ou correspondência de atributo de cadeia de consulta, ou, opcionalmente, pode especificar correspondências parciais.

Seguem-se os operadores de critérios de correspondência suportados:

- **É igual a**: Este operador é usado para uma correspondência exata. Por exemplo, para a seleção de um cabeçalho denominado **bearerToken**, utilize o operador equals com o Seletor de definido como **bearerToken**.
- **Começa com**: Este operador corresponde a todos os campos que começam com o valor de Seletor especificado.
- **Termina com**: Este operador corresponde a todos os campos de solicitação que terminam com o valor de Seletor especificado.
- **Contém**: Este operador corresponde a todos os campos de solicitação que contêm o valor de Seletor especificado.

Em todos os casos, a correspondência é sensível a maiúsculas e expressões regulares não são permitidos como seletores.

## <a name="next-steps"></a>Passos Seguintes

Depois de configurar as definições de WAF, pode aprender a ver os registos da WAF. Para obter mais informações, consulte [diagnóstico do Gateway de aplicação](application-gateway-diagnostics.md#diagnostic-logging).