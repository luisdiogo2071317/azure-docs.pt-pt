---
title: Alojamento de Web site estático no armazenamento do Azure (pré-visualização) | Microsoft Docs
description: Agora, o Storage do Azure oferece Web site estático (pré-visualização) de alojamento, fornecendo uma solução económica e dimensionável para o alojamento de aplicações web moderna.
services: storage
author: MichaelHauss
manager: vamshik
ms.service: storage
ms.topic: article
ms.date: 06/26/18
ms.author: mihauss
ms.openlocfilehash: 7021a0499547818d702d14aecb9d8e451a820181
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37025939"
---
# <a name="static-website-hosting-in-azure-storage-preview"></a>Alojamento de Web site estático no armazenamento do Azure (pré-visualização)
Agora, o Storage do Azure oferece Web site estático (pré-visualização) de alojamento, permitindo-lhe implementar aplicações web moderna económica e dimensionável no Azure. Num Web site estático, páginas Web contém conteúdo estático e JavaScript ou outro código do lado do cliente. Por outro lado, Web sites dinâmicos dependem do código do lado do servidor e pode ser alojados utilizando [Web Apps do Azure](/app-service/app-service-web-overview.md).

Como implementações de deslocar para modelos elásticos e rentáveis, a capacidade para a entrega de conteúdo web sem a necessidade de gestão do servidor é fundamental. A introdução de alojamento de Web site estático no armazenamento do Azure faz com que esta possíveis, a ativação de funções de back-end avançada com arquiteturas sem servidor tirar partido [das funções do Azure](/azure-functions/functions-overview.md) e outros serviços de PaaS.

## <a name="how-does-it-work"></a>Como funciona?
Quando ativar os Web sites estáticos na sua conta de armazenamento, um novo ponto de final de serviço de web é criado com o formato `<account-name>.<zone-name>.web.core.windows.net`.

Ponto final do serviço web sempre permite acesso de leitura anónimo, devolve formatadas de HTML de páginas em resposta a erros de serviço e permite apenas operações de leitura de objeto. Ponto final do serviço web devolve o documento de índice no pedido diretório de raiz e todos os seus subdiretórios. Quando o serviço de armazenamento devolve um erro 404, o ponto final web devolve um erro personalizado documento caso esteja configurado.

Conteúdo para o Web site estático está localizado num contentor especial com o nome "$web". Como parte do processo de ativação, "$web" é criado para si se já existir. O conteúdo "$web" pode ser acedido na raiz de conta a utilizar o ponto final da web. Por exemplo `https://contoso.z4.web.core.windows.net/` devolve o documento de índice que configurou para o seu site, se existir um documento com esse nome no diretório de raiz de $web.

Quando carregar conteúdo para o seu Web site, utilize o ponto final do blob storage. Para carregar um blob com o nome 'image.jpg', que podem ser acedidos na raiz conta utilize o seguinte URL `https://contoso.blob.core.windows.net/$web/image.jpg`. A imagem carregada pode ser visualizada num ponto de final correspondente do web browser `https://contoso.z4.web.core.windows.net/image.jpg`.


## <a name="custom-domain-names"></a>Nomes de domínio personalizados
Pode utilizar um domínio personalizado para alojar o seu conteúdo web. Para tal, siga as orientações no [configurar um nome de domínio personalizado para a sua conta do Storage do Azure](storage-custom-domain-name.md). Para aceder ao seu Web site alojada em nome de domínio personalizado através de HTTPS, consulte [utilizar a CDN do Azure para aceder a blobs com domínios personalizados através de HTTPS](storage-https-custom-domain-cdn.md).

## <a name="pricing-and-billing"></a>Preços e faturação
Alojamento de Web site estático é fornecida sem custos adicionais. Para obter mais detalhes sobre os preços para o armazenamento de Blobs do Azure, veja o [página de preços do Azure Blob Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="quickstart"></a>Início Rápido
### <a name="azure-portal"></a>Portal do Azure
Para iniciar o alojamento da sua aplicação web no armazenamento do Azure, pode configurar a funcionalidade com o Portal do Azure e clique em "Web site estático (pré-visualização)" em "Definições", na barra de navegação esquerdo. Clique em "Ativada" e introduza o nome do documento índice e (opcionalmente) o caminho do documento de erros personalizados.

![](media/storage-blob-static-website/storage-blob-static-website-portal-config.PNG)

Carregar os recursos de web para o contentor "$web" que foi criado como parte da ativação do Web site estático. Pode fazê-lo diretamente no Portal do Azure ou, pode tirar partido de [Explorador de armazenamento do Azure](https://azure.microsoft.com/features/storage-explorer/) para carregar as estruturas de diretório de todo. Certifique-se de que inclui um documento de índice com o nome que configurou. Neste exemplo, o nome do documento é "index.html".

Por fim, navegue para o ponto final de web para testar o seu Web site.

## <a name="faq"></a>FAQ
**Os Web sites estáticos está disponível para todos os tipos de conta de armazenamento?**  
Não, só está disponível nas contas de armazenamento standard GPv2 alojamento de Web site estático.

**São VNET de armazenamento e as regras de firewall suportadas no ponto final da web de novo?**  
Sim, o novo ponto final da web obeys as regras VNET e de firewall configuradas para a conta de armazenamento.

## <a name="next-steps"></a>Passos Seguintes
* [Utilizar a CDN do Azure para aceder a blobs com domínios personalizados através de HTTPS](storage-https-custom-domain-cdn.md)
* [Configurar um nome de domínio personalizado para o ponto final do blob ou da web](storage-custom-domain-name.md)
* [Funções do Azure](/azure-functions/functions-overview.md)
* [Aplicações Web do Azure](/app-service/app-service-web-overview.md)
* [Criar a sua primeira aplicação sem servidor web](https://aka.ms/static-serverless-webapp)
