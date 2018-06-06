---
title: Estrutura de identidade híbrida - requisitos de proteção de dados do Azure | Microsoft Docs
description: Quando planear a sua solução de identidade híbrida, identificar os requisitos de proteção de dados para a sua empresa e que opções estão disponíveis para melhor satisfazer estes requisitos.
documentationcenter: ''
services: active-directory
author: billmath
manager: mtillman
editor: ''
ms.assetid: 40dc4baa-fe82-4ab6-a3e4-f36fa9dcd0df
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/30/2018
ms.component: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 423624e999e4170ceddf097125e4fb3e9a40384b
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34800926"
---
# <a name="plan-for-enhancing-data-security-through-a-strong-identity-solution"></a>Planear a segurança dos dados através de uma solução de identidade segura de otimização
O primeiro passo na proteção de dados é identificar quem pode aceder a esses dados. Além disso, tem de ter uma solução de identidade que pode integrar com o sistema para fornecer capacidades de autenticação e autorização. Autenticação e autorização, muitas vezes, são confundidos entre si e as respetivas funções misunderstood. Na realidade, forem diferentes, conforme mostrado na imagem abaixo:

![](./media/hybrid-id-design-considerations/mobile-devicemgt-lifecycle.png)

**Fases do ciclo de vida de gestão de dispositivos móveis**

Quando planear a sua solução de identidade híbrida, tem de compreender os requisitos de proteção de dados para a sua empresa e que opções estão disponíveis para melhor satisfazer estes requisitos.

> [!NOTE]
> Depois de concluir a planear a segurança dos dados, reveja [determinar os requisitos de autenticação multifator](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md) para se certificar de que as suas seleções sobre requisitos de autenticação multifator não foram afetados pelas decisões efetuadas nesta secção.
> 
> 

## <a name="determine-data-protection-requirements"></a>Determinar os requisitos de proteção de dados
Na duração de mobilidade, a maioria das empresas tem um objetivo comum: permitir que os utilizadores sejam produtivos em dispositivos móveis, enquanto estão no local ou remotamente a partir de qualquer lugar para aumentar a produtividade. Empresas que têm esses requisitos também será preocupadas com o número de ameaças que têm de ser mitigados para manter os dados da empresa seguros e manter a privacidade do utilizador. Cada empresa pode ter requisitos diferentes neste aspeto, regras de conformidade diferente que irão variar, de acordo com o setor a empresa está a funcionar serão levar a decisões de conceção diferentes. 

No entanto, existem alguns aspetos de segurança que devem ser explorou e validados, independentemente da indústria.

## <a name="data-protection-paths"></a>Caminhos de proteção de dados
![](./media/hybrid-id-design-considerations/data-protection-paths.png)

**Caminhos de proteção de dados**

No diagrama acima, o componente de identidade vai ser o primeiro ser verificado antes dos dados são acedidos. No entanto, estes dados podem estar em diferentes Estados durante o período de tempo que foi acedido. Cada número neste diagrama representa um caminho no qual dados podem ser localizados num ponto no tempo. Estes números são explicados abaixo:

1. Proteção de dados ao nível do dispositivo.
2. Proteção de dados em trânsito.
3. Proteção de dados, rest no local.
4. Proteção de dados Inativos na nuvem.

É necessário que a solução de identidade híbrida é capaz de tirar partido no local e aos recursos da gestão de identidade para identificar o utilizador antes de que concede acesso aos dados na nuvem. Quando planear a sua solução de identidade híbrida, certifique-se de que as perguntas seguintes são respondidas, de acordo com requisitos da sua organização:

## <a name="data-protection-at-rest"></a>Proteção de dados Inativos
Independentemente de onde os dados estão inativos (dispositivo, nuvem ou no local), é importante fazer uma avaliação neste aspeto, a compreender as necessidades da organização. Para esta área, certifique-se de que são mais frequentes seguintes perguntas:

* A sua empresa precisa de proteger os dados Inativos?
  * Se Sim, é a solução de identidade híbrida consegue integrar com a sua infraestrutura no local atual?
  * Se Sim, é a solução de identidade híbrida consegue integrar com as cargas de trabalho localizadas na nuvem?
* A gestão de identidades de nuvem é capaz de proteger as credenciais do utilizador e outros dados armazenados na nuvem?

## <a name="data-protection-in-transit"></a>Proteção de dados em trânsito
Dados em trânsito entre o dispositivo e o Centro de dados ou entre o dispositivo e na nuvem devem ser protegidos. No entanto, a ser em trânsito não significa necessariamente um processo de comunicações com um componente fora do seu serviço em nuvem; são transmitidos internamente, além disso, essas impedindo duas redes virtuais. Para esta área, certifique-se de que são mais frequentes seguintes perguntas:

* A sua empresa precisa de proteger dados em trânsito?
  * Se Sim, é a solução de identidade híbrida consegue integrar com controlos seguros, tais como o SSL/TLS?
* A gestão de identidades de nuvem manter os o tráfego de e para dentro do arquivo de diretório (dentro e entre centros de dados) assinado?

## <a name="compliance"></a>Conformidade
Requisitos de conformidade normativa, leis e regulamentos irão variar de acordo com a indústria que pertence a sua empresa. As empresas na elevada indústrias reguladas devem ser abordadas preocupações de gestão de identidades relacionado com problemas de conformidade. Normas como Sarbanes-Oxley (SOX), o Health Insurance Portability e Accountability Act (HIPAA), o ato de Gramm-Leach-Bliley (GLBA) e a e Payment Card Industry Data segurança (PCI DSS) são strict sobre identidade e acesso. A solução de identidade híbrida que a empresa irá adotar tem de ter as capacidades centrais irão cumprir os requisitos de uma ou mais estes regulamentos. Para esta área, certifique-se de que são mais frequentes seguintes perguntas:

* A solução de identidade híbrida é compatível com os requisitos de regulamentação para a sua empresa?
* Foi criada a solução de identidade híbrida 
* as capacidades que irá ativar da sua empresa para serem requisitos regulamentares de conformidade? 

> [!NOTE]
> Certifique-se de que toma nota de cada resposta e que compreende os fundamentos. [Definir a estratégia de proteção de dados](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) abordará as opções disponíveis e as vantagens/desvantagens de cada opção.  Ao responder a estas questões, vai selecionar que opções melhor se adapta às sua empresa precisa.
> 
> 

## <a name="next-steps"></a>Passos Seguintes
 [Determinar os requisitos de gestão de conteúdo](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)

## <a name="see-also"></a>Consultar Também
[Descrição geral das considerações de design](active-directory-hybrid-identity-design-considerations-overview.md)

