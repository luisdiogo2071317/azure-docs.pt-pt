---
title: incluir ficheiro
description: incluir ficheiro
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 01/25/2019
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: b335cf996de41f4eea15af1899a0c6654c2f679f
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/28/2019
ms.locfileid: "55104988"
---
## <a name="open-cli-shell"></a>Shell CLI aberto

É recomendado que utilize [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest) para executar comandos da CLI. **Cloud Shell** é uma shell gratuita e interativa que pode utilizar para executar os passos neste artigo. As ferramentas comuns do Azure estão pré-instaladas e configuradas no Cloud Shell para que possa utilizá-las com a sua conta. Basta selecione o botão de cópia para copiar o código, colá-lo no Cloud Shell e, em seguida, prima Enter para executá-lo. Existem algumas formas de abrir o Cloud Shell:

Se optar por instalar e utilizar a CLI localmente, este artigo requer a execução da versão 2.0 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 

### <a name="login"></a>Iniciar sessão

Para começar a utilizar a shell CLI (na nuvem ou localmente), execute `az login` para criar uma ligação com o Azure.

Se a CLI conseguir abrir o seu browser predefinido, executa essa ação e carrega uma página de início de sessão. Caso contrário, terá de abrir uma página do browser e siga as instruções na linha de comandos para introduzir um código de autorização depois de navegar para https://aka.ms/devicelogin no seu browser.

### <a name="specify-location-of-files"></a>Especifique a localização dos ficheiros

Muitos comandos da CLI de serviços de multimédia permitem-lhe transmitir um parâmetro com um nome de ficheiro. 

Se estiver a utilizar **Azure Cloud Shell**, carregar o ficheiro para **Azure Cloud Shell**. Pode encontrar o botão de ficheiros de carregamento/transferência na parte superior da janela do shell. Em seguida, referenciar o ficheiro como este: `@{FileName}.` 

![Carregar ficheiros]

Se estiver a utilizar a CLI do Azure localmente, especifique o caminho de ficheiro completo. Por exemplo, `@c:\tracks.json`.


[Carregar ficheiros]: ./media/media-services-cli/upload-download-files.png
