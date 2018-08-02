---
title: Alojamento de Web site estático no armazenamento do Azure (pré-visualização) | Documentos da Microsoft
description: O armazenamento do Azure oferece agora o Web site estático (pré-visualização) de alojamento, fornecendo uma solução económica e escalável para alojar aplicações web modernas.
services: storage
author: MichaelHauss
ms.service: storage
ms.topic: article
ms.date: 06/26/18
ms.author: mihauss
ms.component: blobs
ms.openlocfilehash: e53b573a27f0b1462ccf1170bbde2f8af01d0d3a
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397480"
---
# <a name="static-website-hosting-in-azure-storage-preview"></a>Alojamento de Web site estático no armazenamento do Azure (pré-visualização)
O armazenamento do Azure oferece agora o Web site estático (pré-visualização) de alojamento, permitindo-lhe implementar aplicações web modernas, económica e dimensionável no Azure. Num Web site estático, páginas da Web contém conteúdo estático e o JavaScript ou outro código do lado do cliente. Por outro lado, depender de código do lado do servidor Web sites dinâmicos e pode ser hospedados usando [aplicações Web do Azure](/app-service/app-service-web-overview.md).

Como implementações mudança no sentido de modelos flexíveis e econômicos, a capacidade de fornecer conteúdo da web sem a necessidade de gestão de servidor é fundamental. A introdução de hospedagem de Web site estático no armazenamento do Azure torna isso possível, habilitando recursos avançados de back-end com arquiteturas sem servidor, tirando partido [as funções do Azure](/azure-functions/functions-overview.md) e outros serviços PaaS.

## <a name="how-does-it-work"></a>Como funciona?
Quando ativar os Web sites estáticos na sua conta de armazenamento, um novo ponto de final de serviço de web é criado do formulário `<account-name>.<zone-name>.web.core.windows.net`.

Ponto de extremidade de serviço da web sempre permite acesso de leitura anónimo, devolve formatadas de HTML de páginas em resposta a erros de serviço e permite apenas operações de leitura de objeto. O ponto de final de serviço web devolve o documento de índice no diretório pedido para a raiz e todos os subdiretórios. Quando o serviço de armazenamento devolve um erro 404, o ponto final web devolve um documento de erro personalizada se as tiver configurado.

Conteúdo do Web site estático é hospedado num contêiner especial com o nome "$web". Como parte do processo de ativação, "$web" é criada para caso ainda não exista. Conteúdo em "$web" pode ser acessado na raiz de conta com o ponto final web. Por exemplo `https://contoso.z4.web.core.windows.net/` devolve o documento de índice que configurou para o seu Web site, se um documento esse nome existe no diretório raiz do $web.

Ao carregar o conteúdo para o seu Web site, utilize o ponto final de armazenamento de Blobs. Para carregar um blob com o nome "jpg", que pode ser acessado na raiz da conta use a seguinte URL `https://contoso.blob.core.windows.net/$web/image.jpg`. A imagem carregada pode ser visualizada num navegador da web no ponto final web correspondente `https://contoso.z4.web.core.windows.net/image.jpg`.


## <a name="custom-domain-names"></a>Nomes de domínio personalizados
Pode utilizar um domínio personalizado para alojar o seu conteúdo da web. Para tal, siga as orientações no [configurar um nome de domínio personalizado para a sua conta de armazenamento do Azure](storage-custom-domain-name.md). Para aceder ao seu Web site hospedado num nome de domínio personalizado através de HTTPS, consulte [utilizar a CDN do Azure para aceder a blobs com domínios personalizados através de HTTPS](storage-https-custom-domain-cdn.md).

## <a name="pricing-and-billing"></a>Preços e faturação
Alojamento de Web site estático é fornecido sem custos adicionais. Para obter mais detalhes sobre os preços para o armazenamento de Blobs do Azure, consulte a [página de preços do Azure Blob Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="quickstart"></a>Início Rápido
### <a name="azure-portal"></a>Portal do Azure
Para começar a alojar a sua aplicação web no armazenamento do Azure, pode configurar a funcionalidade com o Portal do Azure e clique em "Web site estático (pré-visualização)" em "Definições", na barra de navegação esquerdo. Clique em "Ativada" e introduza o nome do documento de índice e (opcionalmente) o caminho do documento de erro personalizada.

![](media/storage-blob-static-website/storage-blob-static-website-portal-config.PNG)

Carregue os seus ativos de web para o contentor de "$web" que foi criado como parte da ativação de Web site estático. Pode fazê-lo diretamente no Portal do Azure, ou pode aproveitar [Explorador de armazenamento do Azure](https://azure.microsoft.com/features/storage-explorer/) para carregar as estruturas de diretório inteiro. Certifique-se incluir um documento de índice com o nome que configurou. Neste exemplo, o nome do documento é "Index".

> [!NOTE]
> O nome do documento é sensível às maiúsculas e, por conseguinte, tem de corresponder exatamente ao nome do ficheiro no armazenamento.

Por fim, navegue para o ponto final de web para testar o seu Web site.

## <a name="faq"></a>FAQ
**Os Web sites estáticos está disponível para todos os tipos de conta de armazenamento?**  
Não, o alojamento de Web site estático apenas está disponível nas contas de armazenamento padrão de GPv2.

**São armazenamento VNET e regras de firewall suportadas no novo ponto final web?**  
Sim, o novo ponto final da web obedeça as regras VNET e de firewall configuradas para a conta de armazenamento.

## <a name="next-steps"></a>Passos Seguintes
* [Utilizar a CDN do Azure para aceder a blobs com domínios personalizados através de HTTPS](storage-https-custom-domain-cdn.md)
* [Configurar um nome de domínio personalizado para o ponto final do blob ou web](storage-custom-domain-name.md)
* [Funções do Azure](/azure-functions/functions-overview.md)
* [Aplicações Web do Azure](/app-service/app-service-web-overview.md)
* [Crie seu primeiro aplicativo da web sem servidor](https://aka.ms/static-serverless-webapp)
