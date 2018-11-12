---
title: Azure e uma oferta de serviço de consultoria do Dynamcis 365 | Documentos da Microsoft
description: Guia para definir e publicar uma oferta de serviço consultoria do Azure ou Dynamcis 365 no Portal de parceiros de nuvem.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: pbutlerm
ms.openlocfilehash: 43f1e71486cedbeccb2c02388147a68756581e50
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51037775"
---
# <a name="azure-and-dynamics-365-consulting-service-offer"></a>Azure e uma oferta de serviço de consultoria do Dynamics 365

<table> <tr> <td>Esta secção explica como publicar um serviço de consultoria da Microsoft de qualquer <a href="https://azuremarketplace.microsoft.com">do Azure Marketplace</a> ou o <a href="https://appsource.microsoft.com">Marketplace do AppSource</a>. Soluções com base no Microsoft <a href="https://dynamics.microsoft.com">Dynamics 365</a>, <a href="https://products.office.com">do Office 365</a>, <a href="https://powerbi.microsoft.com">Power BI</a>, e <a href="https://powerapps.microsoft.com">PowerApps</a> são elegíveis para ser listado no AppSource. Outras ofertas com base em outro da Microsoft <a href="https://azure.microsoft.com/services">serviços do Azure</a> são elegíveis para uma listagem no Azure Marketplace. </td> <td><img src="./media/consulting-services-publishing-offer/consulting-icon1.png"  alt="Microsoft consulting icon" /></td> </tr> </table>


# <a name="publishing-benefits"></a>Benefícios de publicação

A publicação de qualquer um dos marketplaces da Microsoft tem vantagens significativas:

- Promova a sua empresa ao tirar partido da marca Microsoft.
- Potencialmente alcançar mais de 100 milhões de utilizadores do Office 365 e Dynamics 365 no AppSource; alcance mais de 200 mil organizações através do Azure Marketplace.
- Obtenha oportunidades potenciais de alta qualidade desses mercados.
- Obtenha os seus serviços promovidos por equipes de campo e telesales da Microsoft.


<a name="define-your-consulting-services-offer"></a>Definir a sua oferta de serviços de consultoria
-------------------------------------

Defina a sua oferta de serviço de consultoria em pacote. Concentre-se no escopo fixo, fixo duração, estimada, preço fixo (ou gratuitas) e orientada a pré-sales principalmente as ofertas para um único cliente. Selecione compromissos em pacote repetíveis que tenham sido populares e uma forma eficaz de dirigir novos negócios para.

<a name="publish-a-consulting-service-offer"></a>Publicar uma oferta de serviço de consultoria
----------------------------------

As secções seguintes descrevem o processo de publicação de sua oferta de serviços de consultoria

1.  Criar uma nova oferta
2.  Definir definições de oferta
3.  Introduza os detalhes de loja e se pretende publicar no Azure Marketplace ou no AppSource.
4.  Publicar a sua oferta

### <a name="create-a-new-offer"></a>Criar uma nova oferta

Para criar uma nova oferta, conclua os seguintes passos:

1.  No menu principal do Portal de parceiro de Cloud, selecione **nova oferta**.
2.  No menu da nova oferta, selecione **serviço de consultoria**.

    ![Criar uma nova oferta dos serviços de consultoria](media/consulting-services-publishing-offer/cppselectnewconsultingoffer.png)

### <a name="define-offer-settings"></a>Definir definições de oferta

No ecrã da nova oferta, a primeira etapa é criar a identidade de oferta.  A identidade de oferta é composta por três partes: **ID da oferta**, **ID de publicador**, e **nome**.  Cada uma dessas partes são abordados nas seções a seguir.

#### <a name="offer-id"></a>ID de Oferta

Este identificador é um nome exclusivo criado por primeiro, submeta a oferta. Tem de consistir apenas carateres alfanuméricos em minúsculas, traços ou carateres de sublinhado. O ID da oferta estará visível no URL e os impactos de resultados do motor de pesquisa. Por exemplo, *yourcompanyname\_exampleservice*

Conforme mostrado no exemplo, o ID de oferta é acrescentado ao seu ID de publicador para criar um identificador exclusivo. Isso é exposto como uma ligação permanente que pode ser reservada e é indexada por motores de busca. 

*Depois de uma oferta em direto, não pode ser atualizada de identificador*

#### <a name="publisher-id"></a>ID de publicador

Este identificador está relacionado com a sua conta. Quando estiver conectado com a sua conta organizacional, a ID de editor aparecerá no menu pendente.

#### <a name="name"></a>Nome

Esta cadeia é o que será apresentado como o nome da oferta no AppSource ou no Azure Marketplace.

**Importante:** introduza apenas o nome do serviço real aqui. Não inclua a duração e o tipo de serviço.

O exemplo seguinte pelo Edgewater Fullscope mostra como o nome da oferta é reunido. O nome da oferta é apresentado como:

![Criar uma nova oferta dos serviços de consultoria](media/consulting-services-publishing-offer/cppsampleconsultingoffer.png)

O nome da oferta é composto por quatro partes:

-   **Duração:** – definidos no **Vitrine detalhes** separador do editor. Duração pode ser expresso em horas, dias ou semanas.
-   **Tipo de serviço:** – definidos no **Vitrine detalhes** separador do editor. Tipos de serviços são `Assessment`, `Briefing`, `Implementation`, `Proof of concept`, e `Workshop`.
-   **Preposition:** - inserida pelo revisor
-   **Nome:** – definidos no **oferecer definições** página.

A lista seguinte fornece vários nomes de oferta bem com nome:

-   Princípios básicos para serviços profissionais: Resumo de 1 hora
-   Migração de plataforma: Resumo de 1 hora
-   PowerApps e Microsoft Flow: Workshop de 1 dia
-   Serviços do Azure Machine Learning: prova de conceito de 3 semanas
-   Solução de revenda de módulo e clique em: Resumo de 1 hora
-   Traga os seus dados: Workshop de semanas de 1
-   Análise de cloud: Workshop de 3 dias
-   Formação do Power BI: Workshop de 3 dias
-   Solução de gerenciamento de vendas: implementação de 1 semana
-   Início rápido CRM: Workshop de 1 dia
-   Dynamics 365 for Sales: avaliação de 2 dias

Depois de concluir o **oferecer definições** guia, pode salvar sua submissão. O nome de oferta agora vai aparecer acima do editor e pode encontrá-lo em todas as ofertas.

### <a name="enter-storefront-details"></a>Introduza os detalhes Vitrine

Em seguida, tem de introduzir os detalhes para a loja. Os detalhes de loja inclui as secções seguintes:

-   Detalhes da Oferta
-   Informações do publicador
-   Detalhes da lista
-   Artefactos de marketing

#### <a name="offer-details"></a>Detalhes da Oferta

A secção de detalhes de oferta contém os seguintes campos:

-   Resumo da oferta
-   Descrição da oferta

##### <a name="offer-summary"></a>Resumo da oferta

O resumo de oferta é uma breve descrição da sua oferta que aparece imediatamente abaixo do nome da oferta. Texto sem formatação de utilização ao introduzir a resumida de oferta e não devem ter quebras de linha. Seguem-se bons exemplos de resumos de oferta juntamente com seus nomes de oferta correspondente:

*Exemplo 1*

-   **Nome da oferta:** Analytics na Cloud: Workshop de 3 dias
-   **Resumo da oferta:** descrição geral do Microsoft Azure e o Power BI, avaliação do ambiente atual e a Minibarra de prova de conceito.

*Exemplo 2*

-   **Nome da oferta:** industriais do Azure IoT: 30 dias prova de conceito
-   **Resumo da oferta:** criar um piloto de industrial produtos ligados para ligar em segurança o equipamento no campo para uma solução de IoT Hub do Azure com os dashboards, relatórios e notificações.

*Exemplo 3*

-   **Nome da oferta:** serviços profissionais: Resumo de 1 hora
-   **Resumo da oferta:** descrição geral e demonstração de pré-configurado, expandido do Dynamics 365 para o fornecimento da solução de operações avançada de gerenciamento de projetos, faturação e recursos de serviços profissionais.

*Exemplo 4*

-   **Nome da oferta:** Power BI no seu mundo: Workshop de 4 horas
-   **Resumo da oferta:** começar a trabalhar com o seu primeiro dashboard e Aprenda as melhores práticas. Para estudantes até 12, realizada no local.

*Exemplo 5*

-   **Nome da oferta:** Dynamics e projetos: avaliação de 3 dias
-   **Resumo da oferta:** requisitos de recolha e de avaliação de solução ERP concebida para empresas de serviços profissionais e controlada por projeto as empresas.

##### <a name="offer-description"></a>Descrição da oferta

Descrição de utilização do serviço de consultoria. Uma descrição da oferta boa abrange obter detalhes exatos sobre o que o envolvimento será semelhante e qual será o resultado final para o cliente final. Claramente deve ajudar a cliente compreender o que eles recebem.

Inclui ligações de e-mail ou números de telefone para tentar contactá-lo em sua descrição da oferta. Haverá um contacto Me botão com a oferta que irá carregar o leva para o destino de gestão de oportunidades potenciais que identifique a sua oferta.

Irá introduzir a descrição da oferta no formato de Markdown. Se não estiver familiarizado com o Markdown ou a formatação de HTML, pode rever os recursos sobre o [site do Microsoft docs](https://docs.microsoft.com/contribute/how-to-write-use-markdown).

Usar esses formatos garantirá a que sua oferta tem legibilidade máxima para os clientes.

Manter a sua oferta descrição breve e respeitar o limite de carateres que os utilizadores não necessárias ler texto longo. Ainda terá a opção de carregar brochuras marketing, folhas de fato e outros documentos que descrevem a oferta mais detalhadamente.

O exemplo seguinte mostra uma descrição da oferta bem formada e seu nome relacionado e o resumo:

**Nome da oferta:** Analytics na Cloud: Workshop de 3 dias

**Resumo da oferta:** descrição geral do Microsoft Azure e o Power BI, avaliação do ambiente atual e a Minibarra de prova de conceito.

**Descrição da oferta:** Este workshop de 3 dias é para os líderes técnicos e comerciais e é mantido no local na instalação do cliente.

***Agenda***

Dia 1

-   Se concentra em como proteger, dimensionar e organizar dados na cloud da Microsoft com o Azure Data Lake, o HDInsight ou o Azure SQL Data Warehouse.

Dia 2

-   Aborda como configurar e implementar soluções de análise avançada com o Microsoft R e o Azure Machine Learning.

Dia 3

-   Aborda como desenhar informações acionáveis e operacionalizar a análise com o Power BI, incluindo uma sessão colaborativa conjuntamente criar um dashboard do Power BI.

No final o workshop, o cliente poderá definir um plano de alto nível e uma estratégia de implementação para soluções de dados e análise na cloud da Microsoft.

*Ficheiro de Markdown de exemplo para uma oferta seguindo este formato:*

    This 3-day workshop is for technical and business leaders and is held on-site at the client’s facility.

      ### Agenda

      **Day 1**

      * Focuses on how to secure, scale, and organize data within the Microsoft cloud using Azure Data Lake, HDInsight, or Azure SQL Data Warehouse

      **Day 2**

      * Covers how to configure and deploy advanced analytics solutions with Microsoft R and Azure Machine Learning

      **Day 3**

      * Covers how to draw actionable insights and operationalize analytics with Power BI, including a collaborative session to co-build a Power BI dashboard.


      ### Deliverables
      By the end of the workshop, the client will be able to define a high-level plan and an implementation roadmap for data and analytics solutions in the Microsoft cloud.


#### <a name="publisher-information"></a>**Informações do publicador**

**MPN ID**

Sua ID do 9 dígitos Microsoft Partner Network (MPN). Se não tiver um ID MPN, pode aceder ao Microsoft Partner Center para adquirir uma.

**ID do Centro de parceiros**

Novo Centro de ID de parceiro, se tiver uma.

**MPN ID**

Introduza uma chave secreta para pré-visualizar a oferta no AppSource antes da ativação.
Este identificador não é uma palavra-passe.

#### <a name="listing-details"></a>Detalhes da lista

**Tipo de serviço de consultoria**

A Microsoft está se concentrar exclusivamente em escopo fixo, duração foi corrigida, preço estimado ou fixo (ou gratuitas) e principalmente pre-sales e orientados a ofertas de serviços de consultoria para um único cliente e, para avaliação, implementação, prova de conceito, resumo, ou Workshop ofertas, realizados ambos no local ou virtualmente (informes tem de ser realizado no. local).

Os seguintes cinco tipos de ofertas estão incluídos:

-   **Avaliação:** uma edição de avaliação do ambiente de um cliente para determinar a aplicabilidade de uma solução e fornecer uma estimativa do custo e temporização das atualizações.
-   **Resumo:** uma introdução a uma solução ou um serviço de consultoria para desenhar o interesse dos clientes através de estruturas, demonstrações e exemplos de clientes. Informes tem de ser realizado de local.
-   **Implementação:** uma instalação completa, que resulta numa solução totalmente funcional. Para este projeto-piloto, a Microsoft recomenda a limitar a soluções que podem ser implementados numa semana ou menos.
-   **Prova de conceito:** uma implementação de âmbito limitado para determinar se uma solução atenderá aos requisitos de um cliente.
-   **Workshop:** um compromisso interativo conduzidas no local de um cliente que pode incluir sessões de treinamento, informes, avaliações ou demonstrações criadas no ambiente ou de dados do cliente.

**Disponibilidade de país/região**

Selecione o país e região em que esta oferta de serviço de consultoria vai estar disponível. Não é possível publicar uma oferta única em vários países ou regiões. Tem de ser criada uma nova oferta para cada país ou região.

**Setores**

Selecione os setores que seu serviço de consultoria oferecem mais aplicáveis para.

**Duração**

Selecione um número (por exemplo, 3, 4, etc.) no período de tempo e, em seguida, selecione hora, dia ou semana.

**Produtos principais**

Para publicar no Azure Marketplace, selecione **Azure** como o produto principal e, em seguida, selecione áreas de solução relevantes.

Para publicar no AppSource, selecione **Dynamics 365**, **Power BI**, ou **PowerApps**como seu produto principal. Também pode selecionar outros produtos aplicáveis relevantes e o serviço de consultoria oferecem apresentará em listagens de que estão associadas a cada um desses produtos no AppSource.

**Competências relevantes**

Selecione competências relevantes para esta oferta tê-los apresentados juntamente com os detalhes da oferta.

#### <a name="marketing-artifacts"></a>Artefactos de marketing

**Logótipo da empresa (formato. png, 48 x 48 pixels)**

Carregar uma imagem que será apresentado no mosaico da sua oferta na página de vista de galeria da oferta. A imagem tem de ser uma imagem de formato. PNG com uma resolução de 48x48 pixels.

**Logótipo do logótipo da empresa (formato. png, 216 x 216 pixels) de empresa**

Carregar uma imagem que aparecerá na sua página de detalhes da sua oferta. A imagem tem de ser uma imagem de formato. PNG com uma resolução de pixels 216 x 216.

**Vídeos (máx. 4)**

Carregar até quatro vídeos de estudo de caso de cliente ou vídeos de referência do cliente. Se não tiver nenhum, carregar um vídeo que explica a experiência da sua empresa relacionados com a oferta. Se tiver um showcase de soluções Power BI ou no PowerApps, carregue o vídeo de demonstração aqui. Ligações de vídeo tem de ser para o YouTube ou Vimeo.

**Documentos (máx. 3)**

Carregue o folheto de marketing que descreve a sua oferta de serviço de consultoria em detalhes. Em alternativa, também pode carregar visão geral da empresa, folhas de fato ou estudos de caso.

**Capturas de ecrã (máx. 5)**

Carregar até cinco imagens que fornecem mais informações sobre a oferta, resultados finais da oferta ou sua empresa. Por exemplo, um trecho de sua folheto de marketing, um slide relevante a partir de uma apresentação ou uma imagem que mostra a dinâmica da empresa ou conhecimentos.

### <a name="publish-your-offer"></a>Publicar a sua oferta

Depois de concluir a oferecer definições, os detalhes de loja e contatos, selecione **publicar** e fornecer um endereço de e-mail. Quando a Microsoft está pronto para publicar a sua oferta, receberá um e-mail para a pré-visualização antes de entrar no ar. Pode retornar para o portal para verificar o estado da sua oferta em qualquer ponto no tempo.

Ofertas podem aparecer num "Publicação cancelada" ou "Falha de publicação" Estado durante o processo de publicação. Este estado é uma parte normal do processo e permite que a Microsoft fazer edições em sua oferta. Se vir a sua oferta no "Publicar cancelada", deixá-lo no estado.
