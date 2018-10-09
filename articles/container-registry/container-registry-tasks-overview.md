---
title: Automatizar o SO e aplicação de patches de estrutura com tarefas de registo de contentor do Azure (ACR tarefas)
description: Uma introdução às tarefas do ACR, um conjunto de recursos do Azure Container Registry, que fornece a criação da imagem do contentor seguras e automatizadas e aplicação de patches na cloud.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 09/24/2018
ms.author: danlep
ms.openlocfilehash: 63affd4ad22d5246274ddfa3160d5675f702003f
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48855769"
---
# <a name="automate-os-and-framework-patching-with-acr-tasks"></a>Automatizar o SO e aplicação de patches de estrutura com tarefas do ACR

Os contentores oferecem novos níveis de virtualização, isolando as dependências de aplicativo e o desenvolvedor da infraestrutura e os requisitos operacionais. No entanto, o que permanece, é a necessidade de abordar como a virtualização de aplicativo é corrigida.

## <a name="what-is-acr-tasks"></a>O que é o ACR tarefas?

**Tarefas de ACR** é um conjunto de recursos no Azure Container Registry. Fornece a criação de imagem de contentor com base na cloud para Linux, Windows e ARM e pode automatizar [SO e aplicação de patches do framework](#automate-os-and-framework-patching) para os seus contentores do Docker. Tarefas de ACR não apenas expande o seu ciclo de desenvolvimento de "loop interno" para a cloud com compilações de imagem de contentor a pedido, mas também permite que as compilações automáticas em consolidação de código de origem ou quando a imagem de base de um contentor é atualizada. Com os acionadores de atualização da imagem base, pode automatizar o seu sistema operacional e a estrutura da aplicação de fluxo de trabalho, a manutenção de ambientes de seguros enquanto adere a entidades de segurança dos contentores imutáveis de aplicação de patches.

Criar e testar imagens de contentor com o ACR tarefas de quatro formas:

* [Tarefas rápidas](#quick-task): criar e enviar por push contentor imagens sob demanda, no Azure, sem precisar de uma instalação local do motor do Docker. Pense `docker build`, `docker push` na cloud. Compilação a partir do código-fonte local ou um repositório de Git.
* [Criar em consolidação de código de origem](#automatic-build-on-source-code-commit): acionar uma compilação de imagem de contentor automaticamente quando o código é consolidado para um repositório de Git.
* [Criar na atualização da imagem base](#automate-os-and-framework-patching): acionar uma compilação de imagem de contentor quando tiver sido atualizada a imagem de base dessa imagem.
* [Tarefas de vários passos](#multi-step-tasks-preview) (pré-visualização): definir tarefas de vários passos que criar imagens, executam contentores como comandos e enviar imagens para um registo. Esta funcionalidade de pré-visualização das tarefas de ACR suporta a execução da tarefa a pedido e compilação de imagem paralela, testar e operações de push.

## <a name="quick-task"></a>Tarefas rápidas

O ciclo de desenvolvimento de loop interno, o processo interativo de escrita de código, criar e testar seu aplicativo antes de consolidar ao controlo de origem, é realmente o início do gerenciamento de ciclo de vida do contentor.

Antes de consolidar a primeira linha de código, tarefas de ACR [tarefas rápidas](container-registry-tutorial-quick-task.md) funcionalidade pode fornecer uma experiência de desenvolvimento integrado ao descarregar a sua imagem de contentor baseia-se para o Azure. Com tarefas rápidas, pode verificar seu automatizada definições de compilação e detetar potenciais problemas antes de consolidar a seu código.

Usando os familiares `docker build` formato, o [compilação do az acr] [ az-acr-build] comando na CLI do Azure demora um *contexto* (o conjunto de ficheiros para criar), envia-os ACR tarefas e, por predefinição, envia a imagem criada para o registo após a conclusão.

A tabela seguinte mostra alguns exemplos de localizações de contexto suportados para tarefas de ACR:

| Localização de contexto | Descrição | Exemplo |
| ---------------- | ----------- | ------- |
| Sistema de ficheiros local | Ficheiros dentro de um diretório no sistema de ficheiros local. | `/home/user/projects/myapp` |
| Ramo principal do GitHub | Ramo de arquivos dentro do mestre (ou outro padrão) de um repositório do GitHub.  | `https://github.com/gituser/myapp-repo.git` |
| Ramo do GitHub | Ramo específico de um repositório do GitHub.| `https://github.com/gituser/myapp-repo.git#mybranch` |
| Pedido Pull do GitHub | Pedido Pull num repositório do GitHub. | `https://github.com/gituser/myapp-repo.git#pull/23/head` |
| Subpasta do GitHub | Arquivos numa subpasta num repositório do GitHub. Exemplo mostra a combinação da especificação de PR e subpasta. | `https://github.com/gituser/myapp-repo.git#pull/24/head:myfolder` |
| Tarball remoto | Arquivos num arquivo compactado num servidor Web remoto. | `http://remoteserver/myapp.tar.gz` |

Tarefas de ACR foi concebida como um ciclo de vida de contentor primitivo. Por exemplo, integre ACR tarefas na sua solução de CI/CD. Executando [início de sessão az] [ az-login] com um [principal de serviço][az-login-service-principal], sua solução de CI/CD, em seguida, poderia emitir [az acr build] [ az-acr-build] compilações de imagem de comandos para iniciar.

Saiba como utilizar tarefas rápidas no primeiro tutorial de tarefas do ACR, [criar imagens de contentor na cloud com o Azure Container Registry tarefas](container-registry-tutorial-quick-task.md).

## <a name="automatic-build-on-source-code-commit"></a>Compilação automática em consolidação de código de origem

Utilize tarefas de ACR para acionar automaticamente uma imagem de contentor criar quando o código ser enviado para um repositório de Git. Criar tarefas, configuráveis com o comando da CLI do Azure [tarefas de acr az][az-acr-task], permitem-lhe especificar um repositório de Git e, opcionalmente, um ramo e Dockerfile. Quando a sua equipa de consolidações código para o repositório, um webhook criado para tarefas de ACR aciona uma compilação da imagem de contentor definida no repositório.

> [!IMPORTANT]
> Se tiver criado anteriormente tarefas durante a pré-visualização com o `az acr build-task` comando, essas tarefas têm de ser recriado com o [tarefa do az acr] [ az-acr-task] comando.

Saiba como acionar compilações em consolidação de código de origem no segundo tutorial de tarefas do ACR, [compilações de imagem de contentor de automatizar com tarefas de registo de contentor do Azure](container-registry-tutorial-build-task.md).

## <a name="automate-os-and-framework-patching"></a>Automatizar o sistema operacional e a aplicação de patches do framework

O poder das tarefas do ACR para realmente aprimorar seu fluxo de trabalho de compilação do contentor é proveniente de sua capacidade de detetar uma atualização para uma imagem base. Quando a imagem base atualizada é emitida para o seu registo, o ACR tarefas pode criar automaticamente quaisquer imagens de aplicação com base no mesmo.

Podem ser amplamente categorizadas em imagens de contentor *base* imagens e *aplicação* imagens. As imagens bases incluem, geralmente, o sistema operativo e as estruturas de aplicativo no qual a aplicação é criada, juntamente com outras personalizações. Estas imagens de bases são normalmente baseadas nas imagens de montante públicas, por exemplo: [Alpine Linux][base-alpine], [Windows][base-windows], [.NET][base-dotnet], ou [node. js][base-node]. Várias das suas imagens de aplicação podem partilhar uma imagem de base comum.

Quando uma imagem de estrutura de aplicação ou sistema operacional é atualizada, o responsável pela manutenção a montante, por exemplo com um patch de segurança crítico do sistema operacional, tem também de atualizar suas imagens base para incluir a correção crítica. Cada imagem de aplicação, em seguida, deve também ser reconstruída para incluir essas correções a montante agora incluídas em sua imagem base.

Uma vez que as tarefas de ACR Deteta dinamicamente dependências de imagem de base quando ele cria uma imagem de contentor, pode detetar quando a imagem de base de uma imagem de aplicação é atualizada. Com um pré-configurada [tarefa de compilação](container-registry-tutorial-base-image-update.md#create-a-task), tarefas de ACR, em seguida, **recria automaticamente a cada imagem de aplicativo** para. Com esta deteção automática e reconstrução, poupa de tarefas do ACR, criar o tempo e esforço normalmente necessário para controlar e atualizar cada aplicação manualmente a imagem de referência a sua imagem de base atualizada.

Saiba mais sobre o sistema operacional e a aplicação de patches de estrutura no terceiro tutorial de tarefas do ACR, [automatizar imagem baseia-se na imagem base a atualização com o Azure Container Registry tarefas](container-registry-tutorial-base-image-update.md).

> [!NOTE]
> Imagem de base de atualizações de compilações de Acionador apenas quando tanto as imagens bases e de aplicações residem no mesmo registo de contentor do Azure ou a base de reside num repositório do Docker Hub público.

## <a name="multi-step-tasks-preview"></a>Tarefas de vários passos (pré-visualização)

Tarefas de vários passos, uma funcionalidade de pré-visualização de tarefas do ACR, fornece a definição de tarefa com base no passo e execução para criação, teste e aplicar patches em imagens de contentor na cloud. Passos de tarefas definem compilação de imagem de contentor individual e operações de push. Também podem definir a execução de um ou mais contentores, com cada passo com o contentor de como o seu ambiente de execução.

Por exemplo, pode criar uma tarefa de várias etapa que automatiza o seguinte:

1. Crie uma imagem de aplicação web
1. Executar o contentor da aplicação web
1. Crie uma imagem de teste de aplicações web
1. Executar o contentor de teste de aplicativos web que executa os testes em relação a execução do contentor da aplicação
1. Se os testes forem bem-sucedidos, criar um pacote de arquivo do gráfico Helm
1. Executar um `helm upgrade` usando o novo pacote de arquivo do gráfico Helm

Tarefas de vários passos permitem-lhe dividir a criação, execução e teste de uma imagem em etapas mais compostas, com suporte de dependência entre passo. Com tarefas de várias etapas nas tarefas do ACR, tem controle mais granular sobre a criação de imagem, teste e sistema operacional e estrutura de fluxos de trabalho de aplicação de patches.

Saiba mais sobre tarefas de vários passos em [executar vários passo compilação, teste e as tarefas de patch no ACR tarefas](container-registry-tasks-multi-step.md).

> [!IMPORTANT]
> A capacidade de tarefa de várias etapas das tarefas de ACR está atualmente em pré-visualização. As pré-visualizações ser-lhe-ão disponibilizadas na condição de concordar com os [termos suplementares de utilização][terms-of-use]. Alguns aspetos desta capacidade podem ser alterados antes da disponibilidade geral (GA)

## <a name="next-steps"></a>Passos Seguintes

Quando estiver pronto para automatizar o sistema operacional e a estrutura de aplicação de patches ao criar as imagens de contentor na cloud, veja a série de tutoriais de tarefas de ACR de três partes.

> [!div class="nextstepaction"]
> [Criar imagens de contentor na cloud com tarefas de registo de contentor do Azure](container-registry-tutorial-quick-task.md)

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
[az-acr-task]: /cli/azure/acr#az-acr-task
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli#log-in-with-a-service-principal

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png