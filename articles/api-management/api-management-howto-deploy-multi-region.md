---
title: Implementar serviços de gestão de API do Azure em várias regiões do Azure | Documentos da Microsoft
description: Saiba como implementar uma instância de serviço de gestão de API do Azure em várias regiões do Azure.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: apimpm
ms.openlocfilehash: 82ae0ef72bb4f546a1f946f3127aa5d74bec3c3b
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52957764"
---
# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>Como implementar uma instância de serviço de gestão de API do Azure em várias regiões do Azure

Gestão de API do Azure suporta implementação em várias regiões, que permite que os publicadores de APIS distribuir um único serviço de gestão de API do Azure em qualquer número de regiões do Azure pretendidos. Isto ajuda a reduzir a latência percebida pelo distribuídos geograficamente os consumidores de APIS e também melhora a disponibilidade do serviço, se uma região ficar offline de pedido.

Um novo serviço de gestão de API do Azure contém apenas um inicialmente [unidade] [ unit] numa única região do Azure, a região primária. Regiões adicionais podem ser facilmente adicionados através do portal do Azure. É implementado um servidor de gateway de gestão de API para cada região e chamada de tráfego será encaminhado para o gateway mais próximo. Se uma região ficar offline, o tráfego é automaticamente redirecionado para o gateway mais próximo seguinte.

> [!NOTE]
> Gestão de API do Azure replica apenas o componente de gateway de API em várias regiões. O componente de gestão de serviço está alojado apenas na região primária. Em caso de indisponibilidade na região primária, aplicar alterações de configuração a uma instância de serviço de gestão de API do Azure não é possível – incluindo as definições ou atualizações de políticas.

[!INCLUDE [premium.md](../../includes/api-management-availability-premium.md)]

## <a name="add-region"> </a>Implementar uma instância de serviço de gestão de API para uma nova região

> [!NOTE]
> Se ainda não criou uma instância de serviço de gestão de API, consulte [criar uma instância de serviço de gestão de API][Create an API Management service instance].

No portal do Azure, navegue para o **dimensionamento e preços** página para a sua instância do serviço de gestão de API. 

![Separador Escala][api-management-scale-service]

Para implementar uma nova região, clique em **+ adicionar região** da barra de ferramentas.

![Adicionar região][api-management-add-region]

Selecione a localização da lista pendente e definir o número de unidades para com o controlo de deslize.

![Especifique unidades][api-management-select-location-units]

Clique em **adicionar** para colocar a sua seleção na tabela de localizações. 

Repita este processo até ter todas as localizações configuradas e clique em **guardar** da barra de ferramentas para iniciar o processo de implantação.

## <a name="remove-region"> </a>Eliminar uma instância de serviço de gestão de API a partir de uma localização

No portal do Azure, navegue para o **dimensionamento e preços** página para a sua instância do serviço de gestão de API. 

![Separador Escala][api-management-scale-service]

Para a localização que pretende remover, abra o menu de contexto utilizando o **...**  botão na extremidade direita da tabela. Selecione o **eliminar** opção.

Confirmar a eliminação e clique em **guardar** para aplicar as alterações.

## <a name="route-backend"> </a>Chamadas de API de rota para serviços de back-end regional

Apenas um URL de serviço de back-end de funcionalidades de gestão de API do Azure. Apesar de existirem instâncias de API Management do Azure em várias regiões, o gateway de API irá ainda encaminhar pedidos para o mesmo serviço de back-end, que é implementada na região de apenas um. Neste caso, o ganho de desempenho virão-se apenas a partir de respostas em cache dentro da gestão de API do Azure numa região específica para o pedido, mas entrar em contato com o back-end em todo o mundo pode causar uma latência elevada.

Para aproveitar totalmente a distribuição geográfica do seu sistema, deve ter os serviços de back-end implementados nas mesmas regiões como instâncias de API Management do Azure. Em seguida, através de políticas e `@(context.Deployment.Region)` propriedade, pode encaminhar o tráfego para instâncias locais do seu back-end.

1. Navegue para a instância de gestão de API do Azure e clique em **APIs** no menu à esquerda.
2. Selecione a sua API pretendido.
3. Clique em **editor de código** pendente na seta para a **processamento de entrada**.

    ![Editor de código de API](./media/api-management-howto-deploy-multi-region/api-management-api-code-editor.png)

4. Utilize o `set-backend` combinado com condicional `choose` políticas para construir uma política de encaminhamento adequada no `<inbound> </inbound>` secção do ficheiro.

    Por exemplo, o ficheiro abaixo XML funciona para E.U.A. oeste e Ásia Oriental:

    ```xml
    <policies>
        <inbound>
            <base />
            <choose>
                <when condition="@("West US".Equals(context.Deployment.Region, StringComparison.OrdinalIgnoreCase))">
                    <set-backend-service base-url="http://contoso-us.com/" />
                </when>
                <when condition="@("East Asia".Equals(context.Deployment.Region, StringComparison.OrdinalIgnoreCase))">
                    <set-backend-service base-url="http://contoso-asia.com/" />
                </when>
                <otherwise>
                    <set-backend-service base-url="http://contoso-other.com/" />
                </otherwise>
            </choose>
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <base />
        </outbound>
        <on-error>
            <base />
        </on-error>
    </policies>
    ```

[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-location-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-location-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Create an API Management service instance]: get-started-create-service-instance.md
[Get started with Azure API Management]: get-started-create-service-instance.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[unit]: https://azure.microsoft.com/pricing/details/api-management/
[Premium]: https://azure.microsoft.com/pricing/details/api-management/
