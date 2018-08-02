---
title: Utilizar a CDN do Azure para aceder a blobs com domínios personalizados através de HTTPS
description: Saiba como integrar a CDN do Azure com o armazenamento de BLOBs para aceder a blobs com domínios personalizados através de HTTPS
services: storage
author: michaelhauss
ms.service: storage
ms.topic: article
ms.date: 06/26/2018
ms.author: mihauss
ms.component: blobs
ms.openlocfilehash: 7c4acc7d0832442b94735619ea3a01cb319da993
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398260"
---
# <a name="using-the-azure-cdn-to-access-blobs-with-custom-domains-over-https"></a>Utilizar a CDN do Azure para aceder a blobs com domínios personalizados através de HTTPS
Azure Content Delivery Network (CDN) suporta agora o HTTPS para nomes de domínio personalizados. Pode aproveitar esse recurso para aceder a blobs de armazenamento com o seu domínio personalizado através de HTTPS. Para tal, primeiro terá de ativar a CDN do Azure no seu ponto final de web ou de BLOBs e mapear a CDN para um nome de domínio personalizado. Depois de efetuar estes passos, ativar HTTPS para o seu domínio personalizado é simplificado através da ativação de um clique, gestão de certificados completa e tudo sem custos adicionais para os preços normais da CDN.

Esta capacidade é importante porque permite-lhe proteger a privacidade e integridade dos dados dos seus dados de aplicação web confidenciais em trânsito. Utilizar o protocolo SSL para servir o tráfego através de HTTPS, garante que os dados são encriptados quando são enviados pela internet. HTTPS proporciona confiança e a autenticação e protege as suas aplicações web contra ataques.

> [!NOTE]  
> Além de fornecer suporte SSL para nomes de domínio personalizado, a CDN do Azure pode ajudá-lo a dimensionar a sua aplicação para fornecer conteúdo de largura de banda alta em todo o mundo. Para obter mais informações, confira [descrição geral da CDN do Azure](../../cdn/cdn-overview.md).

## <a name="quick-start"></a>Início rápido
Estes são os passos necessários para ativar HTTPS para o ponto de final de armazenamento de BLOBs personalizado:

1.  [Integrar uma conta de armazenamento do Azure CDN do Azure](../../cdn/cdn-create-a-storage-account-with-cdn.md).
    Este artigo explica como criar uma conta de armazenamento no Portal do Azure, se não o tiver feito isso já.

    > [!NOTE]  
    > Durante a pré-visualização do suporte de Web sites estáticos no armazenamento do Azure, selecione "origem personalizada" no menu para adicionar o ponto final de web de armazenamento pendente "tipo de origem". No Portal do Azure, terá de fazê-lo a partir do seu perfil da CDN em vez de diretamente na sua conta de armazenamento.

2.  [Mapear conteúdo da CDN do Azure para um domínio personalizado](../../cdn/cdn-map-content-to-custom-domain.md).
3.  [Ativar o HTTPS num domínio personalizado da CDN do Azure](../../cdn/cdn-custom-ssl.md).

## <a name="shared-access-signatures"></a>Assinaturas de acesso partilhado
Se o ponto final de armazenamento de BLOBs estiver configurado para não permitir o acesso de leitura anónimo, terá de fornecer um [assinatura de acesso partilhado (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) token em cada pedido fizer ao seu domínio personalizado. Por predefinição, os pontos finais de armazenamento de BLOBs não permitam o acesso de leitura anónimo. Ver [gerir acesso de leitura anónimo a contentores e blobs](storage-manage-access-to-resources.md) para obter mais informações sobre assinaturas de acesso partilhado.

A CDN do Azure não respeitam quaisquer restrições adicionadas para o token SAS. Por exemplo, todos os tokens SAS têm um prazo de expiração. Isso significa que o conteúdo ainda pode ser acedida com uma SAS expirada até que os conteúdos são eliminados de nós de extremidade da CDN. Pode controlar o tempo que dados é colocado em cache na CDN, definindo o cabeçalho de resposta de cache. Ver [gerir a expiração de blobs de armazenamento do Azure na CDN do Azure](../../cdn/cdn-manage-expiration-of-blob-content.md) para obter instruções.

Se criar várias URLs de SAS para o mesmo ponto de final do blob, recomendamos que ativar a colocação em cache na CDN do Azure de cadeia de consulta. Trata-se para se certificar de que cada URL é tratada como uma entidade exclusiva. Ver [controlar o comportamento com cadeias de consulta de cache do Azure CDN](../../cdn/cdn-query-string.md) para obter mais informações.

## <a name="http-to-https-redirection"></a>HTTP para redirecionamento a HTTPS
Pode optar por redirecionar o tráfego HTTP para HTTPS. Isto requer a utilização do premium de CDN do Azure da Verizon da oferta. Precisa [comportamento de substituir HTTP usando o mecanismo de regras do CDN do Azure](../../cdn/cdn-rules-engine.md) com a seguinte regra:

![](./media/storage-https-custom-domain-cdn/redirect-to-https.png)

"Nome de ponto final de Cdn" refere-se para o nome que configurou para o ponto final da CDN. Pode selecionar este valor na lista pendente. "Caminho de origem" refere-se para o caminho na sua conta de armazenamento de origem onde reside o seu conteúdo estático. Se estiver a alojar todo o conteúdo estático num único contentor, substitua "caminho de origem" com o nome nesse contentor.

Para se aprofundar nas regras, consulte a [funcionalidades do motor de regras de CDN do Azure](../../cdn/cdn-rules-engine-reference-features.md).

## <a name="pricing-and-billing"></a>Preços e faturação
Quando aceder a blobs através de uma CDN do Azure, paga [os preços de armazenamento de BLOBs](https://azure.microsoft.com/pricing/details/storage/blobs/) para o tráfego entre os nós de extremidade e a origem (armazenamento de BLOBs), e [os preços da CDN](https://azure.microsoft.com/pricing/details/cdn/) para dados acedidos a partir de nós de extremidade.

Por exemplo, digamos que tenha uma conta de armazenamento na região E.U.A. oeste que está sendo acessado a utilizar uma CDN do Azure. Se alguém no Reino Unido tentar acessar um dos blobs nessa conta de armazenamento através da CDN, o Azure verifica primeiro o nó de extremidade mais próximo do Reino Unido para esse blob. Se encontrado, ele acessa essa cópia do blob e irá utilizar preços da CDN, uma vez que está sendo acessado na CDN. Se não for encontrado, o Azure irá copiar o blob ao nó de extremidade, o que resulta em custos de transação e de saída, conforme especificado nos preços de armazenamento de BLOBs e, em seguida, aceder ao ficheiro no nó de extremidade, o que irá resultar numa faturação da CDN.

Ao examinar os [página de preços de CDN](https://azure.microsoft.com/pricing/details/cdn/), tenha em atenção que o suporte de HTTPS para nomes de domínio personalizados só está disponível na CDN do Azure da Verizon produtos (Standard e Premium).

## <a name="next-steps"></a>Passos Seguintes
* [Configurar um nome de domínio personalizado para o ponto final de armazenamento de BLOBs](storage-custom-domain-name.md)
* [Alojamento de Web site estático no armazenamento do Azure (pré-visualização)](storage-blob-static-website.md)
