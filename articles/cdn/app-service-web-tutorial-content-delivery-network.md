---
title: Tutorial - Adicionar a CDN do Azure a uma aplicação Web do Serviço de Aplicações do Azure | Microsoft Docs
description: Neste tutorial, a Rede de Entrega de Conteúdos (CDN) do Azure é adicionada a uma aplicação Web do Serviço de Aplicações do Azure para colocar em cache e entregar os seus ficheiros estáticos a partir de servidores próximos aos seus clientes em todo o mundo.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/14/2018
ms.author: v-deasim
ms.custom: mvc
ms.openlocfilehash: d782f5bf30160d85d7d9ef7a00190551f9a9843a
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
ms.locfileid: "34161249"
---
# <a name="tutorial-add-azure-cdn-to-an-azure-app-service-web-app"></a>Tutorial: Adicionar a CDN do Azure a uma aplicação Web do Serviço de Aplicações do Azure

Este tutorial mostra como adicionar a CDN do Azure a uma [aplicação Web no Serviço de Aplicações do Azure](../app-service/app-service-web-overview.md). A [Rede de Entrega de Conteúdos (CDN) do Azure](cdn-overview.md) coloca em cache conteúdo Web estático em localizações estrategicamente colocadas de modo a fornecer o débito máximo para disponibilização de conteúdo aos utilizadores. A CDN do Azure também reduz a carga do servidor na sua aplicação Web. 

Segue a home page do site HTML estático de exemplo com que irá trabalhar:

![Home page da aplicação de exemplo](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page.png)

O que irá aprender:

> [!div class="checklist"]
> * Criar um ponto final da CDN.
> * Atualizar ativos em cache.
> * Utilize cadeias de consulta para controlar versões em cache.
> * Utilize um domínio personalizado para o ponto final da CDN.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial:

- [Instale o Git](https://git-scm.com/).
- [Instalar a CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-web-app"></a>Criar a aplicação Web

Para criar a aplicação Web com que irá trabalhar, siga o [início rápido HTML estático](../app-service/app-service-web-get-started-html.md), através do passo **Navegar para a aplicação**.

### <a name="have-a-custom-domain-ready"></a>Ter um domínio personalizado pronto

Para concluir o passo de domínio personalizado deste tutorial, precisa de possuir um domínio personalizado e ter acesso ao seu registo DNS para o seu fornecedor de domínios. Por exemplo, para adicionar entradas DNS para contoso.com e www.contoso.com, tem de ter acesso para configurar as definições de DNS para o domínio de raiz contoso.com.

Se ainda não tiver um nome de domínio, considere seguir o [App Service domain tutorial](../app-service/custom-dns-web-site-buydomains-web-app.md)(tutorial de domínio do Serviço de Aplicações), para comprar um domínio com o portal do Azure. 

## <a name="log-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Abra um browser e navegue para o [portal do Azure](https://portal.azure.com).

## <a name="create-a-cdn-profile-and-endpoint"></a>Criar um perfil da CDN e ponto final

No painel de navegação esquerdo, selecione **Serviço de Aplicações** e, em seguida, selecione a aplicação que criou no [início rápido HTML estático](../app-service/app-service-web-get-started-html.md).

![Selecione a aplicação de Serviço de Aplicações no portal](media/app-service-web-tutorial-content-delivery-network/portal-select-app-services.png)

Na página **Serviço de Aplicações**, na secção **Definições**, selecione **Redes > Configurar a CDN do Azure para a sua aplicação**.

![Selecione a CDN no portal](media/app-service-web-tutorial-content-delivery-network/portal-select-cdn.png)

Na página **Azure Content Delivery Network**(Rede de Entrega de Conteúdos do Azure), indique as definições **Novo ponto final** conforme especificado na tabela.

![Criar perfil e ponto final no portal](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint.png)

| Definição | Valor sugerido | Descrição |
| ------- | --------------- | ----------- |
| **Perfil da CDN** | myCDNProfile | Selecione **Criar novo** para criar um novo perfil de CDN. Um perfil da CDN é uma coleção de pontos finais da CDN com o mesmo escalão de preços. |
| **Escalão de preço** | Standard da Akamai | O [escalão de preço](cdn-features.md) especifica o fornecedor e as funcionalidades disponíveis. Este tutorial utiliza o *Standard da Akamai*. |
| **Nome do ponto final da CDN** | Qualquer nome que é exclusivo no domínio azureedge.net | Aceda aos seus recursos em cache no domínio  *\<endpointname>.azureedge.net*.

Selecione **Criar**.

O Azure cria o perfil e o ponto final. O novo ponto final é apresentado na lista **Pontos finais** e, quando está aprovisionado, o estado é **A executar**.

![Novo ponto final na lista](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint-in-list.png)

### <a name="test-the-cdn-endpoint"></a>Testar o ponto final da CDN

Se tiver selecionado o escalão de preços Verizon, demora geralmente cerca de 90 minutos para a propagação do ponto final. Para Akamai, a propagação demora alguns minutos.

A aplicação de exemplo tem um ficheiro *index.html* e pastas *css*, *img* e *js* que contêm outros elementos estáticos. Os caminhos de conteúdo para todos estes ficheiros são os mesmos no ponto final da CDN. Por exemplo, ambos os seguintes URLs acedem ao ficheiro *bootstrap.css* na pasta *css*:

```
http://<appname>.azurewebsites.net/css/bootstrap.css
```

```
http://<endpointname>.azureedge.net/css/bootstrap.css
```

Navegue até um browser para o seguinte URL:

```
http://<endpointname>.azureedge.net/index.html
```

![Home page da aplicação de exemplo servida a partir de CDN](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page-cdn.png)

 Será apresentada a mesma página que executou anteriormente numa aplicação Web do Azure. A CDN do Azure obteve os ativos da aplicação Web de origem e está a servi-los a partir do ponto final da CDN

Para se certificar de que esta página está colocada em cache no CDN, atualize-a. Por vezes são necessários dois pedidos para o mesmo recurso, para o CDN colocar em cache os conteúdos solicitados.

Para obter mais informações sobre a criação de perfis de CDN do Azure e os pontos finais, consulte [Introdução ao Azure CDN](cdn-create-new-endpoint.md).

## <a name="purge-the-cdn"></a>Remover a CDN

A CDN atualiza periodicamente os respetivos recursos da aplicação Web de origem consoante a configuração de tempo de duração (TTL). O TTL predefinido é de sete dias.

Por vezes, poderá precisar de atualizar a CDN antes de expirar o TTL; por exemplo, ao implementar conteúdo atualizado na aplicação Web. Para acionar uma atualização, remova manualmente os recursos da CDN. 

Nesta secção do tutorial, implemente uma alteração na aplicação Web e remova o CDN para acionar o CDN para atualizar a respetiva cache.

### <a name="deploy-a-change-to-the-web-app"></a>Implementar uma alteração na aplicação Web

Abra o ficheiro *index.html* e adicione *-V2* ao cabeçalho H1, conforme mostrado no seguinte exemplo: 

```
<h1>Azure App Service - Sample Static HTML Site - V2</h1>
```

Consolide as alterações e implemente-as na aplicação Web.

```bash
git commit -am "version 2"
git push azure master
```

Depois de concluída a implementação, navegue para o URL da aplicação Web para ver a alteração.

```
http://<appname>.azurewebsites.net/index.html
```

![V2 no título na aplicação Web](media/app-service-web-tutorial-content-delivery-network/v2-in-web-app-title.png)

Se navegar para o URL de ponto final da CDN da home page, não verá a alteração porque a versão em cache na CDN ainda não expirou. 

```
http://<endpointname>.azureedge.net/index.html
```

![Nenhum V2 no título na CDN](media/app-service-web-tutorial-content-delivery-network/no-v2-in-cdn-title.png)

### <a name="purge-the-cdn-in-the-portal"></a>Remova a CDN no portal

Para acionar a CDN para atualizar a versão em cache, remova a CDN.

Na navegação à esquerda do portal, selecione **Grupos de recursos** e, em seguida, selecione o grupo de recursos que criou a sua aplicação Web (myResourceGroup).

![Selecionar grupo de recursos](media/app-service-web-tutorial-content-delivery-network/portal-select-group.png)

Na lista de recursos, selecione o ponto final de CDN.

![Selecione o ponto final](media/app-service-web-tutorial-content-delivery-network/portal-select-endpoint.png)

Na parte superior da página **Ponto final**, selecione **Remover**.

![Selecione Remover](media/app-service-web-tutorial-content-delivery-network/portal-select-purge.png)

Introduza os caminhos do conteúdo que quer remover. Pode transmitir um caminho de ficheiro completo para remover um ficheiro individual ou um segmento de caminho para remover e atualizar todo o conteúdo numa pasta. Como alterou o *index.html*, certifique-se de que está num dos caminhos.

Na parte inferior da página, selecione **Remover**.

![Remover página](media/app-service-web-tutorial-content-delivery-network/app-service-web-purge-cdn.png)

### <a name="verify-that-the-cdn-is-updated"></a>Certifique-se de que a CDN é atualizada

Aguarde até que o pedido de remoção conclua o processamento, que demora normalmente alguns minutos. Para ver o estado atual, selecione o ícone da campainha na parte superior da página. 

![Remover notificação](media/app-service-web-tutorial-content-delivery-network/portal-purge-notification.png)

Ao navegar para o URL de ponto final da CDN para *index.html*, verá o *V2* que adicionou ao título na home page, que indica que a cache da CDN foi atualizada.

```
http://<endpointname>.azureedge.net/index.html
```

![V2 no título na CDN](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title.png)

Para obter mais informações, consulte [Remover um ponto final do Azure CDN](../cdn/cdn-purge-endpoint.md). 

## <a name="use-query-strings-to-version-content"></a>Utilizar cadeias de consulta para o conteúdo da versão

A CDN do Azure oferece as seguintes opções de comportamento de colocação em cache:

* Ignorar cadeias de consulta
* Ignorar a colocação em cache de cadeias de consulta
* Colocar em cache todos os URL exclusivos 

A primeira opção é a predefinida, o que significa que existe apenas uma versão em cache de um ativo, independentemente da cadeia de consulta no URL. 

Nesta secção do tutorial, pode alterar o comportamento de colocação em cache para colocar em cache todos os URL exclusivos.

### <a name="change-the-cache-behavior"></a>Alterar o comportamento de cache

Na página **Ponto Final CDN** do portal do Azure, selecione **Cache**.

Selecione **Colocar em cache todos os URL exclusivos** relativo ao **comportamento de colocação em cache da Cadeia de consulta** na lista pendente.

Selecione **Guardar**.

![Selecione o comportamento de colocação em cache de cadeia de consulta](media/app-service-web-tutorial-content-delivery-network/portal-select-caching-behavior.png)

### <a name="verify-that-unique-urls-are-cached-separately"></a>Certifique-se de que os URL exclusivos são colocados em cache em separado

Num browser, navegue para a home page no ponto final da CDN e inclua uma cadeia de consulta: 

```
http://<endpointname>.azureedge.net/index.html?q=1
```

A CDN do Azure devolve o conteúdo da aplicação Web atual, que inclui *V2* no cabeçalho. 

Para se certificar de que esta página está colocada em cache no CDN, atualize-a. 

Abra *index.html*, altere *V2* para *V3* e, em seguida, implemente a alteração. 

```bash
git commit -am "version 3"
git push azure master
```

Num browser, aceda ao URL de ponto final da CDN com uma nova cadeia de consulta, como `q=2`. A CDN do Azure obtém o ficheiro *index.html* atual e apresenta *V3*. No entanto, se navegar para o ponto final da CDN com a cadeia de consulta `q=1`, verá *V2*.

```
http://<endpointname>.azureedge.net/index.html?q=2
```

![V3 no título na CDN, cadeia de consulta 2](media/app-service-web-tutorial-content-delivery-network/v3-in-cdn-title-qs2.png)

```
http://<endpointname>.azureedge.net/index.html?q=1
```

![V2 no título na CDN, cadeia de consulta 1](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title-qs1.png)

Este resultado mostra que cada cadeia de consulta é tratada de forma diferente:

* q=1 foi utilizado antes, pelo que os conteúdos armazenados em cache são devolvidos (V2).
* q=2 é novo, pelo que os conteúdos mais recentes da aplicação Web são obtidos e devolvidos (V3).

Para obter mais informações, consulte [Control Azure CDN caching behavior with query strings](../cdn/cdn-query-string.md)(Controlar o comportamento de colocação em cache do Azure CDN com cadeias de consulta).

## <a name="map-a-custom-domain-to-a-cdn-endpoint"></a>Mapear domínios personalizados para um ponto final de CDN

Irá mapear o seu domínio personalizado para o ponto final da CDN ao criar um registo CNAME. Um registo CNAME é uma funcionalidade de DNS que mapeia domínios de origem para um domínio de destino. Por exemplo, pode mapear cdn.contoso.com ou static.contoso.com para contoso.azureedge.net.

Se não tiver um domínio personalizado, considere seguir o [tutorial de domínio do Serviço de Aplicações](../app-service/custom-dns-web-site-buydomains-web-app.md) para comprar um domínio com o portal do Azure. 

### <a name="find-the-hostname-to-use-with-the-cname"></a>Encontre o nome de anfitrião para utilizar com o CNAME

Na página **Ponto final** do portal do Azure, certifique-se de que a opção **Descrição Geral** está selecionada na navegação à esquerda e, em seguida, selecione o botão **+ Domínio Personalizado** na parte superior da página.

![Selecione Adicionar Domínio Personalizado](media/app-service-web-tutorial-content-delivery-network/portal-select-add-domain.png)

Na página **Adicionar um domínio personalizado**, verá o nome de anfitrião do ponto final a utilizar na criação de um registo CNAME. O nome do anfitrião é obtido a partir do seu URL do ponto final de CDN: **&lt;EndpointName>.azureedge.net**. 

![Página Adicionar Domínio](media/app-service-web-tutorial-content-delivery-network/portal-add-domain.png)

### <a name="configure-the-cname-with-your-domain-registrar"></a>Configure o CNAME com a sua entidade de registo do domínio

Navegue para o site da entidade de domínio e localize a secção para criar registos DNS. Por exemplo, esta secção pode ser denominada **Nome de Domínio**, **DNS** ou **Gestão de Servidor de Nomes**.

Encontre a secção para gerir CNAMEs. Poderá ter de voltar a uma página de definições avançadas e procurar as palavras, CNAME, Alias ou Subdomínios.

Crie um registo CNAME que mapeia o subdomínio escolhido (por exemplo, **estático** ou **cdn**) para o **Nome de anfitrião do ponto final** apresentado anteriormente no portal. 

### <a name="enter-the-custom-domain-in-azure"></a>Introduza o domínio personalizado no Azure

Regresse à página **Adicionar domínio personalizado** e introduza o seu domínio personalizado, incluindo o subdomínio, na caixa de diálogo. Por exemplo, introduza *cdn.contoso.com*.   
   
O Azure verifica se o registo CNAME existe para o nome de domínio que introduziu. Se o CNAME estiver correto, o seu domínio personalizado é validado.

Pode levar algum tempo para o registo CNAME ser propagado para servidores de nomes na Internet. Se o seu domínio não for validado imediatamente, aguarde alguns minutos e tente novamente.

### <a name="test-the-custom-domain"></a>Testar o domínio personalizado

Num browser, navegue para o ficheiro *index.html* com o seu domínio personalizado (por exemplo, cdn.contoso.com/index.html) para verificar se o resultado é o mesmo quando vai diretamente a &lt;endpointname&gt;azureedge.net/index.html.

![Home page da aplicação de exemplo através de URL de domínio personalizado](media/app-service-web-tutorial-content-delivery-network/home-page-custom-domain.png)

Para obter mais informações, consulte [Mapear conteúdo do Azure CDN para um domínio personalizado](../cdn/cdn-map-content-to-custom-domain.md).

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Passos seguintes

O que aprendeu:

> [!div class="checklist"]
> * Criar um ponto final da CDN.
> * Atualizar ativos em cache.
> * Utilize cadeias de consulta para controlar versões em cache.
> * Utilize um domínio personalizado para o ponto final da CDN.

Saiba como otimizar o desempenho da CDN nos seguintes artigos:

> [!div class="nextstepaction"]
> [Tutorial: Adicionar um domínio personalizado ao ponto final da CDN do Azure](cdn-map-content-to-custom-domain.md)


