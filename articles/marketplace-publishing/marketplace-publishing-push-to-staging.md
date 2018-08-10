---
title: Preparar e testar a sua oferta para a implementação no Azure Marketplace | Documentos da Microsoft
description: Obter instruções detalhadas sobre a fornecer conteúdo de marketing, configurar planos de preços e teste a sua oferta antes de implementar no Azure Marketplace.
services: marketplace-publishing
documentationcenter: ''
author: HannibalSII
manager: hascipio
editor: ''
ms.assetid: 3ccd2448-895b-477e-adf6-ab655a21d2fa
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/17/2016
ms.author: hascipio
ms.openlocfilehash: 7db86716cdf8f9eb921c3c1813970acae7a3016b
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39714964"
---
# <a name="complete-the-offer-creation-with-marketing-content"></a>Concluir a criação de oferta com conteúdo de marketing
Neste passo do processo de publicação, terá de fornecer determinados conteúdo de marketing e detalhes sobre a oferta de e/ou a SKUs no Azure Marketplace. Por exemplo, fornecerá uma descrição do seu produto, logótipos, planos de preços, os detalhes dos planos e outras informações necessárias para emitir a oferta de e/ou SKU para teste. Estas informações são utilizadas como conteúdo de marketing no portal do Azure. Começará esse processo no [portal de publicação][link-pubportal].

## <a name="step-1-provide-marketplace-marketing-content"></a>Passo 1: Fornecer conteúdo de marketing do Marketplace
**Inglês é o padrão e único idioma suportado.** Certifique-se de que todas as informações fornecidas nos campos estão em inglês. Todas as informações podem ser editadas em qualquer altura até emitir para a fase de testes.

1. Aceda ao portal de publicação [ https://publish.windowsazure.com ](https://publish.windowsazure.com).
2. No menu da esquerda, clique nas **Marketing** separador.
3. No painel principal, clique nas **inglês (EUA)** botão.
   
   > [!IMPORTANT]
   > Todos os campos devem conter entradas, incluindo as imagens, para que possa ser capaz de emitir para teste.
   > 
   > 

### <a name="details-and-plans"></a>Os planos e detalhes
1. Introduza o título da oferta (máximos de 50 caracteres), resumo (máximo de 100 carateres) da oferta, oferecem resumo longo (máximo de 256 carateres), oferecem descrição (máximo de 1300 de carateres), logótipos sob o **detalhes** separador
2. Introduza o título do plano (máximos de 50 caracteres), o resumo de plano (máximo de 100 carateres), planeie descrição (máximo de 2000 carateres) sob o **planos** separador.
   
   > [!NOTE]
   > Pode utilizar as seguintes tags de HTML para formatar o resumo, há muito tempo resumo e descrição da oferta e planos. As marcas HTML permitidas são h1, h2, h3, h4, h5, p, ol ul e li, um [destino | href] forte, e em, b, eu.
   > 
   > 
3. Não introduza texto duplicado na descrição da oferta e o plano.
4. Não introduza texto duplicado sob o título e o tempo de resumo de oferta do plano.
5. Não introduza texto duplicado no título do plano e oferecer resumo.
6. Não introduza títulos de plano idênticas para uma oferta com vários esquemas.
7. Carregar imagens das especificações necessárias (mencionadas no Portal de publicação) no formato PNG, uma para cada tamanho.
8. Certifique-se de que os logótipos seguem as diretrizes de logótipo do Azure Marketplace mencionadas a seguir.
   
   ![desenho](media/marketplace-publishing-push-to-staging/pubportal-marketingcontent-details-02.png)

**Diretrizes de logótipo do Azure Marketplace**

Todos os logótipos carregados no Portal de publicação devem seguir as diretrizes abaixo:

* O design do Azure tem uma paleta de cores simples. Mantenha o número de principal e secundário cores seu logótipo baixa.
* As cores de tema do portal do Azure são brancas e preto. Evite, por conseguinte, utilizar essas cores como a cor de fundo de suas logótipos. Utilize uma cor que tornaria os logótipos proeminente no portal do Azure. Recomendamos cores primárias simples. **Se estiver a utilizar o fundo transparente, em seguida, certifique-se de que o texto/logótipos não são branco ou preto ou azul.**
* Não utilize um plano de fundo de gradação o logótipo.
* Evite colocar texto, até mesmo sua empresa ou o nome da marca, sobre o logótipo. O aspeto e funcionalidade do seu logótipo deve ser "simples" e deve evitar gradientes.
* O logótipo não deverá ser ampliado.
* Logotipo pequeno deve ter um tamanho 40 X 40 px
* Logótipo médio deve ter um tamanho 90 X 90 px
* Logotipo grande deve ter um tamanho 115 X 115 px
* Logotipo grande deve ter um tamanho 255 X 115 px
* Logótipo de Hero deve ter um tamanho 815 X 290 px

> [!NOTE]
> O logótipo de Hero é opcional. O publicador pode optar por não carregue um logótipo de Hero. No entanto uma vez carregado o ícone de hero não é possível eliminar a publicação do portal. Nessa altura, o parceiro tem de seguir as diretrizes do Azure Marketplace para os ícones de Hero.
> 
> 

  ![desenho](media/marketplace-publishing-push-to-staging/pubportal-marketingcontent-details-03.png)

**Diretrizes adicionais para o ícone de logótipo Hero (opcional)**

* O logótipo de Hero é opcional. O publicador pode optar por não carregue um logótipo de Hero. **No entanto uma vez carregado o ícone de hero não é possível eliminar a publicação do portal. Nessa altura, o parceiro tem de seguir as diretrizes do Azure Marketplace para os ícones de Hero outras que a oferta não será aprovada para produção.**
* O nome de exibição do publicador, título de plano e a oferta longa resumo são apresentados na cor da fonte branco. Por isso, deve evitar manter qualquer cor leve no plano de fundo do ícone Hero. Preto, branco e transparente em segundo plano não é permitido para os ícones de Hero.
* O nome de exibição do publicador, título, a oferta resumo há muito tempo e no botão Criar incorporados por meio de programação dentro o logótipo de Hero depois de fica a oferta de plano listado. Portanto, não deve digitar qualquer texto durante a conceção o logótipo de Hero. Apenas deixe um espaço vazio à direita, uma vez que o texto (ou seja, publicador nome a apresentar, plano título, a oferta longa resumo) será incluído por meio de programação por nós por aí. O espaço vazio para o texto deve ser 415 x 100 à direita (e ele é contrabalançado por 370px da esquerda).
  
  ![desenho](media/marketplace-publishing-push-to-staging/pubportal-herobanner.png)

### <a name="links"></a>Ligações
Sobre o **Links** separador na barra esquerda, introduza as ligações com informações que podem ajudar os clientes. Introduza um nome e um URL para cada ligação.

![desenho](media/marketplace-publishing-push-to-staging/pubportal-marketingcontent-link-01.png)

### <a name="sample-images-optional"></a>Imagens de exemplo (opcionais)
> [!NOTE]
> A inclusão de uma imagem de exemplo é um passo opcional.
> Mesmo que pode carregar várias imagens de exemplo na publicação portal, apenas uma imagem (selecionada aleatoriamente pelo sistema) é apresentada no portal do Azure. Por esse motivo, recomendamos que carregar no máximo, uma imagem de exemplo.
> 
> 

Sobre o **imagens de exemplo** separador no menu da esquerda, carregue uma imagem nova ao clicar em **carregue uma nova imagem**. Se tiver uma imagem existente e pretende substituí-lo, clique em **Replace imagem**.

![desenho](media/marketplace-publishing-push-to-staging/pubportal-marketingcontent-sampleimg-01.png)

### <a name="legal"></a>Informações Legais
Sobre o **legais** separador, forneça uma ligação para as suas políticas/termos de utilização. Introduza ou cole os termos em grande escala **termos de utilização** caixa. O limite de carateres para os termos legais de utilização é de 1 000 000 carateres.

![desenho](media/marketplace-publishing-push-to-staging/pubportal-marketingcontent-legal-01.png)

**Nota:** para ofertas de Máquina Virtual, uma vez que uma oferta/SKU ser preparado no Portal do Azure, não é possível alterar os campos indicados abaixo:

* **Identificador da oferta:** [portal de publicação -> máquinas virtuais -> a sua oferta -> imagens de VM separador -> identificador oferecem]
* **Identificador SKU:** [portal de publicação -> máquinas virtuais -> selecione a sua oferta -> SKUs separador -> adicione um SKU]
* **Espaço de nomes do publicador:** [portal de publicação -> máquinas virtuais -> passo a passo guia -> diga-nos sobre a sua empresa (encontrada em "Passo 2 registar a sua empresa") -> espaço de nomes do publicador -> espaço de nomes]

Para ofertas de Máquina Virtual, uma vez que a oferta/SKU está listado no Azure Marketplace, não é possível alterar os campos indicados abaixo:

* **Identificador da oferta:** [portal de publicação -> máquinas virtuais -> selecione a sua oferta -> imagens de VM -> identificador oferecem]
* **Identificador SKU:** [portal de publicação -> máquinas virtuais -> selecione a sua oferta -> SKUs separador -> adicione um SKU]
* **Espaço de nomes do publicador:** [portal de publicação -> máquinas virtuais -> passo a passo guia -> espaço de nomes de publicador de dizer-nos sobre a sua empresa (encontrada no passo 2 registar) -> espaço de nomes]
* **Portas:** [portal de publicação -> máquinas virtuais -> a sua oferta -> imagens de VM separador -> portas abertas]
* **Alteração de preços dos SKU (s) listados**
* **Alteração de SKU (s) listados do modelo de faturação**
* **Remoção de regiões de SKU (s) listados de faturação**
* **Alterar o número de discos de dados de SKU (s) listados**

## <a name="step-2-set-your-prices"></a>Passo 2: Definir os preços
### <a name="pricing-models"></a>Modelos de preços
| Modelo de determinação de preço | Descrição |
| --- | --- |
| Base |Taxa mensal fixa pagos no momento da compra; Por exemplo, US $10/ mês. |
| Consumo (também conhecido como utilização, medidor) |Paga por utilização, que é definida pelo editor da oferta. Utilização excedida não pode ser definida por posto de trabalho, por utilizador, etc., porque não existe nenhum conceito de uma fração de um utilizador ou a capacidade de fazer o rateio. Utilização é comunicada pelo parceiro numa base horária. Cliente paga no do ciclo de faturação mensal, em vez de com antecedência like planos mensais. |
| Avaliação gratuita |Cliente pode utilizar gratuitamente, para um período limitado e, em seguida, posteriormente, a pagar as tarifas normais. |
| Escalão gratuito |Plano é sempre gratuito. |
| Migração (também conhecido como conversão ou atualização/mudança para versão anterior) do plano |Conceito de um utilizador mudar de seu plano atual para outro plano aceitável; definido pelo parceiro. |

**Os modelos disponíveis por tipo de oferta de preços**

> [!IMPORTANT]
> Disponibilidade de determinados modelos de preços variam consoante o tipo de oferta. Veja a tabela abaixo.
> 
> 

|  | Apenas base | Consumo apenas | Base + consumo |
| --- | --- | --- | --- |
| Imagem da máquina virtual |Não |Sim |Não |
| Serviço de programador |Sim |Sim |Sim |

### <a name="21-set-your-vm-prices"></a>2.1. Definir os preços VM
Atualmente para máquinas virtuais, temos o seguinte **3 tipos de modelos de faturação:**

* **Hora a hora:** clientes cobrados numa base por hora com base nas tarifas definidas pelos publicadores sobre os tamanhos VM. No caso de **faturação à hora** modelo dos SKUs, o preço total será a soma do custo do software cobrado pelo fabricante e o custo de infraestrutura cobrada pela Microsoft. Esse custo total será apresentado ao cliente como um preço por hora e mensal quando estiver a considerar a compra (veja a captura de ecrã abaixo). **Publicador recebe 80% do custo do software cobrado por eles.** O problema, por conseguinte, o cálculo em conformidade antes de definir os preços para os SKUs de marca.
  
    ![desenho](media/marketplace-publishing-push-to-staging/img2.1-01.png)
* **Versão de avaliação gratuita:** este é outro sabor do modelo por hora. Aqui, o cliente não obter cobrado por custo de software para os primeiros 30 days(Free) depois de implementar a VM. Depois de 30days eles serão cobrados numa base por hora com base nas tarifas definidas pelos publicadores no modelo por hora.
* **Bring-Your-própria licença (BYOL):** os publicadores gerir o licenciamento do software em execução na VM.

**Importante:** assim que a oferta/SKU está listado no Azure Marketplace, não é possível alterar os campos indicados abaixo.

* **Alteração de preços dos SKU (s) listados**
* **Alteração de SKU (s) listados do modelo de faturação**
* **Remoção de regiões de SKU (s) listados de faturação**
* **Alterar o número de discos de dados de SKU (s) listados**
* **Identificador da oferta:** [portal de publicação -> máquinas virtuais -> selecione a sua oferta -> imagens de VM -> identificador oferecem]
* **Identificador SKU:** [portal de publicação -> máquinas virtuais -> selecione a sua oferta -> SKUs separador -> adicione um SKU]
* **Espaço de nomes do publicador:** [portal de publicação -> máquinas virtuais -> passo a passo guia -> espaço de nomes de publicador de dizer-nos sobre a sua empresa (encontrada no passo 2 registar) -> espaço de nomes]
* **Portas:** [portal de publicação -> máquinas virtuais -> a sua oferta -> imagens de VM separador -> portas abertas]

### <a name="sell-to-countries-of-the-sku"></a>"Vender" países do SKU
Precisa considerar cuidadosamente qual realizar suas SKUs disponíveis. Alguns países estão classificados como "Remessa da Microsoft" e outras pessoas estão classificadas como "Remessa de ISV."

* Em países de "Admitem a remessa de Microsoft", a Microsoft cobra impostos de clientes e paga impostos de (remits) para o Governo.
* Em países "ISV remeter", os parceiros são responsáveis por clientes de impostos a recolher e pagar impostos ao. Se optar por vender em países "ISV remeter", tem de ter a capacidade de calcular e pagar impostos em países/regiões que selecionar.

> [!NOTE]
> O SKU não estará disponível nos países, a menos que definir os preços [portal de publicação](https://publish.windowsazure.com). Orientações para obter, definir os preços de SKUs de BYOL e de hora a hora é indicada abaixo.
> 
> 

### <a name="211-how-to-setup-hourly-pricing-model-for-a-sku"></a>2.1.1 como configurar o modelo de preços por hora para um SKU
Siga os passos indicados abaixo, a configuração do modelo de preços por hora para uma SKU:

1. Início de sessão para o [portal de publicação](https://publish.windowsazure.com).
2. Navegue para o **máquinas virtuais** separador e selecione a sua oferta.
3. No menu do lado esquerdo, clique nas **SKUS** separador.
4. Certifique-se de que o SKU é marcado como "Modelo de faturação por hora". Se não for, em seguida, clique nas **editar** botão para reverter o modelo de faturação. Uma janela será aberta. Desmarque a caixa de verificação 'faturação e licenciamento é feito externamente do Azure (também conhecido como traga a sua própria licença)' e guarde as alterações.
5. Se pretender ativar a avaliação gratuita para a primeira 30days da implementação de SKU, em seguida, selecione a opção "Mês" para a pergunta "É uma avaliação gratuita disponível?" Caso contrário, selecione a opção "Sem versão de avaliação". Agora, siga os passos indicados abaixo.
6. No menu do lado esquerdo, clique nas **preços** separador.
7. Selecione a sua região base.
   
   ![desenho](media/marketplace-publishing-push-to-staging/img2.1.1_07.png)
8. Defina os preços para todos os núcleos. **Tem de fornecer o preço para todos os núcleos de um SKU, mesmo que o SKU não o suporta.**
   
    ![desenho](media/marketplace-publishing-push-to-staging/img2.1.1_08.png)
9. Definir os preços para as outras regiões manualmente ou pode utilizar o assistente AUTOPRICE para definir os preços de outras regiões com base na região base. Para utilizar o AUTOPRICE assistente, clique no botão **AUTOPRICE outros mercados com base no preços IN UNITED Estados.** **Nota:** etiqueta do botão pode ser diferente consoante a região que selecionou. Uma vez que selecionamos dos Estados Unidos ao criar este documento, então, o nome do botão é como "Preço automática em outros mercados com base nos preços nos Estados Unidos" na captura de ecrã abaixo.
   
   ![desenho](media/marketplace-publishing-push-to-staging/img2.1.1_09.png)
10. O Assistente de preço automática será aberto. A primeira página exibe a seleção para o mercado de base. Verifique a seção e mover para a página seguinte, ao clicar no botão "->".
    
    ![desenho](media/marketplace-publishing-push-to-staging/img2.1.1_10.png)
11. Opção para a seleção de planos de núcleos e será apresentada na página 2. Selecione os planos desejados e clique em "-a >" do botão. Clique no **Ativar/desativar todos** botão para selecionar todos os **planos de serviço** e **medidores** ou possa verificar manualmente as caixas de verificação. **Tem de fornecer o preço para todos os núcleos de um SKU, mesmo que o SKU não o suporta.** Por conseguinte, certifique-se de que todos os tamanhos de núcleo estão selecionados.
    
    ![desenho](media/marketplace-publishing-push-to-staging/img2.1.1_11.png)
12. Página 3 apresenta os mercados/regiões. Clique nas **todos os ativar/desativar** botão selecionar todas as regiões ou verificar manualmente as caixas de região. Clique no botão "->" mover para a página seguinte. **Nota:** Remitted países do imposto sobre Microsoft estão assinalados com uma casa, como o símbolo. Para obter mais detalhes consulte os secção "vender" países do SKU desta página.
    
    ![desenho](media/marketplace-publishing-push-to-staging/img2.1.1_12.png)
13. Página 4 apresenta as taxas de câmbio. Clique no botão Concluir para concluir os passos.

### <a name="212-how-to-setup-byol-pricing-model-for-a-sku"></a>2.1.2 como configurar o modelo de preços para uma SKU de BYOL
Siga os passos indicados abaixo para configurar o modelo de preços para uma SKU de BYOL:

1. Início de sessão para o [portal de publicação](https://publish.windowsazure.com).
2. Navegue para o **máquinas virtuais** separador e selecione a sua oferta.
3. No menu do lado esquerdo, clique nas **SKUS** separador.
4. Certifique-se de que o SKU está marcado como "Traga a sua própria licença do SKU". Caso contrário, em seguida, clique no botão Editar para reverter o modelo de faturação. Uma janela será aberta. Marque a caixa de verificação 'faturação e licenciamento é feito externamente do Azure (também conhecido como traga a sua própria licença)' e guarde as alterações.
   
   ![desenho](media/marketplace-publishing-push-to-staging/img2.1.2_04.png)
5. No menu do lado esquerdo, clique nas **preços** separador.
6. Selecione a região base e disponibilizar o SKU na região marcando a caixa de verificação contra o SKU na secção de disponibilidade de SKU EXTERNALLY-LICENSED (BYOL) (veja a captura de ecrã abaixo).
   
   ![desenho](media/marketplace-publishing-push-to-staging/img2.1.2_06.png)
7. Tornar o SKU disponível noutras regiões manualmente ou pode utilizar o assistente AUTOPRICE para esta finalidade. Consulte os pontos de #9 para #13 (o que explica o uso do assistente AUTOPRICE) na seção **"2.1.1 como configurar a hora a hora modelo de preços para uma SKU"** desta página.

### <a name="22-set-your-developer-service-prices"></a>2.2. Definir os preços do serviço de programador
Planos podem ser qualquer combinação de base + consumo, onde a base é o preço mensal e utilização excedida é o preço pay-per-use. (Veja abaixo para obter mais detalhes).

**Exemplo:** oferta de serviço do Programador de Contoso

| Planear | Preço | Inclui | Caminho de migração |
| --- | --- | --- | --- |
| Gratuito |US $0/ mês |Funcionalidade básica. |Pode migrar para qualquer outro plano |
| Bronze |US $10/ mês |A funcionalidade básica e uma quota de 1.000 da funcionalidade X. |Pode migrar para planos de Bronze Plus, Silver e Gold |
| Bronze Plus |Período de avaliação gratuita: 0 us $/ mês + US $0/meter01 |A funcionalidade básica e uma quota de 10.000 de funcionalidades X.  Assim que a quota de funcionalidades X é usado, o cliente pode pagar por utilização via meter01. |Pode migrar para planos Silver Plus e Gold |
| Bronze Plus |Pago período (também conhecido como versão de avaliação gratuita expirou): 10 de US $/ mês + US $ 0,05/meter01 |A funcionalidade básica e uma quota de 10.000 de funcionalidades X.  Assim que a quota de funcionalidades X é usado, o cliente pode pagar por utilização via meter01. |Pode migrar para planos Silver Plus e Gold |
| Prateado |US $ 0,15/meter01 |O cliente pode pagar por utilização via meter01, que se destine a funcionalidade de X. |Pode migrar para planos de Bronze e Gold |
| Silver Plus |US $20/ mês + US $ 0,15/meter01 + US $ 0,01/meter02 |A funcionalidade básica e uma quota de 10.000 de funcionalidades X e 100 da funcionalidade Y.  Assim que a quota de funcionalidades X é usada, o cliente pode pagar por utilização via meter01.  Assim que a quota de funcionalidade Y é usada, o cliente pode pagar por utilização via meter02. |Pode migrar para planos de Bronze Plus e Gold |
| Dourado |US $ 1.000/mês |Quota de 10.000 de funcionalidades X, 1.000 da funcionalidade Y e ilimitado da funcionalidade Z. |Pode migrar para todos os planos exceto gratuitos |

## <a name="step-3-provide-support-information"></a>Passo 3: Fornecer suporte de informações
Os detalhes de contactos são utilizados para as comunicações internas entre o parceiro e a Microsoft apenas. O URL de suporte estará disponível para os clientes finais.

1. Vá para o **suporte** cabeçalho no lado esquerdo do portal de publicação.
2. Introduza informações em **contacto de engenharia**.
3. Introduza informações em **suporte ao cliente**. Se fornecer apenas suporte de e-mail, introduza um número de telefone fictício e seu endereço de e-mail será utilizado em vez disso.
4. Introduza o URL de suporte.

## <a name="step-4-choose-azure-marketplace-categories"></a>Passo 4: Escolher categorias do Azure Marketplace
O **categorias** separador fornece uma matriz de seleções. A oferta pode estar abrangida por estas categorias e, pode selecionar até cinco categorias.

## <a name="how-your-marketing-will-appear"></a>Como será apresentado o marketing
Segue-se uma vista detalhada de como a oferta de informações de marketing é usada no [Web site do Azure Marketplace](https://azure.microsoft.com/marketplace/) e, no [portal do Azure](https://portal.azure.com).

### <a name="azure-marketplace-website"></a>Site do Azure Marketplace
![desenho](media/marketplace-publishing-push-to-staging/acom-catalog-01.png)

![desenho](media/marketplace-publishing-push-to-staging/acom-catalog-02.png)

*Lista de ofertas no site do Azure Marketplace*

![desenho](media/marketplace-publishing-push-to-staging/acom-listing-details-01.png)

*Detalhes de descrição da oferta no site do Azure Marketplace*

![desenho](media/marketplace-publishing-push-to-staging/acom-listing-details-02.png)

*Detalhes de preços no site do Azure Marketplace de descrição da oferta*

### <a name="azure-portal"></a>Portal do Azure
![desenho](media/marketplace-publishing-push-to-staging/azureportal-galleryblade-01.png)

*Lista de ofertas no Portal do Azure*

![desenho](media/marketplace-publishing-push-to-staging/azureportal-galleryblade-02.png)

*Detalhes de descrição da oferta no portal do Azure*

## <a name="next-steps"></a>Passos Seguintes
Agora que o conteúdo de Marketplace é carregado, vamos seguir em frente com o teste a sua oferta para efeitos de teste. No entanto, tem de selecionar o tipo de oferta adequada na lista a seguir, conforme os passos variam consoante o tipo de oferta.

* [Testar a sua oferta VM para efeitos de teste](marketplace-publishing-vm-image-test-in-staging.md)
* [Testar a sua oferta de modelo de solução para efeitos de teste](marketplace-publishing-solution-template-test-in-staging.md)

## <a name="see-also"></a>Consulte também
* [Introdução: como publicar uma oferta no Azure Marketplace](marketplace-publishing-getting-started.md)

[img-map-acom]:media/marketplace-publishing-push-to-staging/pubportal-mapping-acom.jpg
[img-map-portal]:media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg
[img-map-link]:media/marketplace-publishing-push-to-staging/marketing-content-guide-links.jpg
[img-map-logo]:media/marketplace-publishing-push-to-staging/marketing-content-guide-logos.jpg
[img-map-title]:media/marketplace-publishing-push-to-staging/marketing-content-guide-publisher-offer.png

[link-pubportal]:https://publish.windowsazure.com
[link-push-to-production]:marketplace-publishing-push-to-production.md
