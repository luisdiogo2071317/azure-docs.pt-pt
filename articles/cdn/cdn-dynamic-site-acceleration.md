---
title: Aceleração dinâmicas do site através da CDN do Azure
description: CDN do Azure suporta otimização de aceleração (DSA) dinâmicas do site para os ficheiros com conteúdo dinâmico.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2018
ms.author: rli; v-deasim
ms.openlocfilehash: 66032a68634be5b52bbfa544d0a3bcd3d1090652
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="dynamic-site-acceleration-via-azure-cdn"></a>Aceleração dinâmicas do site através da CDN do Azure

Com explosão de redes sociais, comércio eletrónico e web hyper personalizadas, é gerada uma percentagem do conteúdo servido aos utilizadores finais rapidamente aumentar em tempo real. Os utilizadores esperam que uma experiência de web personalizado, rápida e fiável, independentemente do respetivo browser, localização, dispositivo ou rede. No entanto, as inovações muito que fará com que estes experiências para exercer também lenta transferências de página e colocar a qualidade da experiência do consumidor em risco. 

Capacidade de entrega de conteúdos padrão de rede (CDN) inclui a capacidade de ficheiros de cache próximo aos utilizadores finais para acelerar a entrega de ficheiros estáticos. No entanto, com aplicações web dinâmicas, esse conteúdo em localizações de limite a colocação em cache não é possível porque o servidor gera o conteúdo em resposta ao comportamento de utilizador. Acelerar o fornecimento desse conteúdo for mais complexo do que a colocação em cache de limite tradicional e necessita de uma solução ponto-a-ponto que lhe tunes cada elemento ao longo do caminho de dados completo de inception para entrega. Com a otimização de aceleração (DSA) do Azure CDN dinâmicas do site, o desempenho da sua idade aumenta de web com o conteúdo dinâmico measurably foi melhorado.

**CDN do Azure da Akamai** e **CDN do Azure da Verizon** ambos oferecem otimização DSA através de **otimizado para** menu durante a criação do ponto final.

> [!Important]
> Para **CDN do Azure da Akamai** perfis, pode alterar a otimização de um ponto final de CDN após ter sido criado.
>   
> Para **CDN do Azure da Verizon** perfis, não é possível alterar a otimização de um ponto final de CDN depois de terem sido criadas.

## <a name="cdn-endpoint-configuration-to-accelerate-delivery-of-dynamic-files"></a>Configuração de ponto final da CDN para acelerar a entrega de ficheiros dinâmicos

Para configurar um ponto final da CDN para otimizar a entrega de ficheiros dinâmicos, pode utilizar o portal do Azure, as APIs REST ou qualquer um dos SDKs de cliente para efetuar a mesma coisa através de programação. 

**Para configurar um ponto final da CDN para a otimização de DSA utilizando o portal do Azure:**

1. No **perfil da CDN** página, selecione **Endpoint**.

   ![Adicionar um novo ponto final CDN](./media/cdn-dynamic-site-acceleration/cdn-endpoint-profile.png) 

   O painel **Adicionar um ponto final** aparece.

2. Em **otimizado para**, selecione **aceleração dinâmicas do site**.

    ![Criar um novo ponto final CDN com DSA](./media/cdn-dynamic-site-acceleration/cdn-endpoint-dsa.png)

3. Para **caminho de pesquisa**, introduza um caminho válido para um ficheiro.

    Caminho de pesquisa é uma funcionalidade específica DSA e é necessário para a criação de um caminho válido. DSA utiliza uma pequena *caminho de pesquisa* ficheiro colocada no servidor de origem para otimizar o encaminhamento as configurações de rede para a CDN. O ficheiro de caminho de pesquisa, pode transferir e carregar o ficheiro de exemplo para o seu site ou utilizar um recurso existente na sua origem que está prestes a 10 KB de tamanho.

4. Introduza as outras opções de ponto final necessárias (para obter mais informações, consulte [criar um novo ponto final da CDN](cdn-create-new-endpoint.md#create-a-new-cdn-endpoint)), em seguida, selecione **adicionar**.

   Após a criação do ponto final de CDN, aplica-se as otimizações de DSA para todos os ficheiros que correspondem a determinados critérios. 


**Para configurar um ponto final existente para DSA (CDN do Azure da Akamai perfis apenas):**

1. No **perfil da CDN** página, selecione o ponto final que pretende modificar.

2. No painel esquerdo, selecione **otimização**. 

   O **otimização** é apresentada a página.

3. Em **otimizado para**, selecione **aceleração dinâmicas do site**, em seguida, selecione **guardar**.

> [!Note]
> DSA incorreu encargos adicionais. Para obter mais informações, consulte [preços de rede de entrega de conteúdos](https://azure.microsoft.com/pricing/details/cdn/).

## <a name="dsa-optimization-using-azure-cdn"></a>Otimização do DSA utilizando CDN do Azure

Aceleração de Site dinâmico na CDN do Azure acelera a entrega de elementos dinâmicos utilizando as seguintes técnicas:

-   [Otimização de rota](#route-optimization)
-   [Otimizações de TCP](#tcp-optimizations)
-   [Obtenção prévia do objeto (CDN do Azure da Akamai apenas)](#object-prefetch-azure-cdn-from-akamai-only)
-   [Compressão de imagem adaptável (CDN do Azure da Akamai apenas)](#adaptive-image-compression-azure-cdn-from-akamai-only)

### <a name="route-optimization"></a>Otimização de rota

Otimização de rota é importante porque a Internet é um local dinâmico, onde o tráfego e temporariamente falhas estão constantemente a topologia da rede. O Border Gateway Protocol (BGP) é o protocolo de encaminhamento de Internet, mas poderá rotas mais rápidas através de servidores de ponto de presença (PoP) intermediário. 

Otimização de rota escolhe o caminho ideal para a origem para que um site continuamente está acessível e dinâmico conteúdo é entregue aos utilizadores finais através da rota mais rápido e mais fiável possíveis. 

A rede da Akamai utiliza técnicas para recolher dados em tempo real e comparar vários caminhos através de diferentes nós no servidor de Akamai, bem como a rota BGP predefinida através da Internet aberta para determinar a rota mais rápida entre a origem e o limite CDN. Estes técnicas evitar Internet pontos de congestionamento e rotas de comprimento. 

Da mesma forma, as utilizações de rede da Verizon uma combinação de Anycast DNS, elevada capacidade suportam PoPs e verificações de estado de funcionamento para determinar os melhor gateways para dados da melhor rota do cliente para a origem.
 
Como resultado, completamente dinâmico e transacional conteúdo é entregue mais rapidamente e mais fiável para os utilizadores finais, mesmo quando é uncacheable. 

### <a name="tcp-optimizations"></a>Otimizações de TCP

Protocolo de controlo de transmissão (TCP) é o padrão do conjunto de protocolos de Internet utilizado para fornecer informações entre aplicações numa rede IP.  Por predefinição, vários pedidos back-e-estabelecido são necessários para configurar uma ligação de TCP, bem como os limites para evitar congestions de rede, o que resultam numa inefficiencies à escala. **CDN do Azure da Akamai** processa este problema por otimizar em três áreas: 

 - [Eliminando início lento de TCP](#eliminating-tcp-slow-start)
 - [Tirar partido das ligações persistentes](#leveraging-persistent-connections)
 - [Parâmetros de pacotes TCP de otimização](#tuning-tcp-packet-parameters)

#### <a name="eliminating-tcp-slow-start"></a>Eliminando início lento de TCP

TCP *lenta início* é um algoritmo do protocolo TCP que impede o congestionamento de rede ao limitar a quantidade de dados enviados através da rede. Começa com o tamanho de janela de congestionamento pequeno entre remetente e o recetor até que o máximo é alcançado ou perda de pacotes é detetada.

 Ambos **CDN do Azure da Akamai** e **CDN do Azure da Verizon** perfis eliminar início lento de TCP com os seguintes três passos:

1. Estado de funcionamento e a monitorização de largura de banda é utilizada para medir a largura de banda das ligações entre servidores de PoP edge.
    
2. As métricas são partilhadas entre servidores do edge PoP, para que cada servidor tem conhecimento das condições de rede e estado de funcionamento do servidor dos outros PoPs em torno deles.  
    
3. Os servidores de limite CDN tornar pressupostos sobre alguns parâmetros de transmissão, por exemplo, o que o tamanho ideal da janela deve ser ao comunicar com outros servidores edge CDN respetiva proximidade. Este passo significa que o tamanho da janela de congestionamento inicial pode ser aumentado, se o estado de funcionamento da ligação entre os servidores de limite CDN é capaz de transferências de dados de pacotes superiores.  

#### <a name="leveraging-persistent-connections"></a>Tirar partido das ligações persistentes

Utilizar uma CDN, menos máquinas exclusivas ligam ao seu servidor de origem em comparação comparada diretamente com os utilizadores ligados diretamente para a origem. CDN do Azure também agrupamentos pedidos de utilizador em conjunto para estabelecer ligações menos com a origem.

Como anteriormente mencionadas vários pedidos de handshake são necessários para estabelecer uma ligação de TCP. Ligações persistentes, que são implementadas pelo `Keep-Alive` cabeçalho de HTTP, a reutilização de ligações TCP para vários pedidos HTTP guardar vezes reportadas round-trip e acelerar a entrega existentes. 

**CDN do Azure da Verizon** também envia pacotes ligação keep-alive periódicas através da ligação de TCP para impedir que uma ligação aberta de que está a ser fechado.

#### <a name="tuning-tcp-packet-parameters"></a>Parâmetros de pacotes TCP de otimização

**CDN do Azure da Akamai** tunes os parâmetros que regulam as ligações de servidor para servidor e reduz a quantidade de ações de ida longa haul necessária para obter o conteúdo incorporado no site utilizando as seguintes técnicas:

- Aumentar a janela de congestionamento inicial, para que mais pacotes podem ser enviados sem aguardar por uma confirmação.
- Diminuir o tempo limite de retransmitir inicial, para que seja detetada uma perda e retransmissão ocorre mais rapidamente.
- Diminuir o tempo limite mínimo e máximo retransmitir para reduzir o tempo de espera antes de partindo do princípio de pacotes foram perdidos na transmissão.

### <a name="object-prefetch-azure-cdn-from-akamai-only"></a>Obtenção prévia do objeto (CDN do Azure da Akamai apenas)

A maioria dos Web sites é constituída por uma página HTML, que referencia vários outros recursos, tais como imagens e scripts. Normalmente, quando um cliente solicita uma página Web, o browser transfere primeiro e analisa o objeto HTML e, em seguida, efetua pedidos adicionais aos elementos ligados que são necessárias para totalmente carregar a página. 

*Obtenção prévia* é uma técnica para obter imagens e scripts incorporada numa página HTML enquanto o HTML é fornecido para o browser e antes do browser mesmo torna estes pedidos de objeto. 

Com a opção de obtenção prévia ativou o momento quando a CDN serve página HTML base para o browser do cliente, a CDN e analisa o ficheiro HTML efetuam pedidos adicionais para todos os recursos ligados e armazene-o na respetiva cache. Quando o cliente efetua os pedidos para os recursos ligados, o servidor edge CDN é já tem os objectos solicitados e pode servi-los imediatamente sem uma ida e volta para a origem. Esta otimização beneficia conteúdos colocáveis e não colocáveis.

### <a name="adaptive-image-compression-azure-cdn-from-akamai-only"></a>Compressão de imagem adaptável (CDN do Azure da Akamai apenas)

Alguns dispositivos, especialmente móveis aqueles, experiência de tempos a tempos mais lentas velocidades de rede. Nestes cenários, é mais vantajoso para ao utilizador receber mais rapidamente imagens inferior na sua página Web em vez de esperar muito tempo para imagens de resolução completa.

Esta funcionalidade automaticamente monitoriza qualidade da rede e utiliza os métodos padrão de compressão de JPEG quando velocidades de rede são mais lentas melhorar a hora de entrega.

Compressão de imagem adaptável | Extensões de ficheiro  
--- | ---  
Compressão de JPEG | . jpg, JPEG, .jpe, .jig, .jgig, .jgi

## <a name="caching"></a>Colocação em cache

Com DSA, colocação em cache está desativada por predefinição na CDN, mesmo quando a origem inclui `Cache-Control` ou `Expires` cabeçalhos na resposta. DSA é normalmente utilizado para ativos dinâmicos que devem não ser colocados em cache porque são exclusivos para cada cliente. A colocação em cache, pode interromper este comportamento.

Se tiver um Web site com uma combinação de recursos dinâmicos e estáticos, é melhor adotar uma abordagem de híbrida para obter o melhor desempenho. 

Para **CDN do Azure Standard da Verizon** e **CDN do Azure Standard da Akamai** perfis, pode ativar a colocação em cache para os pontos finais DSA utilizando [regras a colocação em cache](cdn-caching-rules.md).

A colocação em cache as regras de acesso:

1. Do **perfil da CDN** página, em definições, selecionadas **regras a colocação em cache**.  
    
    ![Botão Regras de colocação em cache da CDN](./media/cdn-dynamic-site-acceleration/cdn-caching-rules-btn.png)

    O **regras a colocação em cache** é aberta a página.

2. Crie uma regra de colocação em cache global ou personalizada para ativar a colocação em cache para o ponto final DSA. 

Para **CDN do Azure Premium da Verizon** perfis, ativar a colocação em cache para os pontos finais DSA utilizando o [motor de regras](cdn-rules-engine.md). Quaisquer regras que são criadas afetam apenas esses pontos finais do seu perfil que estão otimizados para DSA. 

Para o motor de regras de acesso:
    
1. Do **perfil da CDN** página, selecione **gerir**.  
    
    ![Botão de gerir do perfil de CDN](./media/cdn-dynamic-site-acceleration/cdn-manage-btn.png)

    É aberto o portal de gestão do CDN.

2. No portal de gestão CDN, selecione **ADN**, em seguida, selecione **motor de regras**. 

    ![Motor de regras para DSA](./media/cdn-dynamic-site-acceleration/cdn-dsa-rules-engine.png)



Em alternativa, pode utilizar dois pontos finais da CDN: um ponto final otimizada com DSA para fornecer recursos dinâmicos e de outro ponto final otimizada com um tipo de otimização estáticos, tais como geral entrega web, a recursos colocáveis de entrega. Modificar o URL de página Web para ligar diretamente para o elemento no ponto final da CDN que planeia utilizar. 

Por exemplo: `mydynamic.azureedge.net/index.html` é uma página dinâmica e é carregada a partir do ponto final de DSA.  Página html faz referência a vários recursos estáticos, tais como bibliotecas de JavaScript ou imagens que são carregadas a partir estático ponto final de CDN, tais como `mystatic.azureedge.net/banner.jpg` e `mystatic.azureedge.net/scripts.js`. 



