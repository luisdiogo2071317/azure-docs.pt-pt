---
title: "Funciona como colocação em cache | Microsoft Docs"
description: "A colocação em cache é o processo de armazenar dados localmente para que o futuro pedidos para que os dados podem ser acedidos mais rapidamente."
services: cdn
documentationcenter: 
author: dksimpson
manager: 
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/23/2017
ms.author: v-deasim
ms.openlocfilehash: 284b4bcbeafc422a2ed91cec00a5b5b83bb37b7b
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
---
# <a name="how-caching-works"></a>Como funciona a colocação em cache

Este artigo fornece uma descrição geral dos conceitos gerais de colocação em cache e como [Azure rede de entrega de conteúdos (CDN)](cdn-overview.md) utiliza a colocação em cache para melhorar o desempenho. Se gostaria de saber mais sobre como personalizar o comportamento de colocação em cache no ponto final de CDN, consulte [CDN do Azure de controlo de colocação em cache comportamento com colocação em cache regras](cdn-caching-rules.md) e [CDN do Azure de controlo de colocação em cache comportamento com cadeias de consulta](cdn-query-string.md).

## <a name="introduction-to-caching"></a>Introdução à colocação em cache

A colocação em cache é o processo de armazenar dados localmente para que o futuro pedidos para que os dados podem ser acedidos mais rapidamente. O tipo mais comuns de colocação em cache, a colocação em cache browser web, uma web browser armazena cópias dos dados estáticos localmente num disco rígido local. Ao utilizar a colocação em cache, o browser pode evitar efetuar várias ida e volta ao servidor e aceder em vez disso, os mesmos dados localmente, assim poupar tempo e recursos. A colocação em cache é adequada para gerir localmente os dados de pequenos e estáticos, tais como estáticas imagens, CSS ficheiros e ficheiros de JavaScript.

Da mesma forma, a colocação em cache é utilizada por uma rede de entrega de conteúdos em servidores edge próximo do utilizador para evitar pedidos viaja de volta para a origem e reduzindo a latência de utilizador final. Ao contrário de uma cache de browser web, que é utilizada apenas para um único utilizador, a CDN tem uma cache partilhada. Cache CDN partilhado, um ficheiro que é solicitado por um utilizador pode ser acedido mais tarde por outros utilizadores, que reduz significativamente o número de pedidos para o servidor de origem.

Não é possível colocar em cache dinâmicos recursos que alteram frequentemente ou são exclusivas para um utilizador individual. Os tipos de recursos, no entanto, podem tirar partido da otimização de aceleração (DSA) dinâmicas do site da rede de entrega de conteúdo do Azure para melhorias de desempenho.

A colocação em cache pode ocorrer em vários níveis entre o servidor de origem e o utilizador final:

- Servidor Web: utiliza uma cache partilhada (por vários utilizadores).
- Rede de entrega de conteúdos: utiliza uma cache partilhada (por vários utilizadores).
- O fornecedor de serviços de Internet (ISP): utiliza uma cache partilhada (por vários utilizadores).
- Browser da Web: utiliza uma cache privada (para um utilizador).

Cada cache normalmente gere a suas próprias reatualização de recursos e executa a validação quando um ficheiro está desatualizado. Este comportamento está definido no HTTP especificação, a colocação em cache [RFC 7234](https://tools.ietf.org/html/rfc7234).

### <a name="resource-freshness"></a>Actualização de recursos

Porque um recurso em cache potencialmente pode estar desatualizado ou obsoleta (em comparação com o recurso correspondente no servidor de origem), é importante para qualquer mecanismo de colocação em cache para controlar quando o conteúdo é atualizado. Para guardar o consumo de largura de banda e a hora, um recurso em cache não é comparada em para a versão no servidor de origem sempre que é acedido. Em vez disso, desde que um recurso em cache é considerado raiz, este é pressupõe-se que a versão mais atual e é enviada diretamente para o cliente. Um recurso em cache é considerado raiz quando a sua idade é inferior a idade ou período definido por uma definição de cache. Por exemplo, quando um browser reloads uma página web, verifica se cada recurso em cache no disco rígido é novo e carrega-lo. Se o recurso não está actualizado (obsoleto), uma cópia atualizada está carregada do servidor.

### <a name="validation"></a>Validação

Se um recurso é considerado obsoletos, o servidor de origem é pediu a validá-lo, ou seja, determine se os dados na cache ainda corresponde ao que está no servidor de origem. Se o ficheiro foi modificado no servidor de origem, a cache atualiza a versão do recurso. Caso contrário, se o recurso estiver atualizado, os dados são transmitidos diretamente a partir da cache sem validar o primeiro.

### <a name="cdn-caching"></a>Colocação em cache de CDN

A colocação em cache é integral para a forma como uma CDN funciona para acelerar a entrega e reduzir a carga de origem para elementos estáticos, tais como imagens, tipos de letra e vídeos. Na CDN colocação em cache, recursos estáticos seletivamente estão armazenados nos servidores estrategicamente colocadas de modo que são mais locais para um utilizador e tem as seguintes vantagens:

- Porque a maioria do tráfego web é estático (por exemplo, imagens, tipos de letra e vídeos), colocação em cache de CDN reduz a latência de rede, movendo conteúda próximo para o utilizador, reduzindo, deste modo, a distância que circulam de dados.

- Ao descarregar o trabalho para uma CDN, colocação em cache pode reduzir o tráfego de rede e a carga no servidor de origem. Se o fizer, reduz os requisitos de custos e recursos para a aplicação, mesmo quando existem grande número de utilizadores.

Semelhante à forma como a colocação em cache é implementada num browser, pode controlar como colocação em cache é efetuada numa CDN enviando cabeçalhos cache-diretiva. Cabeçalhos de cache diretiva são cabeçalhos HTTP, o que normalmente são adicionados através do servidor de origem. Embora a maioria destes cabeçalhos originalmente foram concebida para abordar a colocação em cache nos browsers do cliente, agora também são utilizados por todas as caches intermédios, como CDNs. 

Dois cabeçalhos podem ser utilizados para definir a actualização da cache: `Cache-Control` e `Expires`. `Cache-Control`é mais atual e tem precedência sobre `Expires`, se existem ambas. Também existem dois tipos de cabeçalhos utilizados para a validação (denominada validações): `ETag` e `Last-Modified`. `ETag`é mais atual e tem precedência sobre `Last-Modified`, se ambos estiverem definidas.  

## <a name="cache-directive-headers"></a>Cabeçalhos de directiva cache

> [!IMPORTANT]
> Por predefinição, um ponto final de CDN do Azure está otimizado para DSA ignora cabeçalhos cache-diretiva e ignora a colocação em cache. Pode ajustar a forma como um ponto final de CDN do Azure trata estes cabeçalhos utilizando CDN regras a colocação em cache para ativar a colocação em cache. Para obter mais informações, consulte [CDN do Azure de controlo de colocação em cache comportamento com colocação em cache regras](cdn-caching-rules.md).

CDN do Azure suporta os seguintes cabeçalhos cache diretiva HTTP, definem a duração da cache e a partilha de cache: 

`Cache-Control`
- Introduzida no HTTP 1.1 para dar os publicadores web mais controlo sobre o conteúdo e a abordar as limitações do `Expires` cabeçalho.
- Substitui o `Expires` cabeçalho, se os dois-lo e `Cache-Control` são definidos.
- Quando utilizado no cabeçalho de pedido, `Cache-Control` é ignorada por CDN do Azure, por predefinição.
- Quando utilizado num cabeçalho de resposta, a CDN do Azure suporta as seguintes `Cache-Control` diretivas, de acordo com o produto: 
   - **CDN do Azure da Verizon**: suporta todas as `Cache-Control` diretivas. 
   - **CDN do Azure da Akamai**: suporta apenas o seguinte `Cache-Control` diretivas; todos os outros são ignorados: 
      - `max-age`: Uma cache pode armazenar o conteúdo para o número de segundos especificado. Por exemplo, `Cache-Control: max-age=5`. Esta diretiva Especifica a quantidade máxima de tempo que o conteúdo é considerado raiz.
      - `no-cache`: O conteúdo em cache, mas validar o conteúdo, sempre que a entrega de antes da cache. Equivalente ao `Cache-Control: max-age=0`.
      - `no-store`: Nunca cache o conteúdo. Remova o conteúdo se foi anteriormente armazenado.

`Expires`
- Cabeçalho legado introduzido no HTTP 1.0; suportado para efeitos de compatibilidade.
- Utiliza uma hora de expiração com base na data com precisão. 
- Semelhante ao `Cache-Control: max-age`.
- Utilizado quando `Cache-Control` não existe.

`Pragma`
   - Não cumpridas pela CDN do Azure, por predefinição.
   - Cabeçalho legado introduzido no HTTP 1.0; suportado para efeitos de compatibilidade.
   - Utilizado como um cabeçalho de pedido de cliente com a diretiva seguinte: `no-cache`. Esta diretiva dá instruções ao servidor para fornecer uma nova versão do recurso.
   - `Pragma: no-cache`é equivalente ao `Cache-Control: no-cache`.

## <a name="validators"></a>Validações

Quando a cache está obsoleta, validações da cache HTTP são utilizadas para comparar a versão em cache de um ficheiro com a versão no servidor de origem. **CDN do Azure da Verizon** suporta ETag e Last-Modified validações por predefinição, enquanto **CDN do Azure da Akamai** só suporta Last-Modified por predefinição.

`ETag`
- **CDN do Azure da Verizon** utiliza `ETag` por predefinição ao **CDN do Azure da Akamai** não.
- `ETag`Define uma cadeia que é exclusiva para cada ficheiro e a versão de um ficheiro. Por exemplo, `ETag: "17f0ddd99ed5bbe4edffdd6496d7131f"`.
- Introduzido no HTTP 1.1 e é mais atual que `Last-Modified`. Útil quando a data da última modificação é difícil determinar.
- Suporta tanto a validação segura, como a validação fraca; No entanto, a CDN do Azure suporta apenas forte validação. Para validação forte, representações dois recursos têm de ser para bytes idênticos. 
- Valida a uma cache de um ficheiro que utiliza `ETag` enviando um `If-None-Match` cabeçalho com um ou mais `ETag` validações no pedido. Por exemplo, `If-None-Match: "17f0ddd99ed5bbe4edffdd6496d7131f"`. Se corresponder a versão do servidor um `ETag` validador na lista, envia o código de estado 304 (não modificados) na respetiva resposta. Se for diferente da versão, o servidor responde com o código de estado 200 (OK) e o recurso atualizado.

`Last-Modified`
- Para **CDN do Azure da Verizon apenas**, Last-Modified é utilizada se ETag não faz parte da resposta HTTP. 
- Especifica a data e hora em que o servidor de origem tem determinou que o recurso foi modificado pela última vez. Por exemplo, `Last-Modified: Thu, 19 Oct 2017 09:28:00 GMT`.
- Valida a uma cache utilizando um ficheiro `Last-Modified` enviando um `If-Modified-Since` cabeçalho com uma data e hora no pedido. O servidor de origem compara essa data com o `Last-Modified` cabeçalho do recurso mais recente. Se o recurso não foi modificado desde a hora especificada, o servidor devolve o código de estado 304 (não modificados) na respetiva resposta. Se o recurso foi modificado, o servidor devolve o estado do código 200 (OK) e o recurso atualizado.

## <a name="determining-which-files-can-be-cached"></a>Determinar os ficheiros que podem ser colocadas em cache

Nem todos os recursos podem ser colocadas em cache. A tabela seguinte mostra os recursos que podem ser colocadas em cache, com base no tipo de resposta de HTTP. Não é possível colocar em cache recursos entregues com as respostas HTTP que não cumpram todas estas condições. Para **CDN do Azure da Verizon Premium** apenas, pode utilizar o motor de regras para personalizar algumas das seguintes condições.

|                   | CDN do Azure da Verizon | CDN do Azure da Akamai            |
|------------------ |------------------------|----------------------------------|
| Códigos de estado HTTP | 200                    | 200, 203, 300, 301, 302 e 401 |
| Método HTTP       | GET                    | GET                              |
| Tamanho dos ficheiros         | 300 GB                 | -Otimização de entrega de web geral: 1.8 GB<br />-Suporte de dados de transmissão em fluxo otimizações: 1.8 GB<br />-Otimização de ficheiros grandes: 150 GB |

## <a name="default-caching-behavior"></a>Comportamento de colocação em cache predefinido

A tabela seguinte descreve a predefinição comportamento para os produtos da CDN do Azure e os respetivos otimizações de colocação em cache.

|                    | Verizon - entrega web geral | Verizon – DSA | Akamai - entrega web geral | Akamai - DSA | Akamai - transferência de ficheiros grandes | Akamai - geral ou VOD suporte de dados de transmissão em fluxo |
|--------------------|--------|------|-----|----|-----|-----|
| **Origem de honor**   | Sim    | Não   | Sim | Não | Sim | Sim |
| **Duração da cache CDN** | 7 dias | Nenhum | 7 dias | Nenhum | 1 dia | um ano |

**Respeite origem**: Especifica se que respeite o [suportados cabeçalhos cache-diretiva](#http-cache-directive-headers) caso existam na resposta HTTP do servidor de origem.

**Duração da cache CDN**: Especifica a quantidade de tempo que um recurso é colocado em cache na CDN do Azure. No entanto, se **honrar origem** é Sim e a resposta HTTP do servidor de origem inclui o cabeçalho de directiva cache `Expires` ou `Cache-Control: max-age`, CDN do Azure utiliza o valor de duração especificado pelo cabeçalho em vez disso. 

## <a name="next-steps"></a>Passos Seguintes

- Para saber como personalizar e substituir a predefinição do comportamento da CDN através de regras a colocação em cache a colocação em cache, consulte [CDN do Azure de controlo de colocação em cache comportamento com colocação em cache regras](cdn-caching-rules.md). 
- Para saber como utilizar cadeias de consulta para controlar a colocação em cache comportamento, consulte [CDN do Azure de controlo de colocação em cache comportamento com cadeias de consulta](cdn-query-string.md).



