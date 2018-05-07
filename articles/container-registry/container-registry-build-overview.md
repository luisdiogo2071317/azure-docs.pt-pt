---
title: Automatizar SO e aplicação de patches framework com o Azure contentor registo criar (ACR criar)
description: Uma introdução ao criar o ACR, um conjunto de funcionalidades no registo de contentor do Azure que fornece a compilação de imagem do contentor segura, automatizada e aplicação de patches na nuvem.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 05/01/2018
ms.author: marsma
ms.openlocfilehash: 7506351c0d65fb167136478d2dc383cdabd81835
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
---
# <a name="automate-os-and-framework-patching-with-acr-build"></a>Automatizar SO e aplicação de patches framework com ACR de compilação

Contentores fornecem novos níveis de virtualização, isolando os programadores de aplicações e dependências de infraestrutura e a requisitos operacionais. No entanto, que permanece, é necessário para resolver como virtualização esta aplicação é aplicada.

**Criar ACR**, um conjunto de funcionalidades no registo de contentor do Azure, fornece não apenas contentor nativo compilação capacidade de imagem, mas também automatiza [SO e aplicação de patches do framework](#automate-os-and-framework-patching) para os contentores de Docker.

> [!IMPORTANT]
> Criar ACR está a ser atualmente em pré-visualização e só é suportada pelos registos do contentor do Azure no **EUA Leste** e **Europa Ocidental** regiões. Pré-visualizações ficam disponíveis para si condition que está a concordar com o [termos de utilização suplementares][terms-of-use]. Alguns aspetos desta funcionalidade podem alterar-se após a disponibilidade geral (GA).

## <a name="what-is-acr-build"></a>O que é ACR criar?

Registo de contentor do Azure compilação é um serviço de compilação de imagem de contentor do Azure nativo. Criar ACR desenvolvimento de ciclo interna ativa na nuvem com a imagem do contentor a pedido baseia-se e compilações automatizadas na imagem de consolidação e a base de código de origem de atualização.

Imagem de contentor do acionador baseia-se automaticamente quando o código é submetido para um repositório de Git, ou quando a imagem de base de um contentor é atualizada. Com os acionadores de atualização da imagem base, pode automatizar o SO e estrutura da aplicação de fluxo de trabalho, manter ambientes seguras ao cumprir os principais de contentores imutáveis de aplicação de patches.

## <a name="quick-build-inner-loop-extended-to-the-cloud"></a>Compilação rápida: interna-ciclo expandido para a nuvem

Inicia o início do ciclo de vida de gestão antes dos programadores consolidar os respetivos primeiro linhas de código. Do ACR de compilação [criar rápida](container-registry-tutorial-quick-build.md) funcionalidade permite uma experiência de desenvolvimento integrado de ciclo interna local, a descarga de compilações para o Azure. Com compilações rápido, pode verificar as definições de compilação automatizada antes de consolidar o seu código.

Utilizar o familiar `docker build` formato, o [compilação de acr az] [ az-acr-build] comando da CLI do Azure assume um contexto local, envia-a para o serviço de compilação ACR e, por predefinição, pushes a imagem incorporada no respetivo registo após conclusão. Criar ACR segue os registos georreplicação, ativar as equipas de desenvolvimento dispersos tirar partido do registo replicado mais próximo. Durante a pré-visualização, compilação ACR está disponível nas regiões EUA leste e na Europa Ocidental.

Criar ACR foi concebido como um ciclo de vida de contentor primitivo. Por exemplo, integre ACR compilar a solução de CI/CD. Executando [início de sessão az] [ az-login] com um [principal de serviço][az-login-service-principal], a solução de CI/CD, em seguida, pode emitir [az acr criar] [ az-acr-build] compilações de imagem de comandos para iniciar.

Saiba como utilizar rápida cria primeiro tutorial ACR criar, [criar imagens de contentor na nuvem com o registo de contentor do Azure compilação](container-registry-tutorial-quick-build.md).

## <a name="automatic-build-on-source-code-commit"></a>Compilação automática na consolidação de código de origem

Utilizar ACR compilação para acionar automaticamente uma imagem do contentor de compilação quando o código é submetido para um repositório de Git. Criar tarefas, configuráveis com o comando da CLI do Azure [tarefa de compilação do acr az][az-acr-build-task], permitem-lhe especificar um repositório de Git e opcionalmente um ramo e Dockerfile. Quando a sua equipa consolida código para o repositório, um webhook criados ACR criar aciona uma compilação da imagem do contentor definida no repositório.

Saiba como acionar compilações na consolidação de código de origem no tutorial ACR criar segundo, [imagem de contentor Automate baseia-se com o registo de contentor do Azure compilação](container-registry-tutorial-build-task.md).

## <a name="automate-os-and-framework-patching"></a>Automatizar SO e aplicação de patches do framework

Potência de compilação ACR para melhorar verdadeiramente o pipeline de compilação do contentor for proveniente da respetiva capacidade de detetar uma atualização a uma imagem de base. Quando a imagem base atualizada está instalada no seu registo, ACR compilar automaticamente pode criar as imagens de aplicação baseadas nas mesmas.

Imagens de contentor podem ser amplamente categorizadas em *base* imagens e *aplicação* imagens. As imagens de base incluem, geralmente, o sistema operativo e sobre os quais é construída a aplicação, juntamente com outras personalizações de estruturas de aplicações. Estas imagens base são próprios normalmente com base nas imagens montante públicas, por exemplo [Linux Extreme] [ base-alpine] ou [Node.js][base-node]. Várias das suas imagens de aplicação podem partilhar uma imagem de base comum.

Quando uma imagem de arquitetura do SO ou aplicação é atualizada, o responsável pela manutenção a montante, por exemplo com um patch de segurança crítica do SO, tem também de atualizar as imagens de base para incluir a correção crítico. Cada imagem de aplicação, em seguida, deve também ser reconstruída para incluir estas correções montante agora incluídas na imagem de base.

Porque dinamicamente ACR criar Deteta a imagem base dependências quando baseia-se para uma imagem de contentor, pode detetar quando a imagem de base de uma imagem de aplicação é atualizada. Com um pré-configurada [criar tarefas](container-registry-tutorial-base-image-update.md#create-build-task), ACR, em seguida, criar **recria automaticamente a cada imagem de aplicação** por si. Com esta deteção automática e reconstrução, guarda ACR criar tem do tempo e esforço normalmente manualmente controlar e atualizar todas as aplicações da imagem de referência da imagem base atualizada.

Saiba mais sobre o SO e aplicação de patches framework tutorial ACR criar terceiro, [Automate imagem baseia-se na atualização da imagem base com o registo de contentor do Azure compilação](container-registry-tutorial-base-image-update.md).

> [!NOTE]
> Para a pré-visualização inicial, acionador de atualizações de imagem base baseia-se apenas quando ambas as imagens de base e de aplicações de residir no mesmo registo de contentor do Azure.

## <a name="next-steps"></a>Passos Seguintes

Quando estiver pronto para automatizar o SO e a arquitetura de aplicação de patches pelo criar imagens contentor na nuvem, consulte o três partes ACR criar tutorial da série.

> [!div class="nextstepaction"]
> [Criar imagens de contentor na nuvem com o registo de contentor do Azure compilação](container-registry-tutorial-quick-build.md)

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-build-task]: /cli/azure/acr#az-acr-build-task
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli#log-in-with-a-service-principal

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
