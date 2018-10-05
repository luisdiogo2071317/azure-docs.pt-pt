---
title: SaaS - venda através do Azure | Documentos da Microsoft
description: Descreve a modelo para aplicações SaaS e como implementá-lo de faturação de subscrição.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: pbutlerm
ms.openlocfilehash: f193138fbc5e779c3a6671757320d8918e08db46
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48810666"
---
<a name="saas---sell-through-azure"></a>SaaS - venda através do Azure
=========================

Este artigo descreve o modelo de faturação de subscrição para aplicações SaaS e como implementá-lo no Portal de parceiros de nuvem.


<a name="overview"></a>Descrição geral
--------

O Azure marketplace permite-lhe publicar e lucrar com suas ofertas SaaS através do Azure marketplace. Para os seus clientes, eles podem ser cobrados por meio da faturação do Azure diretamente e apenas receberão uma fatura do Azure para todos os respetivos recursos e serviços que tenham ativado. Para publicação, subscrições de SaaS tem as seguintes vantagens:

-   **Experiência consistente de publicação** - - se de que já publicar ofertas de SaaS ou qualquer outra oferta no Azure Marketplace, a experiência de publicação é através do portal de publicação do mesmo.
-   **Nova loja para a deteção** – todas as ofertas publicadas serão visíveis no externa loja do Azure Marketplace, bem como a extensão do Azure Marketplace no portal do Azure.
-   **Integrado faturação** -agora pode vender em todas as regiões onde o Azure vende e permitir que o Azure tratar da faturação para.
-   **Integrado a geração de oportunidades potenciais** – pode agora receber automaticamente oportunidades potenciais do Azure de CRM à sua escolha, sempre que um utilizador do Azure subscreve ao seu serviço de SaaS com o Azure marketplace.
-   **Venda conjunta com vendedores da Microsoft** - - pode se qualificar para a partilha, listagens de catálogo priorizadas de oportunidades potenciais bidirecionais e a oportunidade de se envolver e fechar negócios lado a lado com vendedores da Microsoft.

<a name="billing-models"></a>Modelos de faturação
--------------

O modelo de faturação suportado atualmente apenas é uma taxa mensal fixa por subscrição do serviço SaaS.

**Nota:** modelos de faturação adicionais podem estar disponíveis no futuro.

Cada oferta SaaS pode ter um ou mais planos (por exemplo, básico ou Premium). Cada plano tem alguns metadados básicos de associados à mesma, juntamente com o preço associado com o plano.

Tem controle total sobre a definição de um plano. Por exemplo, o que um plano básico constituem? O plano é definido por si, e pode fornecer o texto necessário para descrevê-lo como parte do seu plano de publicação.

O preço associado com o plano é a taxa mensal fixa que os utilizadores do Azure irão pagar para utilizar o serviço.

### <a name="what-is-not-supported-today"></a>O que não é suportado atualmente?

-   A faturação baseada nas unidades personalizadas – por exemplo, um preço definido com base no número de pedidos.
-   Faturação com base na alocação de postos de trabalho – por exemplo, permitir que os utilizadores do Azure comprar licenças com base no número de utilizadores.

<a name="end-to-end-flow"></a>Fluxo de ponto a ponto
---------------

A subscrição de SaaS oferecem fluxo a partir de uma perspectiva do usuário final primeiro

**Nota:** essa descrição do fluxo parte do princípio de que publicou a oferta de SaaS no Azure marketplace, chamado "Contoso demonstração SaaS".

![Fluxo de utilizador de subscrição de SaaS](media/saas-offer-subscription/saas-subscription-user-flow.png)

Um utilizador do Azure pode ter as interações com o seu serviço de SaaS a seguir:

-   Detetar o serviço SaaS no Azure Marketplace
-   Subscrever o serviço SaaS no Azure
-   Navegar para o serviço de SaaS a partir do portal do Azure
-   Criar uma conta no serviço SaaS e gerir (plano de alteração/eliminação) a conta de SaaS
-   Anular a subscrição para o serviço de SaaS a partir do portal do Azure

<a name="discover-your-saas-service-in-azure-marketplace"></a>Detetar o serviço SaaS no Azure Marketplace
-----------------------------------------------

Quando os utilizadores iniciar o Azure Marketplace no portal do Azure, irão ver uma categoria chamada "Software como serviço (SaaS)".

![Detetar SaaS utilizando o menu de categoria](media/saas-offer-subscription/saas-category-menu.png)

Os utilizadores também podem procurar o serviço SaaS.

![Detetar a utilizar a pesquisa de SaaS](media/saas-offer-subscription/saas-cpp-search.png)

Os utilizadores podem, em seguida, ver os detalhes do seu serviço e, em seguida, clique em **criar**.

![Criar uma subscrição de SaaS](media/saas-offer-subscription/saas-create-subscription.png)

### <a name="subscribe-to-saas-service-in-azure"></a>Subscrever o serviço de SaaS no Azure

O utilizador pode, em seguida, subscrever o serviço SaaS do Azure.

![Subscrever um serviço SaaS](media/saas-offer-subscription/saas-subscribe-signup.png)

Quando o utilizador subscreve ao seu serviço de SaaS, o utilizador fornece as seguintes informações

-   Nome – O nome que os utilizadores podem detetar ou gerir esta instância de subscrição de SaaS no Azure.
-   Subscrição – O contexto da subscrição que pretende associar a faturação para o serviço SaaS.
-   Plano – O SaaS plano do serviço que querem para subscrever.

O documento de termos de utilização fornecido como parte de publicar a oferta também é mostrado ao utilizador antes do utilizador se inscreve para o serviço SaaS.

O utilizador pode agora subscrever o serviço clicando **Subscribe**.
O Azure envia uma notificação no portal do depois de concluída a subscrição.

### <a name="navigate-to-the-saas-service-from-azure-portal"></a>Navegar para o serviço de SaaS a partir do Portal do Azure

Os utilizadores, em seguida, clique em **Ir para recurso** para ver ou gerir a sua instância de subscrição de SaaS.

![Ver ou gerir a sua instância de SaaS](media/saas-offer-subscription/saas-go-to-resource.png)

O utilizador é notificado de que eles tem de configurar a conta no serviço SaaS pela primeira vez. Faturação deles cessa depois do serviço SaaS notifica do Azure para começar a faturação, o que é quando o utilizador cria uma conta no site de serviço SaaS.

![Configurar a sua instância de SaaS](media/saas-offer-subscription/saas-configure-account.png)

Quando o usuário clica em **configurar a conta**, eles são redirecionados para o ponto de final de serviço SaaS. Durante este redirecionamento, um token é passado como um parâmetro de consulta. Por exemplo:

![Token de contas de SaaS](media/saas-offer-subscription/saas-account-token.png)

Tenha em atenção este valor do token. Este token é uma curta duração e precisa de ser resolvido para obter um identificador de subscrição no Azure.

<a name="creating-a-saas-offer-subscription"></a>Criar uma subscrição da oferta de SaaS
----------------------------------

Para criar essa experiência, existem duas partes do trabalho que são necessários:

-   Ligar o seu site do serviço de SaaS com a Microsoft\'s SaaS APIs. Este documento [venda de SaaS através do Azure — APIs](./cloud-partner-portal-saas-subscription-apis.md) explica como criar esta ligação.  
-   Ativar **venda através do Azure** no Portal de parceiro de Cloud na **informações técnicas** secção bem como fornecer todos os detalhes de configuração.

![Novas informações técnicas de oferta de SaaS](media/cpp-creating-saas-offers/saas-new-offer-technical-info-2.png)

Segue-se obter uma explicação de todos os campos necessários para o **informações técnicas** secção:

|  **Campos de oferta**                 |  **Descrição**                                   |
|  ----------------                 |  -------------------------------------             |
| IDs de subscrição de pré-visualização          | Todos os os subscrição do Azure identificadores utilizados para testar a sua oferta na pré-visualização antes de ser publicamente disponível. |
| Instruções de introdução      | Instruções para partilhar com os seus clientes para os ajudar a ligar à sua aplicação SaaS. É permitida a utilização básica do HTML, essas marcas como `<p>`, `<h1>`, `<li>`, etc.  |
| URL da página de destino                  | O URL do site que irá ser direcionar seus clientes aterrar em depois de adquirir a partir do portal do Azure. Este URL também será o ponto final que irão receber a ligação de APIs para facilitar o comércio com a Microsoft.  |
| Webhook de ligação                | Para todos os eventos assíncronos que a Microsoft tem de enviar a em nome do cliente (exemplo: subscrição do Azure tornou-se inválida), pedimos que nos forneça um webhook da ligação. Se ainda não tiver um sistema de webhook no local, a configuração mais simples é fazer com uma aplicação de lógica de ponto final de HTTP que irá escutar quaisquer eventos que está a ser postados nele e, em seguida, manipulá-las adequadamente.  Para obter mais informações, consulte [chamar, acionar, ou aninhar fluxos de trabalho com pontos de extremidade HTTP no logic apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-http-endpoint). |
| Inquilino do Azure AD ID e o ID de aplicação     | No portal do Azure, é necessário que crie uma aplicação do Active Directory para que possamos validar a ligação entre nossos dois serviços estiver atrás de uma comunicação autenticada. Para estes campos, crie uma aplicação do AD e cole o Id de inquilino correspondente e o Id da aplicação necessária. |
|  |  |


Por fim, se selecionar **venda através do Azure**, há uma seção adicional chamada **planos**. Planos só são necessários se estiver selecionada a origem de venda através do Azure. Esta secção lista os planos específicos e seus preços correspondentes, que oferece suporte a sua aplicação SaaS. Até hoje, vamos permitir preços mensais, com a capacidade de permitir para 1 ou 3-meses de acesso gratuito. Estes planos e preços devem corresponder os planos exatos e os preços que tem no seu próprio site de aplicação SaaS.
