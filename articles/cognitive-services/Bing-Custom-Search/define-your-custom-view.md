---
title: 'Pesquisa do Bing personalizada: Definir uma vista personalizada | Microsoft Docs'
description: Descreve como criar um site e os serviços de pesquisa vertical
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 8ffe3087df398d6310828e41d0c6992199fafbed
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352904"
---
# <a name="configure-your-custom-search-experience"></a>Configurar a sua experiência de pesquisa personalizada
Uma instância de pesquisa personalizada permite-lhe personalizar a experiência de pesquisa para incluir conteúdo apenas a partir de sites que os utilizadores mais importantes para si. Em vez de efetuar uma pesquisa de toda a web, o Bing procura apenas o setor de web que lhe interessa.
Para criar a vista personalizada da web, utilize a pesquisa de personalizados do Bing [portal](https://customsearch.ai). Para obter informações sobre o início de sessão portal, consulte [criar a primeira instância de pesquisa do Bing personalizada](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/quick-start). O portal permite-lhe criar uma instância de pesquisa que especifica os domínios, subpages e páginas Web que pretende que o Bing para pesquisar e aqueles que não pretende que procura. Para além de especificar os URLs do conteúdo que sabe mais sobre, também pode colocar o portal sugerir conteúdo que pretende adicionar à vista. Seguem-se as opções que pode definir um setor da web: 

1.  Domínio. Um setor de domínio inclui todo o conteúdo localizado dentro de um domínio de internet. Por exemplo, www.microsoft.com. Omitindo 'www' faz com que o Bing para pesquisar também subdomínios do domínio. Por exemplo, se especificar microsoft.com, o Bing devolve também os resultados de support.microsoft.com ou technet.microsoft.com.
2.  Subpage. Um setor subpage inclui todo o conteúdo localizado na subpage e caminhos abaixo do mesmo. Pode especificar um máximo de dois subpages no caminho. Por exemplo, www.microsoft.com/en-us/windows/ 
3.  Página Web. Um setor de página Web pode incluir apenas essa página Web numa pesquisa personalizada. Opcionalmente, pode especificar se pretende incluir subpages.

Todos os domínios, subpages e páginas Web que especificar tem de ser público e indexada ao Bing. Se possuir um site público que pretende incluir na pesquisa e, ainda não indexada Bing-lo, consulte o Bing [documentação webmaster](https://www.bing.com/webmaster/help/webmaster-guidelines-30fba23a) para obter detalhes sobre o Bing para índice-lo a obter. Além disso, consulte a documentação de webmaster para obter detalhes sobre como obter o Bing para atualizar o seu site crawled se o índice está desatualizado.

## <a name="adding-slices-to-your-custom-search"></a>A adição de setores a sua pesquisa personalizada
Quando definir a instância de pesquisa personalizada, especifique os domínios do Active Directory e bloqueados, subpages e páginas Web que pretende procurar ou procura.  

- Active Directory: Uma lista de domínios, subpages ou páginas Web para incluir na pesquisa. 
- Bloqueada: Uma lista de domínio, subpages ou páginas Web para excluir da pesquisa. Os itens que blocos devem ser conteúdo encontrada nos domínios e subpages listadas na sua lista de Active Directory.

Para aceder a cada lista, clique nos separadores do Active Directory e bloqueado na sua instância de pesquisa personalizada. 

<a name="active-and-blocked-lists"></a>
## <a name="active-and-blocked-lists"></a>Active Directory e listas de bloqueado 
Para especificar um setor de web que pretende que o Bing para pesquisar, clique em de **Active Directory** separador e lista os domínios, subpages e páginas Web para procurar. Pode adicionar um setor diretamente à lista ou adicione mais do que um setor através do carregamento de um ficheiro de texto utilizando o ícone de carregamento.

Detalhes do carregamento de ficheiros: 

- Carregamento de ficheiros só está disponível para setores a adicionar à lista de Active Directory, não é possível utilizá-lo para adicionar setores à lista de bloqueado. 
- Criar um ficheiro de texto e especificar um único domínio, subpage ou página Web por linha. O carregamento completo é rejeitado se ocorrer um erro. 
- Se a lista de bloqueado contém o domínio, subpage ou página Web que especificou no ficheiro de carregamento, o serviço remove-o da lista bloqueado e adiciona-o à lista de Active Directory. 
- O serviço ignora duplicados no ficheiro de carregamento.

Para editar ou eliminar setores, utilize as opções sob a coluna de controlos. 

Da mesma forma, pode adicionar setores à lista de bloqueado (exceto não é possível utilizar um ficheiro de carregamento para especificar os setores).

## <a name="pinned-list"></a>Lista afixada
O portal também lhe permite afixar uma página Web específica na parte superior do resultado da pesquisa, se o utilizador introduzir um termo de pesquisa específico. O **Pinned** separador contém uma lista de pares de prazo e de página Web de consulta que especificam a página Web que é apresentado como o resultado superior para uma consulta específica. O termo de consulta do utilizador têm de corresponder exatamente o termo de consulta afixados.
Para obter informações sobre a afixação de resultados, consulte [ajustar classificação](#adjustrank).

Resultados de afixação não está disponível para a experiência de pesquisa de imagem.

## <a name="site-suggestions"></a>Sugestões de site
Depois de adicionar setores à lista de Active Directory, o serviço gera sugestões de site e subpage que pretenda adicionar à sua pesquisa. O **pode querer adicionar** secção contém as sugestões. A página de definições de instância inclui esta secção só se sugestões estão disponíveis. 

Para adicionar sugestões à sua lista de Active Directory, clique o + ícone.  Porque o serviço gera sugestões com base nas suas definições, lembre-se de que clique **atualizar** depois de adicionar as sugestões. 

## <a name="preview-pane"></a>Painel de pré-visualização
Pode testar a saída da sua instância de pesquisa através do painel de pré-visualização no lado direito para submeter consultas de pesquisa e ver os resultados. Selecione **instância My**, selecione um filtro de pesquisa seguro e o que mercado para pesquisar (consulte [parâmetros de consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters). Introduza uma consulta e prima introduza ou clique no ícone de pesquisa para ver os resultados da configuração atual. Para ver os resultados de web clique **Web**, para ver os resultados de imagem clique **imagem**. 

 Utilizar o painel de pré-visualização, também pode rever os resultados do Bing selecionando **Bing** em vez de **instância My**. Isto pode ser útil para comparar os resultados da sua experiência de pesquisa para os resultados devolvidos pelo Bing.

<a name="adjustrank"></a>
## <a name="adjust-rank"></a>Ajustar a classificação
O portal permite-lhe ajustar a classificação para manipular os resultados que o Bing devolve. No painel de pré-visualização, introduza um termo de pesquisa e executar a consulta. Painel de pré-visualização listar o resultado da pesquisa para a consulta. À direita da cada resultado se a lista de ajustes, pode efetuar. 

- Bloco. Move o domínio, subpage ou página Web para a lista de bloqueado. Selecione o nível para bloquear. Bing exclui a gravação de conteúdo o site selecionado nos resultados da pesquisa. 
- Melhorar. Conteúdo de boosts do domínio ou subpage superior nos resultados da pesquisa. Selecione se pretende melhorar conteúdo do domínio ou subpage a que pertence a página Web.
- Despromova. Despromove conteúdo a partir do domínio ou subpage inferior nos resultados da pesquisa. Selecione se pretende despromover o conteúdo do domínio ou subpage a que pertence a página Web. 
- PIN ao topo. Defina a página Web que aparece na parte superior dos resultados, se a consulta do utilizador, o termo de consulta corresponde exatamente a termo que utilizou. A lista de Active Directory não tem de conter a página Web por si para afixá-lo. 

Ajustar a classificação não está disponível para a experiência de pesquisa de imagem.

## <a name="boosting-and-demoting"></a>Os aumentos e despromover
Super pode melhorar, melhorar, ou despromover qualquer domínio ou subpage na sua lista de Active Directory. Por predefinição, todos os setores são adicionados com a mesma ponderação. Itens que sejam Super elevadas ou elevadas estão ordenadas superior nos resultados da pesquisa (com a classificação de aumento super superior ao aumento). Os itens que forem despromovidos estão ordenados inferior nos resultados da pesquisa.

É importante ter em atenção que super melhorar, melhorar e despromover variantes de ponderação respetiva de conceder a domínios ou subpages. Esta é apenas um dos muitos sinais utilizadas pelo ranker para determinar a ordem dos resultados. Isto significa que o respetivo efeito para uma consulta específica não é garantido como muitos outros fatores podem influenciar a classificação geral dos resultados da web.  Para determinar o efeito possíveis que os aumentos ou despromover tem sobre o teste ranker, a experiência de pesquisa através do painel de pré-visualização.

Super pode melhorar, melhorar, ou despromover itens ao utilizar os controlos de classificação ajustar na lista de Active Directory ou utilizando o aumento e despromover os controlos no painel de pré-visualização. O serviço adiciona o setor à sua lista de Active Directory e ajusta a classificação em conformidade.

Melhorar Super, melhorar e despromover alterações são guardadas automaticamente e refletir instantaneamente contra o ponto final de pesquisa personalizada. 

Melhorar Super, melhorar e despromover não estão disponíveis para a experiência de pesquisa de imagem.

## <a name="pin-to-top"></a>Afixar à parte superior
Para afixar uma página Web na parte superior dos resultados da pesquisa para uma consulta específica, escolha uma das seguintes opções:

1.  No separador de Pinned, introduza o URL da página Web para afixar na parte superior dos resultados e a consulta exata que vai acionar a afixação. 
2.  No painel de pré-visualização, introduza um termo de consulta e clique em Procurar. Em seguida, identifique a página Web nos resultados de que pretende afixar na parte superior dos resultados, se o utilizador introduz a mesma consulta. Em seguida, clique em afixar superior. O serviço adiciona a página Web e a consulta à lista Pinned. 

Pode controlar os pins no separador de Pinned. Os pins são apresentados como\<consulta\>, \<página Web\>' pares. 

A página Web é afixada apenas se a sua consulta corresponde exatamente a consulta do utilizador. 

Para uma consulta específica, pode afixar um máximo de uma página Web na parte superior dos resultados.

PINs não estão disponíveis para a experiência de pesquisa de imagem.

## <a name="use-bing-to-specify-slices"></a>Utilizar o Bing para especificar setores
Existem duas formas para especificar os setores do web que compõem a sua pesquisa personalizada. Se souber os setores de que pretende incluir na sua instância, adicione-o à lista de Active Directory da sua instância. Para obter informações sobre como adicionar itens à lista de Active Directory por si, consulte [listas de Active Directory e bloqueado](#active-and-blocked-lists).
Mas, se não tiver a certeza de que os setores a incluir, pode executar consultas de Bing no painel de pré-visualização e ver que o Bing devolve. Em seguida, pode selecionar os setores de que pretende incluir na sua pesquisa personalizada. Provavelmente, tem de executar vários termos de consulta para se certificar de que identifica a todos os setores que pretende para a instância. 

Siga estes passos para utilizar o Bing para adicionar setores à sua instância de pesquisa personalizada. 
1.  Inicie sessão na pesquisa do Bing personalizada [portal](https://customsearch.ai).
2.  Criar uma instância ou selecione uma instância a atualizar.
3.  No painel de pré-visualização no lado direito, selecione o Bing, na lista pendente.
4.  Na caixa de pesquisa, introduza um termo de consulta que são relevante para a instância.
5.  Clique em **Adicionar site** junto o resultado que pretende incluir.
6.  Clique no botão **OK**.

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

## <a name="view-statistics"></a>Estatísticas de vista
Se subscrever personalizada pesquisa ao nível do adequado (consulte o [preços páginas](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), um **estatísticas** separador é adicionado à sua instâncias de produção. O separador de estatísticas mostra detalhes sobre como são utilizados pontos finais da sua pesquisa personalizada, incluindo o volume de chamadas, consultas superiores, distribuição geográfica, códigos de resposta e pesquisa segura. Pode filtrar os detalhes de utilização de controlos fornecidos.

## <a name="understanding-quota"></a>Quota de compreender
- Para cada instância de pesquisa personalizada, o número máximo de ajustes de classificação que pode efetuar **Active Directory** e **bloqueado** setores está limitado a 400.
- A adição de um setor para os separadores de Active Directory ou bloqueado conta como uma classificação ajuste.
- Os aumentos e despromover contagem como dois ajustes de classificação.
- Para cada instância de pesquisa personalizada, o número máximo de pins que poderão tornar está limitado a 200.

## <a name="next-steps"></a>Passos Seguintes

- [Chamar a pesquisa personalizada](./search-your-custom-view.md)
- [Configurar a sua experiência de IU alojada](./hosted-ui.md)
- [Utilizar marcadores decoration para realçar texto](./hit-highlighting.md)
- [Páginas de página Web](./page-webpages.md)