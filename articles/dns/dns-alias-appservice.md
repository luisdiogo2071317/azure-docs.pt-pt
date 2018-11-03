---
title: Alojar aplicações da web do Azure com balanceamento de carga no vértice da zona
description: Utilizar um alias de registo para alojar aplicações web com balanceamento de carga de DNS do Azure no vértice da zona
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 11/3/2018
ms.author: victorh
ms.openlocfilehash: 2b14753237e118540da6306fa9f06816f3e58b71
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/03/2018
ms.locfileid: "50979867"
---
# <a name="host-load-balanced-azure-web-apps-at-the-zone-apex"></a>Alojar aplicações da web do Azure com balanceamento de carga no vértice da zona

O protocolo DNS impede a atribuição de nada além de um registo A ou AAAA no vértice da zona. Um vértice da zona de exemplo é contoso.com. Esta restrição apresenta um problema para os proprietários da aplicação que tenham aplicações com balanceamento de carga por trás do Gestor de tráfego. Não é possível apontar para o perfil do Gestor de tráfego do registo de apex de zona. Como resultado, os proprietários da aplicação tem de utilizar uma solução alternativa. Um redirecionamento na camada da aplicação tem de redirecionar do vértice da zona para outro domínio. Um exemplo é um redirecionamento de contoso.com para www.contoso.com. Essa organização apresenta um ponto único de falha para a função de redirecionamento.

Com os registros de alias, esse problema deixou de existir. Agora os proprietários de aplicativos podem apontar o respetivo registo de apex de zona para um perfil de Gestor de tráfego que tem pontos finais externos. Os proprietários de aplicativos podem apontar para o mesmo perfil de Gestor de tráfego, que é utilizado para qualquer outro domínio na sua zona DNS.

Por exemplo, contoso.com e www.contoso.com podem apontar para o mesmo perfil de Gestor de tráfego. Esse é o caso, desde que o perfil do Gestor de tráfego tem apenas pontos finais externos configurados.

Neste artigo, saiba como criar um registo de alias para o vértice do domínio e configurar os pontos de final de perfil do Gestor de tráfego para as suas aplicações web.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Deve ter um nome de domínio disponível que possa alojar no DNS do Azure para testar. Tem de ter controlo total sobre este domínio. Controlo total inclui a capacidade de definir o servidor de nomes (NS) de domínio.

Para obter instruções sobre como alojar o seu domínio no DNS do Azure, veja [Tutorial: Alojar o seu domínio no DNS do Azure](dns-delegate-domain-azure-dns.md).

O domínio de exemplo utilizado para este tutorial for contoso.com, mas utilizar o seu próprio nome de domínio.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos para conter todos os recursos usados neste artigo.

## <a name="create-app-service-plans"></a>Criar planos de serviço de aplicações

Crie dois planos de serviço de aplicações Web no seu grupo de recursos utilizando a seguinte tabela para obter informações de configuração. Para obter mais informações sobre como criar um plano do serviço de aplicações, consulte [gerir um plano do serviço de aplicações no Azure](../app-service/app-service-plan-manage.md).


|Nome  |Sistema Operativo  |Localização  |Escalão de Preço  |
|---------|---------|---------|---------|
|ASP-01     |Windows|EUA Leste|Programador/teste D1 partilhado|
|ASP-02     |Windows|EUA Central|Programador/teste D1 partilhado|

## <a name="create-app-services"></a>Criar serviços de aplicações

Crie duas aplicações web, um em cada plano de serviço de aplicações.

1. No canto superior esquerdo da página do portal do Azure, clique em **criar um recurso**.
2. Tipo **aplicação Web** na barra de pesquisa e prima Enter.
3. Clique em **aplicação Web**.
4. Clique em **Criar**.
5. Aceite as predefinições e utilize a tabela seguinte para configurar as duas aplicações web:

   |Nome<br>(tem de ser exclusivos. azurewebsites.net)|Grupo de Recursos |Plano do serviço de aplicações/localização
   |---------|---------|---------|
   |Aplicações-01|Utilizar existente<br>Selecione o grupo de recursos|ASP 01(East US)|
   |Aplicações-02|Utilizar existente<br>Selecione o grupo de recursos|ASP 02(Central US)|

### <a name="gather-some-details"></a>Reunir alguns detalhes

Agora precisa ter em atenção o nome de anfitrião e endereço IP para as aplicações.

1. Abra o seu grupo de recursos e clique em sua primeira aplicação (**aplicação-01** neste exemplo).
2. Na coluna esquerda, clique em **propriedades**.
3. Tome nota do endereço sob **URL**e, em **endereços IP de saída** tenha em atenção o primeiro endereço IP na lista. Irá utilizar estas informações mais tarde quando configurar os pontos finais do Gestor de tráfego.
4. Repita esta operação para **aplicação-02**.

## <a name="create-a-traffic-manager-profile"></a>Criar um perfil do Gestor de Tráfego

Crie um perfil do Gestor de tráfego no seu grupo de recursos. Utilize as predefinições e escreva um nome exclusivo no espaço de nomes trafficmanager.net.

Para obter informações sobre como criar um perfil do Gestor de tráfego, consulte [início rápido: criar um perfil do Gestor de tráfego para uma aplicação web de elevada disponibilidade](../traffic-manager/quickstart-create-traffic-manager-profile.md).

### <a name="create-endpoints"></a>Criar pontos finais

Agora, pode criar os pontos finais para as duas aplicações web.

1. Abra o seu grupo de recursos e clique em seu perfil do Gestor de tráfego.
2. Na coluna esquerda, clique em **pontos de extremidade**.
3. Clique em **Adicionar**.
4. Utilize a tabela seguinte para configurar os pontos de extremidade:

   |Tipo  |Nome  |Destino  |Localização  |Definições personalizadas do cabeçalho|
   |---------|---------|---------|---------|---------|
   |Ponto final externo     |End-01|Endereço IP que registou para a aplicação-01|EUA Leste|anfitrião:\<o URL que registou para a aplicação-01\><br>Exemplo: **anfitrião: a aplicação-01.azurewebsites.net**|
   |Ponto final externo     |End-02|Endereço IP que registou para a aplicação-02|EUA Central|anfitrião:\<o URL que registou para a aplicação-02\><br>Exemplo: **anfitrião: a aplicação-02.azurewebsites.net**

## <a name="create-dns-zone"></a>Criar zona DNS

Pode utilizar uma zona DNS existente para fins de teste, ou pode criar uma nova zona. Para criar e delegar uma nova zona DNS no Azure, consulte [Tutorial: alojar o seu domínio no DNS do Azure](dns-delegate-domain-azure-dns.md).

### <a name="add-the-alias-record-set"></a>Adicionar o conjunto de registos de alias

Quando a zona DNS estiver pronta, pode adicionar um registo de alias para o vértice da zona.

1. Abra o seu grupo de recursos e clique na zona DNS.
2. Clique em **Conjunto de registos**.
3. Adicione o conjunto utilizando a seguinte tabela de registos:

   |Nome  |Tipo  |Conjunto de registos de alias  |Tipo de alias  |Recursos do Azure|
   |---------|---------|---------|---------|-----|
   |@     |A|Sim|Recursos do Azure|Gestor de tráfego - seu perfil|

## <a name="add-custom-hostnames"></a>Adicionar nomes de anfitriões personalizados

Adicione um nome de anfitrião personalizado para ambas as aplicações web.

1. Abra o seu grupo de recursos e clique em sua primeira aplicação web.
2. Na coluna esquerda, clique em **domínios personalizados**.
3. Clique em **Adicionar nome de anfitrião**.
4. Em nome de anfitrião, escreva o seu nome de domínio. Por exemplo, contoso.com.

   O domínio deve passar na validação e Mostrar marcas de verificação verdes junto a **disponibilidade de nome de anfitrião** e **propriedade do domínio**.
5. Clique em **Adicionar nome de anfitrião**.
6. Para ver o novo nome de anfitrião sob **nomes de anfitrião atribuídos ao site**, atualize o browser. A atualização na página não mostram sempre as alterações imediatamente.
7. Repita este procedimento para a segunda aplicação web.

## <a name="test-your-web-apps"></a>Testar as suas aplicações web

Agora pode testar para se certificar de que pode entrar a sua aplicação web e que ele está sendo carga balanceada.

1. Abra um browser e navegue para o seu domínio. Por exemplo, contoso.com. Deverá ver a página de aplicação web predefinida.
2. Pare a sua primeira aplicação web.
3. Feche seu navegador da web e aguarde alguns minutos.
4. Iniciar o browser e navegue para o seu domínio. Ainda verá a página de aplicação web predefinida.
5. Pare a segunda aplicação web.
6. Feche seu navegador da web e aguarde alguns minutos.
7. Iniciar o browser e navegue para o seu domínio. Deverá ver o erro 403, que indica que a aplicação web está parada.
8. Comece a sua segunda aplicação web.
9. Feche seu navegador da web e aguarde alguns minutos.
10. Iniciar o browser e navegue para o seu domínio. Deverá ver a página de aplicação web predefinida novamente.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre os registos de alias, veja os artigos seguintes:

- [Tutorial: Configurar um registo de alias para fazer referência a um endereço IP público do Azure](tutorial-alias-pip.md)
- [Tutorial: Configurar um registo de alias para oferecer suporte a nomes de domínio com o Gestor de tráfego do vértice](tutorial-alias-tm.md)
- [FAQ sobre DNS](https://docs.microsoft.com/azure/dns/dns-faq#alias-records)
