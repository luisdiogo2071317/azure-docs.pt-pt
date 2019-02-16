---
title: Processamento de dados e de registos de pilha do Azure | Documentos da Microsoft
description: Saiba mais sobre a forma como o Azure Stack recolhe informações.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2019
ms.author: PatAltimore
ms.reviewer: chengwei
ms.lastreviewed: 02/14/2019
ms.openlocfilehash: 7beb9dc31e8987d69c2cf9cf64be36caf0e0e308
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/15/2019
ms.locfileid: "56318928"
---
# <a name="azure-stack-log-and-customer-data-handling"></a>Azure Stack log e de clientes dados tratamento 
*Aplica-se a: Integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*  

Na medida do Microsoft é um processador ou subprocessor dos dados pessoais em conexão com o Azure Stack, Microsoft torna a todos os clientes, em vigor a 25 de Maio de 2018, os compromissos em (a) do processamento de"dados pessoais. Aprovisionar GDPR"da seção de"Termos de proteção de dados"a [termos dos Online Services](https://nam06.safelinks.protection.outlook.com/?url=http%3A%2F%2Fwww.microsoftvolumelicensing.com%2FDocumentSearch.aspx%3FMode%3D3%26DocumentTypeId%3D31&data=02%7C01%7Ccomartin%40microsoft.com%7Ce2ce478261764c79c3f308d68df01136%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636852459551078818&sdata=cpWsfZTBHpqEFr50DWQOryq342U8shgeFgMXVPQz5ug%3D&reserved=0) e (b) na União Europeia geral dados proteção regulamento condições em 4 de anexo da [termos dos Online Services](https://nam06.safelinks.protection.outlook.com/?url=http%3A%2F%2Fwww.microsoftvolumelicensing.com%2FDocumentSearch.aspx%3FMode%3D3%26DocumentTypeId%3D31&data=02%7C01%7Ccomartin%40microsoft.com%7Ce2ce478261764c79c3f308d68df01136%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636852459551088813&sdata=bv1CBiaCnYmjiv6S0dFCbWEd4fNCkPBjBwgylNa%2FNt0%3D&reserved=0). 

Como o Azure Stack reside nos centros de dados do cliente, a Microsoft é o controlador de dados exclusivamente dos dados que são partilhados com a Microsoft por meio [diagnóstico](azure-stack-diagnostics.md), [telemetria](azure-stack-telemetry.md), e [defaturação](azure-stack-usage-reporting.md).  

## <a name="data-access-controls"></a>Controlos de acesso de dados 
Os funcionários da Microsoft, que estão atribuídos a investigar um incidente de suporte específico, serão concedidos acesso só de leitura aos dados criptografados. Os funcionários da Microsoft também tem acesso a ferramentas que podem ser usadas para eliminar os dados se for necessário. Todos os acessos aos dados dos clientes são auditados e registados.  

Controlos de acesso de dados:
1.  Dados só são retidos durante um máximo de 90 dias depois de fechar o caso.
2.  O cliente tem sempre a opção para que os dados removidos em qualquer altura nesse período de 90 dias.
3.  Os funcionários da Microsoft recebem acesso aos dados no caso a caso e apenas conforme necessário para o ajudar a resolver o problema de suporte. 
4.  No caso em que Microsoft têm de partilhar dados do cliente com os parceiros OEM, consentimento cliente é obrigatório.  

### <a name="what-data-subject-requests-dsr-controls-do-customers-have"></a>O que fazer de controles de pedidos de requerente de dados (DSR) os clientes têm?
Como mencionado anteriormente, a Microsoft suporta a eliminação de dados sob demanda por solicitação do cliente. Os clientes podem solicitar que o nosso engenheiro de suporte eliminar todos os seus registos para um determinado caso em qualquer altura da escolha do cliente, antes dos dados são apagados permanentemente.  

### <a name="does-microsoft-notify-customers-when-the-data-is-deleted"></a>Microsoft notifique os clientes quando os dados são eliminados?
Para a ação de eliminação de dados automática (90 dias depois de fechar caso), vamos entrar em contacto com os clientes não de forma proativa e notificá-los sobre a eliminação. 

Para a ação de eliminação de dados sob demanda, engenheiro de suporte da Microsoft tem acesso à ferramenta onde eles podem iniciar a eliminação de dados sob demanda e podem fornecer confirmação no telefone com o cliente quando tiver terminado.

## <a name="diagnostic-data"></a>Dados de diagnóstico
Como parte do processo de suporte, os operadores do Azure Stack pode [partilhar os registos de diagnóstico](azure-stack-diagnostics.md) com as equipes de engenharia e suporte do Azure Stack para facilitar a resolução de problemas.

A Microsoft fornece uma ferramenta e o pedido de script para os clientes recolher e carregar ficheiros de registo de diagnóstico. Depois de recolhidos, os ficheiros de registo são transferidos através de HTTPS protegido conexão criptografada para a Microsoft. Como o HTTPS fornece a criptografia durante a transmissão, não há nenhuma palavra-passe necessária para a encriptação em trânsito. Após serem recebidos, os registos são encriptados e armazenados até que eles são automaticamente eliminados 90 dias depois de fechar o incidente de suporte.

## <a name="telemetry-data"></a>Dados de telemetria
[Telemetria do Azure Stack](azure-stack-telemetry.md) carrega automaticamente os dados de sistema para a Microsoft via a experiência do usuário conectado. Operadores do Azure Stack tem controlos para personalizar as definições de privacidade e de recursos de telemetria em qualquer altura.

Microsoft não a pretende recolher dados confidenciais, como números de cartão de crédito, nomes de utilizador e palavras-passe, endereços de e-mail ou informações confidenciais semelhantes. Se determinarmos que informações confidenciais foi recebidas inadvertidamente, podemos eliminá-lo. 

## <a name="billing-data"></a>Dados de faturação
[Faturação do Azure Stack](azure-stack-usage-reporting.md) tira partido do Azure global faturação e utilização de pipelines e, portanto, em conformidade com as diretrizes de conformidade do Microsoft.

Operadores do Azure Stack pode configurar o Azure Stack para reencaminhar as informações de utilização para o Azure para faturação. Isto é necessário para os clientes de vários nós do Azure Stack que escolher o modelo de faturação de pagamento-como-utiliza. Relatórios de utilização é controlada de forma independente de telemetria e não é necessária para os clientes de vários nós que escolher o modelo de capacidade ou para usuários do Kit de desenvolvimento do Azure Stack. Nestes cenários, relatórios de utilização podem ser desativado usando [o script de Registro](azure-stack-usage-reporting.md).


## <a name="next-steps"></a>Passos Seguintes 
[Saiba mais sobre a segurança do Azure Stack](azure-stack-security-foundations.md) 
