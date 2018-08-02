---
title: Automatizar o SO e aplicação de patches de framework com a criação do Azure Container Registry (ACR Build)
description: Uma introdução à criação do ACR, um conjunto de recursos do Azure Container Registry, que fornece a criação da imagem do contentor seguras e automatizadas e aplicação de patches na cloud.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 08/01/2018
ms.author: marsma
ms.openlocfilehash: 63bbd9b5711330207c34ac4aa05aac3a71304653
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413584"
---
# <a name="automate-os-and-framework-patching-with-acr-build"></a>Automatizar o SO e aplicação de patches de estrutura com o ACR Build

Os contentores oferecem novos níveis de virtualização, isolando as dependências de aplicativo e o desenvolvedor da infraestrutura e os requisitos operacionais. No entanto, o que permanece, é a necessidade de abordar como a virtualização de aplicativo é corrigida.

**ACR Build** é um conjunto de recursos no Azure Container Registry. Fornece a criação de imagem de contentor com base na cloud para Linux, Windows e ARM e pode automatizar [SO e aplicação de patches do framework](#automate-os-and-framework-patching) para os seus contentores do Docker.

[!INCLUDE [container-registry-build-preview-note](../../includes/container-registry-build-preview-note.md)]

## <a name="what-is-acr-build"></a>O que é o ACR Build?

Registo de contentor do Azure Build é um serviço de compilação de imagens de contentor nativa do Azure. Baseia-se ACR Build para desenvolvimento de loop interno ativa na cloud com a imagem de contentor sob demanda e compilações automatizadas numa imagem de consolidação e a base de código de origem de atualização.

Imagem de contentor do acionador baseia-se automaticamente quando o código ser enviado para um repositório de Git, ou quando a imagem de base de um contentor é atualizada. Com os acionadores de atualização da imagem base, pode automatizar o seu sistema operacional e a estrutura da aplicação de fluxo de trabalho, a manutenção de ambientes de seguros enquanto adere a entidades de segurança dos contentores imutáveis de aplicação de patches.

## <a name="quick-build-inner-loop-extended-to-the-cloud"></a>Compilação rápida: interna-loop expandido para a cloud

Início do ciclo de vida de gestão é iniciado antes dos desenvolvedores confirmar suas primeiras linhas de código. Da ACR Build [rápido criar](container-registry-tutorial-quick-build.md) funcionalidade permite uma experiência de desenvolvimento integrado de loop interno local, a descarga de compilações para o Azure. Com compilações rápidas, pode verificar as definições de compilação automatizada antes de consolidar a seu código.

Usando os familiares `docker build` formato, o [compilação do az acr] [ az-acr-build] comando na CLI do Azure demora um **contexto** (o conjunto de ficheiros para criar), envia-os para o serviço de criação do ACR e, por predefinição, envia a imagem criada para o registo após a conclusão.

A tabela seguinte mostra alguns exemplos de localizações de contexto suportados para o ACR Build:

| Localização de contexto | Descrição | Exemplo |
| ---------------- | ----------- | ------- |
| Sistema de ficheiros local | Ficheiros dentro de um diretório no sistema de ficheiros local. | `/home/user/projects/myapp` |
| Ramo principal do GitHub | Ramo de arquivos dentro do mestre (ou outro padrão) de um repositório do GitHub.  | `https://github.com/gituser/myapp-repo.git` |
| Ramo do GitHub | Ramo específico de um repositório do GitHub.| `https://github.com/gituser/myapp-repo.git#mybranch` |
| Pedido Pull do GitHub | Pedido Pull num repositório do GitHub. | `https://github.com/gituser/myapp-repo.git#pull/23/head` |
| Subpasta do GitHub | Arquivos numa subpasta num repositório do GitHub. Exemplo mostra a combinação da especificação de PR e subpasta. | `https://github.com/gituser/myapp-repo.git#pull/24/head:myfolder` |
| Tarball remoto | Arquivos num arquivo compactado num servidor Web remoto. | `http://remoteserver/myapp.tar.gz` |

ACR Build também segue os registos georreplicado, permitindo que as equipes de desenvolvimento dispersas aproveitar o registro replicado mais próximo.

Criação do ACR foi concebido como um ciclo de vida de contentor primitivo. Por exemplo, integre o ACR Build na sua solução de CI/CD. Executando [início de sessão az] [ az-login] com um [principal de serviço][az-login-service-principal], sua solução de CI/CD, em seguida, poderia emitir [az acr build] [ az-acr-build] compilações de imagem de comandos para iniciar.

Saiba como utilizar o rápido baseia-se no primeiro tutorial ACR Build, [criar imagens de contentor na cloud com o Azure Container Registry Build](container-registry-tutorial-quick-build.md).

## <a name="automatic-build-on-source-code-commit"></a>Compilação automática em consolidação de código de origem

Utilizar o ACR Build para acionar automaticamente uma imagem de contentor criar quando o código ser enviado para um repositório de Git. Criar tarefas, configuráveis com o comando da CLI do Azure [az acr-tarefa de compilação][az-acr-build-task], permitem-lhe especificar um repositório de Git e, opcionalmente, um ramo e Dockerfile. Quando a sua equipa de consolidações código para o repositório, um webhook criado ACR Build aciona uma compilação da imagem de contentor definida no repositório.

Saiba como acionar compilações em consolidação de código de origem no segundo tutorial ACR Build, [compilações de imagem de contentor de automatizar com o Azure Container Registry Build](container-registry-tutorial-build-task.md).

## <a name="automate-os-and-framework-patching"></a>Automatizar o sistema operacional e a aplicação de patches do framework

O poder do ACR Build para realmente aprimorar seu pipeline de compilação do contentor é proveniente de sua capacidade de detetar uma atualização para uma imagem base. Quando a imagem base atualizada é emitida para o seu registo, a criação do ACR automaticamente pode criar quaisquer imagens de aplicação com base no mesmo.

Podem ser amplamente categorizadas em imagens de contentor *base* imagens e *aplicação* imagens. As imagens bases incluem, geralmente, o sistema operativo e as estruturas de aplicativo no qual a aplicação é criada, juntamente com outras personalizações. Estas imagens de bases são normalmente baseadas nas imagens de montante públicas, por exemplo: [Alpine Linux][base-alpine], [Windows][base-windows], [.NET][base-dotnet], ou [node. js][base-node]. Várias das suas imagens de aplicação podem partilhar uma imagem de base comum.

Quando uma imagem de estrutura de aplicação ou sistema operacional é atualizada, o responsável pela manutenção a montante, por exemplo com um patch de segurança crítico do sistema operacional, tem também de atualizar suas imagens base para incluir a correção crítica. Cada imagem de aplicação, em seguida, deve também ser reconstruída para incluir essas correções a montante agora incluídas em sua imagem base.

Uma vez que dinamicamente ACR Build Deteta a imagem base dependências, quando ele cria uma imagem de contentor, pode detetar quando a imagem de base de uma imagem de aplicação é atualizada. Com um pré-configurada [tarefa de compilação](container-registry-tutorial-base-image-update.md#create-build-task), ACR Build, em seguida, **recria automaticamente a cada imagem de aplicativo** para. Com esta deteção automática e reconstrução, poupa ACR Build criar o tempo e esforço normalmente necessário para controlar e atualizar cada aplicação manualmente a imagem de referência a sua imagem de base atualizada.

Saiba mais sobre o SO e aplicação de patches de estrutura no terceiro ACR Build tutorial [automatizar imagem baseia-se na atualização da imagem base com o Azure Container Registry Build](container-registry-tutorial-base-image-update.md).

> [!NOTE]
> Para a pré-visualização inicial, acionador de atualizações de imagem base baseia-se apenas quando ambas as imagens bases e de aplicações residem no mesmo registo de contentor do Azure ou de repositórios do Docker Hub acessíveis publicamente.

## <a name="next-steps"></a>Passos Seguintes

Quando estiver pronto para automatizar o sistema operacional e a estrutura de aplicação de patches ao criar as imagens de contentor na cloud, confira a série de tutoriais do ACR Build de três partes.

> [!div class="nextstepaction"]
> [Compilar imagens de contentor na cloud com o Azure Container Registry Build](container-registry-tutorial-quick-build.md)

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-build-task]: /cli/azure/acr#az-acr-build-task
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli#log-in-with-a-service-principal

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
