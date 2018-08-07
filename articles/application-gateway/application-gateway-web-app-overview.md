---
title: Descrição geral da multi-inquilino volta termina com o Gateway de aplicação do Azure
description: Esta página fornece uma descrição geral do suporte de Gateway de Aplicação para back-ends multi-inquilino.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 8/1/2018
ms.author: victorh
ms.openlocfilehash: c0084580a2e4860f24aecd37232f38da2e55ccc8
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39578437"
---
# <a name="application-gateway-support-for-multi-tenant-back-ends"></a>Suporte de Gateway de Aplicação para back-ends multi-inquilino

Gateway de aplicação do Azure suporta conjuntos de dimensionamento de máquinas virtuais, interfaces de rede, públicas/privadas IP ou de domínio completamente qualificado o nome (FQDN) como parte dos seus conjuntos de back-end. Por predefinição, o gateway de aplicação não altera o cabeçalho de anfitrião de HTTP recebido do cliente e envia o cabeçalho inalterado para o back-end. Existem inúmeros serviços, como as [Aplicações Web do Azure](../app-service/app-service-web-overview.md), que são multi-inquilino por natureza e dependem de um cabeçalho de anfitrião específico ou de uma extensão SNI para concretizar a resolução para o ponto final correto. Gateway de aplicação suporta agora a capacidade dos utilizadores substituírem o cabeçalho de anfitrião HTTP recebido com base nas definições de HTTP de back-end. Esta capacidade permite o suporte para a gestão de API e aplicações Web do Azure de back-end multi-inquilino. Esta capacidade está disponível tanto para o SKU WAF como para o SKU padrão. Suporte de back-end multi-inquilino também funciona com cenários SSL de ponto a ponto e terminação de SSL.

> [!NOTE]
> Configuração de certificado de autenticação não é necessária para serviços do Azure confiáveis, como aplicações Web do Azure.

![cenário de aplicação Web](./media/application-gateway-web-app-overview/scenario.png)

A capacidade de especificar uma substituição de anfitrião é definida nas definições de HTTP e pode ser aplicada a qualquer conjunto de back-end durante a criação de regra. Os back-ends multi-inquilino suportam as duas formas de substituir o cabeçalho de anfitrião e a extensão SNI que se seguem.

1. Capacidade de definir o nome de anfitrião para um valor fixo nas definições de HTTP. Esta capacidade garante que o cabeçalho de anfitrião é substituído para este valor para todo o tráfego para o conjunto de back-end em que são aplicadas as definições de HTTP. Ao utilizar o SSL ponto a ponto, este nome de anfitrião substituído é utilizado na extensão SNI. Esta capacidade possibilita cenários onde um farm de conjunto de back-end espera um cabeçalho de anfitrião diferente do cabeçalho de anfitrião de cliente recebido.

2. A capacidade de derivar o nome de anfitrião, o IP ou FQDN dos membros do conjunto de back-end. Definições de HTTP também disponibilizam uma opção para escolher o nome de anfitrião de FQDN de um membro conjunto de back-end, se configurado com a opção de derivar o nome de anfitrião de um membro do conjunto de back-end individual. Ao utilizar o SSL ponto a ponto, este nome de anfitrião é derivado a partir do FQDN e é utilizado na extensão SNI. Esta capacidade possibilita cenários em que um conjunto de back-end pode ter dois ou mais serviços de PaaS de multi-inquilino, como de aplicações web do Azure e o cabeçalho de anfitrião do pedido para cada membro contém o nome de anfitrião derivado a partir do respetivo FQDN.

> [!NOTE]
> Em ambos os casos descritos atrás, as definições afetam apenas o comportamento do tráfego ativo e não o comportamento da pesquisa do estado de funcionamento. As pesquisas personalizadas já suportam a capacidade de especificar um cabeçalho de anfitrião na configuração da pesquisa. Agora, as pesquisas personalizadas também suportam a capacidade de derivar o comportamento do cabeçalho de anfitrião a partir das definições de HTTP configuradas atualmente. Esta configuração pode ser especificada com o parâmetro `PickHostNameFromBackendHttpSettings` na configuração da pesquisa. Para a funcionalidade ponto a ponto funcionar, tanto a pesquisa como as definições de HTTP têm de ser modificadas de modo a refletirem a configuração correta.

Com esta capacidade, os clientes especificam as opções nas definições de HTTP e as pesquisas personalizadas com a configuração adequada. Esta definição, em seguida, está associada a um serviço de escuta e um conjunto de back-end utilizando uma regra.

## <a name="next-steps"></a>Passos Seguintes

Saiba como configurar um gateway de aplicação com uma aplicação web como membro do conjunto de back-end, visite a página: [aplicações de web de configurar o serviço de aplicações com o Gateway de aplicação](application-gateway-web-app-powershell.md)
