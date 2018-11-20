---
title: Definir uma vista personalizada - pesquisa personalizada do Bing
titlesuffix: Azure Cognitive Services
description: Descreve como criar sites e serviços de pesquisa vertical
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 09/28/2017
ms.author: aahi
ms.openlocfilehash: ab1b8b011a6523fe3760c233c83d59fd9768ad0c
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/20/2018
ms.locfileid: "52163305"
---
# <a name="configure-your-custom-search-experience"></a>Configurar a sua experiência de pesquisa personalizada

Uma instância de pesquisa personalizada permite-lhe personalizar a experiência de pesquisa para incluir conteúdo apenas a partir de sites que se preocupam com os seus utilizadores. Em vez de efetuar uma pesquisa de toda a web, o Bing procura apenas o setor da web que lhe interessa. Para criar a sua vista personalizada da Web, utilize a Pesquisa Personalizada do portal [Bing](https://customsearch.ai). Para obter informações sobre o início de sessão no portal, consulte [criar a primeira instância de pesquisa personalizada do Bing](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/quick-start). 

O portal permite-lhe criar uma instância de pesquisa que especifica os domínios, subpáginas e páginas da Web que pretende que o Bing para pesquisar e aqueles que não quê-lo para pesquisar. Além de especificar os URLs do conteúdo que conhece, também pode fazer o portal para sugerir conteúdos que pretende adicionar à sua exibição. 

Seguem-se das formas que pode definir um setor da web: 

1.  Domínio. Um setor de domínio inclui todo o conteúdo encontrado dentro de um domínio de internet. Por exemplo, www.microsoft.com. Omitir "www" faz com que o Bing para pesquisar também subdomínios do domínio. Por exemplo, se especificar microsoft.com, o Bing devolve também os resultados da support.microsoft.com ou technet.microsoft.com.  
  
2.  Subpáginas. Um setor de subpáginas inclui todo o conteúdo encontrado na subpáginas e caminhos abaixo dele. Pode especificar um máximo de dois subpáginas no caminho. Por exemplo, www.microsoft.com/en-us/windows/  
  
3.  Página Web. Um setor de página da Web pode incluir apenas essa página da Web numa pesquisa personalizada. Opcionalmente, pode especificar se pretende incluir subpáginas.

Todos os domínios, subpáginas e páginas da Web que especificar tem de ser indexados pela Bing e públicas. Se tiver um site público que pretende incluir na pesquisa e, ainda não indexou Bing-lo, consulte o Bing [documentação webmaster](https://www.bing.com/webmaster/help/webmaster-guidelines-30fba23a) para obter detalhes sobre como obter o Bing para o indexar. Além disso, consulte a documentação de webmaster para obter detalhes sobre como obter o Bing para atualizar o seu site rastreada se o índice está desatualizado.

## <a name="adding-slices-to-your-custom-search"></a>Setores a adicionar a sua pesquisa personalizada

Quando define a sua instância de pesquisa personalizada, especifique os domínios do Active Directory e bloqueados, subpáginas e páginas da Web que deseja pesquisar ou procurar não.  

- Active Directory: Uma lista de domínios, subpáginas ou páginas da Web para incluir na pesquisa.  
  
- Bloqueada: Uma lista de domínio, subpáginas ou páginas da Web para excluir da pesquisa. Os itens que bloco deve ser conteúdo encontra-se nos domínios e subpáginas listados na sua lista de Active Directory.

Para aceder a cada lista, clique nos separadores ativo e bloqueado na sua instância de pesquisa personalizada. 

<a name="active-and-blocked-lists"></a>
## <a name="active-and-blocked-lists"></a>Active Directory e listas de bloqueado 

Para especificar um setor da web que pretende que o Bing para pesquisar, clique a **Active Directory** separador e listar os domínios, subpáginas e páginas da Web para procurar. Pode adicionar um setor diretamente à lista ou adicionar mais do que um setor ao carregar um ficheiro de texto com o ícone de carregamento.

Detalhes de carregamento de ficheiros: 

- Carregamento de ficheiros está disponível apenas para setores a adicionar à lista Active Directory, não é possível usá-lo para adicionar setores à lista de bloqueado.  
  
- Crie um arquivo de texto e especifique um único domínio, subpáginas ou página Web por linha. O carregamento de inteiro será rejeitado se ocorrer um erro.  
  
- Se a lista de bloqueado contém o domínio, subpáginas ou página Web que especificou no ficheiro de carregamento, o serviço de remove-o na lista de bloqueado e o adiciona à lista Active Directory.  
  
- O serviço ignora duplicados no ficheiro de carregamento.

Para editar ou eliminar setores, utilize as opções sob o **controles** coluna. 

Da mesma forma, pode adicionar setores à lista de bloqueado (exceto não é possível utilizar um ficheiro de carregamento para especificar os setores).

## <a name="pinned-list"></a>Lista afixada

O portal permite-lhe ainda afixar uma página Web específica na parte superior do resultado da pesquisa, se o utilizador inserir um termo de pesquisa específico. O **Pinned** separador contém uma lista de pares de prazo e a página Web de consulta que especificam a página Web que é apresentado como o melhor resultado para uma consulta específica. Além disso, serão apresentadas apenas indexadas páginas da Web nas pesquisas. Para obter informações sobre como afixar os resultados, consulte [ajustar classificação](#adjustrank). 

Afixar os resultados não está disponível para a pesquisa de imagens e experiências de pesquisa de vídeo.

## <a name="website-suggestions"></a>Sugestões de site

Depois de adicionar setores à lista Active Directory, o serviço gera o Web site e subpáginas sugestões que pode querer adicionar a sua pesquisa. O **pode querer adicionar** secção contém as sugestões. A página de definições de instância inclui esta secção apenas se sugestões estão disponíveis. 

Para adicionar sugestões à sua lista de Active Directory, clique o ícone +.  Uma vez que o serviço gera sugestões com base nas suas definições, certifique-se de que clique em **Atualize** depois de adicionar as sugestões. 

## <a name="preview-pane"></a>Painel de visualização

Pode testar sua instância de pesquisa usando o painel de visualização no lado direito para enviar consultas de pesquisa e ver os resultados. 

1. Selecione **minha instância**
2. Selecione um filtro de pesquisa segura e o mercado para pesquisa (consulte [parâmetros de consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters)).
3. Introduza uma consulta e prima enter ou clique no ícone de pesquisa para ver os resultados da configuração atual. 

Para selecionar o tipo de pesquisa para executar, clique em **Web** para obter os resultados da web, **imagem** para obter resultados de imagem, ou **vídeo** para obter resultados de vídeo. 

Utilizar o painel de pré-visualização, poderá também ver resultados do Bing, selecionando **Bing** em vez de **minha instância**. Isso pode ser útil para comparar os resultados de sua experiência de pesquisa para os resultados devolvidos pela Bing.

<a name="adjustrank"></a>
## <a name="adjust-rank"></a>Ajustar a classificação

O portal permite-lhe ajustar a classificação para manipular os resultados que o Bing devolve. No painel pré-visualização, introduza um termo de pesquisa e execute a consulta. O painel de visualização listar o resultado da pesquisa para a consulta. À direita de cada resultado é a lista de ajustes, pode fazer. 

- Bloco. Move o domínio, subpáginas ou página Web para a lista de bloqueado. Selecione o nível para bloquear. Bing exclui o conteúdo do site selecionado nos resultados da pesquisa.  
  
- Aumente as. Aumenta a conteúdo a partir do domínio ou subpáginas superior nos resultados da pesquisa. Selecione se pretende aumentar o conteúdo do domínio ou subpage a que pertence a página Web. [Ler mais](#boosting-and-demoting)  
  
- Despromova. Despromove das conteúdo a partir do domínio ou subpáginas inferior nos resultados da pesquisa. Selecione se pretende despromover o conteúdo do domínio ou subpage a que pertence a página Web. [Leia mais](#boosting-and-demoting).  
  
- PIN para cima. Defina a página Web que aparece na parte superior dos resultados se corresponder a cadeia de consulta do utilizador com base na condição de correspondência do pin de cadeia de consulta dos pins. A lista de Active Directory não tem de conter a página Web que pode afixá-la. [Leia mais](#pin-to-top).

Ajustar a classificação não está disponível para a pesquisa de imagens e experiências de pesquisa de vídeo.

## <a name="boosting-and-demoting"></a>Aumentar e despromover

Super pode aumentar, melhorar, ou despromover qualquer domínio ou subpage na sua lista de Active Directory. Por predefinição, todos os setores são adicionados com a mesma importância. Itens que são muito elevados ou aumentados estão ordenados superior nos resultados da pesquisa (com a classificação de super boost superior boost). Itens que estão a ser despromovidos são uma classificação mais baixa nos resultados da pesquisa.

É importante observar que super aumentar, aumentar e despromover variantes de ponderação respetiva de conceder a domínios ou subpáginas. Isso é apenas um dos muitos sinais utilizados pelo classificador para determinar a ordem dos resultados. Isso significa que o seu efeito para uma consulta específica não é garantido como muitos outros fatores podem influenciar a classificação geral dos resultados da web.  Para determinar o efeito de possíveis esse adaptativo ou despromover tem sobre o teste classificador, a experiência de pesquisa através do painel de pré-visualização.

Super pode aumentar, melhorar, ou despromover itens com os controlos de classificação ajustar na lista Active Directory ou com o aumento e despromover os controles no painel de pré-visualização. O serviço adiciona o setor à sua lista de Active Directory e ajusta a classificação em conformidade.

Super aumentar, aumentar e despromover não estão disponíveis para as experiências de pesquisa de imagens e pesquisa de vídeos.

## <a name="pin-to-top"></a>Afixar ao início

Para afixar uma página Web à parte superior dos resultados da pesquisa para uma consulta específica, escolha uma das seguintes opções:

1.  Na **Pinned** separador, introduza o URL da página Web para afixar à parte superior dos resultados e a consulta que aciona a fixação.  
  
2.  Na **pré-visualização** painel, introduza um termo de consulta e clique em Procurar. Em seguida, identifique a página Web nos resultados de que pretende afixar na parte superior dos resultados, se o usuário inserir a mesma consulta. Em seguida, clique em **Pin ao início**. O serviço adiciona a página da Web e a consulta para o **Pinned** lista. 

Pode controlar seus pins no **Pinned** separador. Os pins são mostrados como\<consulta\>, \<página Web\>"pares. 

Para uma consulta específica, pode afixar um máximo de uma página da Web para a parte superior dos resultados.

PINs não estão disponíveis para a pesquisa de imagens e experiências de pesquisa de vídeo.

### <a name="specifying-the-pins-match-condition"></a>Especificar a condição de correspondência do pin

A página da Web está afixada na parte superior dos resultados apenas se a cadeia de consulta do utilizador corresponde à cadeia de consulta o pin com base na condição de correspondência do pin. Por predefinição, um pin é correspondido apenas se a cadeia de consulta o pin e de cadeia de consulta do utilizador corresponder exatamente. Pode alterar o comportamento predefinido ao especificar uma das seguintes condições de correspondência.

- Começa com &mdash; o pin é uma correspondência, se a cadeia de consulta do usuário começa com a cadeia de consulta o pin.
- Termina com &mdash; o pin é uma correspondência, se a cadeia de consulta do utilizador termina com a cadeia de consulta o pin.
- Contém &mdash; o pin é uma correspondência, se a cadeia de consulta do utilizador contém a cadeia de consulta o pin.

Para alterar a condição de correspondência do pin, clique no ícone de edição do pin (parece um lápis). Na **condição de correspondência de consulta** coluna, clique na lista pendente e selecione a nova condição a utilizar. Em seguida, clique em salvar ícone para guardar a alteração.

Todas as comparações diferenciam maiúsculas de minúsculas.

### <a name="changing-the-order-of-the-pins"></a>Alterar a ordem dos pinos

Para alterar a ordem de seus pins, pode arrastar-n-soltar o pin ou pode editar o pin e alterar o respetivo número de ordem. Para arrastar-n-soltar um pin, clique em Afixar na lista para mover, manter o botão do mouse deprimido e arraste o pin sobre o pin que pretende substituir e, em seguida, largue o botão do mouse. Tenha em atenção que o número de ordem do pin é alterada em conformidade.

Também pode editar o número da encomenda do pin. Na **controles** coluna, clique em ícone de edição do pin (parece um lápis). Introduza o número de ordem em que pretende que o pin a aparecer na lista e prima enter ou clique em salvar ícone. Por exemplo, se o pin é atualmente sexto na lista e desejar que ela seja segundo, altere o número de ordem para duas (2).

Se vários pins satisfaçam a condição de correspondência, a ordem dos pinos determina qual pin o Bing utiliza. Por exemplo, se pins dois e três satisfaçam a condição de correspondência, o Bing utiliza pin duas.

## <a name="use-bing-to-specify-slices"></a>Utilize o Bing para especificar os setores

Existem algumas maneiras de especificar os setores da web que compõem a sua pesquisa personalizada. Se souber os setores de que pretende incluir na sua instância, adicioná-los à sua instância **Active Directory** lista. Para obter informações sobre como adicionar itens para o **Active Directory** lista manualmente, consulte [listas de Active Directory e bloqueado](#active-and-blocked-lists).

Mas se não tiver a certeza de que setores para incluir, pode executar Bing consultas **pré-visualização** painel e ver o que o Bing devolve. Em seguida, pode selecionar os setores de que pretende incluir na sua pesquisa personalizada. É provável que precisa executar vários termos de consulta para se certificar de que identificar todos os setores de que pretende para a sua instância. 

Siga estas etapas para usar o Bing para adicionar setores à sua instância de pesquisa personalizada. 

1.  Entrar para a pesquisa personalizada do Bing [portal](https://customsearch.ai).
2.  Criar uma instância ou selecione uma instância para atualizar.
3.  No painel de pré-visualização no lado direito, selecione o Bing, na lista pendente.
4.  Na caixa de pesquisa, introduza um termo de consulta que é relevante para a sua instância.
5.  Clique em **Add site** junto ao resultado que pretende incluir.
6.  Clique no botão **OK**.

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

## <a name="view-statistics"></a>Ver estatísticas

Se subscrever a pesquisa personalizada ao nível apropriado (consulte a [páginas de preços](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), uma **estatísticas** separador é adicionado às instâncias de produção. O separador de estatísticas mostra detalhes sobre como os pontos de extremidade de pesquisa personalizada são usados, incluindo o volume de chamadas, principais consultas, distribuição geográfica, códigos de resposta e pesquisa segura. Pode filtrar detalhes usando os controles fornecidos.

## <a name="understanding-quota"></a>Quota de compreensão

- Para cada instância de pesquisa personalizada, o número máximo de ajustes de classificação que pode fazer **Active Directory** e **bloqueado** setores está limitado a 400.
- Adicionar um setor para os separadores do Active Directory ou bloqueado é contabilizado como uma classificação ajuste.
- Aumentar e despromover contagem como dois ajustes de classificação.
- Para cada instância de pesquisa personalizada, o número máximo de pins que pode fazer é limitado a 200.

## <a name="next-steps"></a>Passos Seguintes

- [Chamar a sua pesquisa personalizada](./search-your-custom-view.md)
- [Configurar a sua experiência de IU alojada](./hosted-ui.md)
- [Utilizar marcadores decorativos para realçar o texto](./hit-highlighting.md)
- [Paginação de páginas Web](./page-webpages.md)