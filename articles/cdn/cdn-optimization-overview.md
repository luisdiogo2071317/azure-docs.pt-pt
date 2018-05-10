---
title: Otimizar a entrega de conteúdos do Azure para o seu cenário
description: Como otimizar o fornecimento de conteúdo para cenários específicos
services: cdn
documentationcenter: ''
author: dksimpson
manager: ''
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: rli
ms.openlocfilehash: de748f7fa33b0250b1572dd5ae55cddf15003a0e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="optimize-azure-content-delivery-for-your-scenario"></a>Otimizar a entrega de conteúdos do Azure para o seu cenário

Quando distribuir conteúdos a uma grande audiência global, é fundamental para garantir que a entrega otimizada do seu conteúdo. Rede de entrega de conteúdos do Azure (CDN) pode otimizar a experiência de entrega com base no tipo de conteúdo que tem. O conteúdo pode ser um Web site, uma transmissão em fluxo em direto, um vídeo ou um ficheiro grande para transferência. Quando cria um ponto final de CDN, especifique um cenário no **otimizado para** opção. À sua escolha determina qual otimização é aplicada para o conteúdo a entregar a partir do ponto final de CDN.

Opções de otimização foram concebidas para utilizar os comportamentos de melhor prática para melhorar o desempenho de entrega de conteúdos e descarga de origem melhor. As opções de cenário afetam o desempenho ao modificar as configurações para a colocação em cache parcial, objeto de agrupamento e a política de repetição de falha de origem. 

Este artigo fornece uma descrição geral de diversas funcionalidades de Otimização e quando deve utilizá-los. Para obter mais informações sobre funcionalidades e limitações, consulte os respetivos artigos em cada tipo de otimização individuais.

> [!NOTE]
> O **otimizado para** opções podem variar consoante o fornecedor que selecionou. Fornecedores CDN aplicam melhoramento de diversas formas, dependendo do cenário. 

## <a name="provider-options"></a>Opções do fornecedor

**Azure CDN padrão da Microsoft** suporta as seguintes otimizações:

* [Entrega de web geral](#general-web-delivery). Esta otimização também é utilizada para suportes de dados de transmissão em fluxo e transferência de ficheiros grandes.


**Azure CDN Standard da Verizon**, e **CDN do Azure Premium da Verizon** suporta as seguintes otimizações:

* [Entrega de web geral](#general-web-delivery). Esta otimização também é utilizada para suportes de dados de transmissão em fluxo e transferência de ficheiros grandes.

* [Aceleração de site dinâmico](#dynamic-site-acceleration) 


**Azure CDN Standard da Akamai** suporta as seguintes otimizações:

* [Entrega de web geral](#general-web-delivery) 

* [Suporte de dados gerais de transmissão em fluxo](#general-media-streaming)

* [Suporte de dados de vídeo a pedido de transmissão em fluxo](#video-on-demand-media-streaming)

* [Transferência de ficheiros grandes](#large-file-download)

* [Aceleração de site dinâmico](#dynamic-site-acceleration) 

A Microsoft recomenda que teste variações de desempenho entre diferentes fornecedores para selecionar o fornecedor ideal para a entrega.

## <a name="select-and-configure-optimization-types"></a>Selecionar e configurar os tipos de otimização

Para criar um novo ponto final, selecione um tipo de otimização que descreva o cenário e tipo de conteúdo que pretende que o ponto final para fornecer. **Entrega de web geral** é a seleção predefinida. Para existente **rede entrega de conteúdos do Azure da Akamai** pontos finais, pode atualizar a opção de otimização em qualquer altura. Esta alteração não interromper a entrega da CDN do Azure. 

1. Dentro de um **CDN do Azure Standard da Akamai** perfil, selecione um ponto final.

    ![Seleção de ponto final ](./media/cdn-optimization-overview/01_Akamai.png)

2. Em definições, selecione **otimização**. Em seguida, selecione um tipo a partir de **otimizado para** na lista pendente.

    ![Seleção de Otimização e o tipo](./media/cdn-optimization-overview/02_Select.png)

## <a name="optimization-for-specific-scenarios"></a>Otimização para cenários específicos

Pode otimizar o ponto final da CDN para um destes cenários. 

### <a name="general-web-delivery"></a>Entrega geral Web

Entrega de web geral é a opção de otimização mais comuns. Foi concebido para a otimização de conteúdo web gerais, tais como páginas Web e aplicações web. Esta otimização também pode ser utilizada para o ficheiro e transferências de vídeo.

Um Web site típico contém conteúdo estático e dinâmico. Conteúdo estático inclui imagens, folhas de estilo que podem ser colocadas em cache e entregue aos diferentes utilizadores e bibliotecas de JavaScript. Conteúdo dinâmico é personalizado para um utilizador individual, tais como itens de notícias adaptados para um perfil de utilizador. Conteúdo dinâmico não está em cache porque é exclusivo para cada utilizador, como o conteúdo de carrinho compras. Entrega de geral web pode otimizar o seu site completo. 

> [!NOTE]
> Se utilizar **CDN do Azure Standard da Akamai**, pode querer utilizar esta otimização se o tamanho de ficheiro médio for inferior a 10 MB. Se o tamanho de ficheiro médio for superior a 10 MB, selecione **transferências de ficheiros grandes** do **otimizado para** na lista pendente.

### <a name="general-media-streaming"></a>Transmissão geral de multimédia

Se precisar de utilizar o ponto final de transmissão em fluxo em direto e vídeo a pedido de transmissão em fluxo, recomendamos que suporte de dados gerais de otimização de transmissão em fluxo.

Suporte de dados de transmissão em fluxo está na altura confidencial, porque os pacotes que chegam tarde no cliente podem causar uma experiência de visualização degradada, tais como frequentes da memória intermédia de conteúdos de vídeo. Suporte de dados de transmissão em fluxo otimização reduz a latência de entrega de conteúdo do suporte de dados e proporciona uma experiência de transmissão em fluxo uniforme de utilizadores. 

Este cenário é comum para clientes do serviço de Media Services do Azure. Quando utilizar os Media Services do Azure, obter um ponto de final transmissão em fluxo que pode ser utilizado para a transmissão em direto e a pedido. Com este cenário, os clientes não precisam de mudar para outro ponto final alterar em direto para transmissão em fluxo a pedido. Otimização de transmissão em fluxo geral de suporte de dados suporta este tipo de cenário.

Para **CDN do Azure Standard da Microsoft**, **CDN do Azure Standard da Verizon**, e **CDN do Azure Premium da Verizon**, utilize o tipo de otimização de entrega web geral para entrega o conteúdo de multimédia de transmissão em fluxo geral.

Para obter mais informações sobre a otimização de transmissão em fluxo de suporte de dados, consulte [suporte de dados de transmissão em fluxo otimização](cdn-media-streaming-optimization.md).

### <a name="video-on-demand-media-streaming"></a>Suporte de dados de vídeo a pedido de transmissão em fluxo

Otimização de transmissão em fluxo de suporte de dados de vídeo a pedido melhora o conteúdo de transmissão em fluxo de vídeo a pedido. Se utilizar um ponto final de transmissão em fluxo de vídeo a pedido, pode querer utilizar esta opção.

Para **CDN do Azure Standard da Microsoft**, **CDN do Azure Standard da Verizon**, e **CDN do Azure Premium da Verizon**, utilize o tipo de otimização de entrega web geral para entrega o conteúdo de multimédia de transmissão em fluxo vídeo a pedido.

Para obter mais informações sobre a otimização de transmissão em fluxo de suporte de dados, consulte [suporte de dados de transmissão em fluxo otimização](cdn-media-streaming-optimization.md).

> [!NOTE]
> Se o ponto final de CDN serve principalmente conteúdo de vídeo a pedido, utilize este tipo de otimização. A principal diferença entre esta Otimização e o suporte de dados gerais otimização de transmissão em fluxo é o limite de tentativas de ligação. O limite de tempo é muito mais curto para trabalhar com cenários de transmissão em fluxo em direto.

### <a name="large-file-download"></a>Transferência de ficheiro grande

Para **CDN do Azure Standard da Akamai**, transferências de ficheiros grandes estão otimizadas para o conteúdo com mais de 10 MB. Se o tamanho de ficheiro médio for inferior a 10 MB, utilize a entrega de web geral. Se os tamanhos de média ficheiros forem consistentemente superiores a 10 MB, poderá ser mais eficiente para criar um ponto de final separado para ficheiros grandes. Por exemplo, atualizações de software ou firmware normalmente são ficheiros grandes. Para fornecer ficheiros maiores do que 1.8 GB, é necessária a otimização de transferência de ficheiros grandes.

Para **CDN do Azure Standard da Microsoft**, **CDN do Azure Standard da Verizon**, e **CDN do Azure Premium da Verizon**, utilize o tipo de otimização de entrega web geral para distribuir os conteúdos de transferência de ficheiros grandes. Não há nenhuma limitação de tamanho de transferência de ficheiros.

Para obter mais informações sobre a otimização de ficheiros grandes, consulte [otimização de ficheiros grandes](cdn-large-file-optimization.md).

### <a name="dynamic-site-acceleration"></a>Aceleração de site dinâmico

 Aceleração dinâmicas do site está disponível para **CDN do Azure Standard da Akamai**, **CDN do Azure Standard da Verizon**, e **CDN do Azure Premium da Verizon** perfis. Esta otimização envolve uma taxa adicional a utilizar. Para obter mais informações, consulte [preços de rede de entrega de conteúdos](https://azure.microsoft.com/pricing/details/cdn/).

Aceleração dinâmicas do site inclui várias técnicas que beneficiam a latência e o desempenho de conteúdo dinâmico. Incluem técnicas de otimização de rota e da rede, a otimização de TCP e muito mais. 

Pode utilizar esta otimização para acelerar uma aplicação web que inclui várias respostas que não são colocáveis. Os exemplos são os resultados da pesquisa, transações de saída ou dados em tempo real. Pode continuar a utilizar funções de colocação em cache de CDN essenciais para os dados estáticos. 

Para obter mais informações sobre a aceleração de dinâmicas do site, consulte [aceleração dinâmicas do site](cdn-dynamic-site-acceleration.md).



