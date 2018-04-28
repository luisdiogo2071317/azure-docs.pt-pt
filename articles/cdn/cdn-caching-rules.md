---
title: Controlar a colocação em cache comportamento com colocação em cache as regras a CDN do Azure | Microsoft Docs
description: Pode utilizar a CDN regras a colocação em cache para definir ou modificar o comportamento de expiração da cache de predefinição global e com condições, por exemplo, extensões de ficheiro e caminho de URL.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: v-deasim
ms.openlocfilehash: 18704f2d2a553d7fafb16575ce81128ab47bf09c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="control-azure-cdn-caching-behavior-with-caching-rules"></a>Controlo do Azure CDN colocação em cache comportamento com colocação em cache as regras

> [!NOTE] 
> Regras de colocação em cache só estão disponíveis para **CDN do Azure Standard da Verizon** e **CDN do Azure Standard da Akamai**. Para **CDN do Azure Premium da Verizon**, pode utilizar o [motor de regras da CDN do Azure](cdn-rules-engine.md) no **gerir** portal para uma funcionalidade semelhante.
 
Azure entrega rede conteúdos (CDN) oferece duas formas de controlar a forma como os ficheiros são colocadas em cache: 

- Regras de colocação em cache: Este artigo descreve como pode utilizar a colocação em cache as regras de rede de entrega de conteúdos (CDN) para definir ou modificar o comportamento de expiração da cache de predefinição global e com condições personalizadas, tais como uma extensão de ficheiro e caminho do URL. CDN do Azure fornece dois tipos de regras a colocação em cache:

   - Global regras a colocação em cache: pode definir uma regra de colocação em cache global para cada ponto final no seu perfil, o que afeta todos os pedidos para o ponto final. A regra de colocação em cache global substitui quaisquer cabeçalhos de cache diretiva HTTP, se definir.

   - Regras de colocação em cache personalizadas: pode configurar um ou mais personalizado a colocação em cache as regras para cada ponto final no seu perfil. Personalizada de colocação em cache as regras de correspondência de caminhos específicos e extensões de ficheiro são processados por ordem e substituir a regra de colocação em cache global, se definir. 

- Colocação em cache de cadeia de consulta: pode ajustar a forma como a CDN do Azure trata a colocação em cache de pedidos com cadeias de consulta. Para informações, consulte [CDN do Azure de controlo de colocação em cache comportamento com cadeias de consulta](cdn-query-string.md). Se o ficheiro não for colocáveis, a cadeia de consulta definição a colocação em cache não tem efeito, com base na colocação em cache as regras e comportamentos predefinidos do CDN.

Para obter informações sobre predefinição comportamento de colocação em cache e cabeçalhos directiva a colocação em cache, consulte [funciona como colocação em cache](cdn-how-caching-works.md). 


## <a name="accessing-azure-cdn-caching-rules"></a>Aceder a regras de colocação em cache da CDN do Azure

1. Abra o portal do Azure, selecione um perfil da CDN e, em seguida, selecione um ponto final.

2. No painel esquerdo em definições, selecione **regras a colocação em cache**.

   ![Botão de regras de colocação em cache de CDN](./media/cdn-caching-rules/cdn-caching-rules-btn.png)

   O **regras a colocação em cache** é apresentada a página.

   ![Página de regras de colocação em cache de CDN](./media/cdn-caching-rules/cdn-caching-rules-page.png)


## <a name="caching-behavior-settings"></a>Definições de comportamento de colocação em cache
Para regras de colocação em cache global e personalizadas, pode especificar o seguinte **comportamento de colocação em cache** definições:

- **Ignorar a cache**: não colocar em cache e ignorar a origem fornecidos pelo cabeçalhos cache-diretiva.

- **Substituir**: Ignorar origem fornecidos pelo cabeçalhos cache-diretiva; utilizar em vez disso, a duração de cache fornecido.

- **Definir se estiverem em falta**: Honor origem fornecidos pelo Directiva cache cabeçalhos, caso existam; caso contrário, utilizam a duração de cache fornecido.

![Regras globais de colocação em cache](./media/cdn-caching-rules/cdn-global-caching-rules.png)

![Regras personalizadas de colocação em cache](./media/cdn-caching-rules/cdn-custom-caching-rules.png)

## <a name="cache-expiration-duration"></a>Duração de expiração da cache
Para regras de colocação em cache global e personalizadas, pode especificar a duração de expiração da cache em dias, horas, minutos e segundos:

- Para o **substituir** e **definir se estiverem em falta** **comportamento de colocação em cache** definições, o intervalo de durações de cache válido entre 0 segundos e 366 dias. Para um valor de 0 segundos, a CDN coloca em cache o conteúdo, mas tem revalidate cada pedido com o servidor de origem.

- Para o **ignorar a cache** definição, a duração da cache é automaticamente definida para 0 segundos e não pode ser alterada.

## <a name="custom-caching-rules-match-conditions"></a>Correspondência de condições de regras de cache personalizada

Para as regras de cache personalizada, estão disponíveis dois correspondência condições:
 
- **Caminho**: esta condição corresponde ao caminho de URL, excluindo o nome de domínio e suporta o símbolo de caráter universal (\*). Por exemplo, _/myfile.html_, _/meu/pasta / *_, e _/my/images/*.jpg_. O comprimento máximo é superior a 260 carateres.

- **Extensão**: esta condição corresponde a extensão de ficheiro do ficheiro pedida. Pode fornecer uma lista separada por vírgulas de extensões de ficheiro a corresponder. Por exemplo, _. jpg_, _. mp3_, ou _PNG_. O número máximo de extensões é 50 e o número máximo de carateres por extensão é 16. 

## <a name="global-and-custom-rule-processing-order"></a>Ordem de processamento da regra global e personalizadas
As regras de colocação em cache de global e personalizadas são processadas pela seguinte ordem:

- Regras de colocação em cache global têm precedência sobre o comportamento predefinido do CDN colocação em cache (definições de cache diretiva cabeçalho HTTP). 

- Colocação em cache de regras personalizadas têm precedência sobre regras de colocação em cache global, onde estas são aplicadas. Regras de colocação em cache personalizadas são processadas ordem da parte superior da parte inferior. Ou seja, se um pedido de corresponder a ambas as condições, as regras na parte inferior da lista de precedência sobre as regras na parte superior da lista. Por conseguinte, deve colocar regras mais específicas mais baixo na lista.

**Exemplo**:
- Regra de colocação em cache global: 
   - Comportamento de colocação em cache: **substituir**
   - Duração de expiração da cache: 1 dia

- A colocação em cache personalizada da regra #1:
   - Correspondem à condição: **caminho**
   - Corresponde ao valor:   _/home / *_
   - Comportamento de colocação em cache: **substituir**
   - Duração de expiração da cache: 2 dias

- A colocação em cache personalizada da regra #2:
   - Correspondem à condição: **extensão**
   - Corresponde ao valor: _*.HTML_
   - Comportamento de colocação em cache: **definir se estiverem em falta**
   - Duração de expiração da cache: 3 dias

Quando estas regras estiverem definidas, um pedido para  _&lt;nome de anfitrião de ponto final&gt;_ acionadores.azureedge.net/home/index.html a colocação em cache personalizada da regra 2 #, a qual está definido como: **definir se estiverem em falta** e 3 dias. Por conseguinte, se o *index.html* ficheiro tem `Cache-Control` ou `Expires` cabeçalhos HTTP, são honrados; caso contrário, se estes cabeçalhos não estiver definidos, o ficheiro é colocado em cache para 3 dias.

> [!NOTE] 
> Os ficheiros que estão em cache antes de uma alteração de regra mantêm a respetiva definição de duração de cache de origem. Para repor o respetivas durações de cache, tem de [remover o ficheiro](cdn-purge-endpoint.md). Para **CDN do Azure da Verizon** pontos finais, pode demorar até 90 minutos para novas regras de colocação em cache entre em vigor.

## <a name="see-also"></a>Consulte também

- [Como funciona a colocação em cache](cdn-how-caching-works.md)
- [Tutorial: Conjunto do Azure CDN regras a colocação em cache](cdn-caching-rules-tutorial.md)
