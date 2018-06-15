---
title: App Service do Azure na implementação de Linux - imagem personalizada, contentor multi ou imagem de plataforma incorporada?  | Microsoft Docs
description: Como escolher entre implementação do contentor de Docker personalizada, contentor multi e uma estrutura de aplicação incorporada para o serviço de aplicações no Linux
keywords: serviço de aplicações do Azure, aplicação web, linux, oss
services: app-service
documentationCenter: ''
authors: msangapu
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2018
ms.author: msangapu
ms.openlocfilehash: 012f78fc07f237e8ed532246c81a3c86bb6ab4ac
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33764347"
---
# <a name="custom-image-multi-container-or-built-in-platform-image"></a>Imagem personalizada, contentor multi ou imagem de plataforma incorporada?

[Serviço de aplicações no Linux](app-service-linux-intro.md) oferece três caminhos diferentes para obter a aplicação publicada para o web:

- **Implementação de imagem personalizada**: "Dockerize" a sua aplicação para uma imagem de Docker que contém todos os seus ficheiros e dependências de um pacote de prontos para execução.
- **Implementação do contentor Multi**: "Dockerize" a sua aplicação através de vários contentores utilizando um Docker Compose ou um ficheiro de configuração Kubernetes. Para obter mais informações, consulte [contentor multi aplicação](#multi-container-apps-supportability).
- **Implementação de aplicação com uma imagem de plataforma incorporada**: imagens nossa plataforma incorporada contenham tempos de execução de aplicações comuns web e dependências, tal como o nó e o PHP. Utilizar qualquer um do [métodos de implementação do App Service do Azure](../app-service-deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) para implementar a aplicação no armazenamento da sua aplicação web e, em seguida, utilizar uma imagem de plataforma incorporadas para executá-la.

##<a name="which-method-is-right-for-your-app"></a>O método é mais adequado para a sua aplicação? 

Os principais fatores a considerar são:

- **Disponibilidade do Docker no seu fluxo de trabalho de desenvolvimento**: programação de imagem personalizada requer o conhecimento básico do fluxo de trabalho de desenvolvimento do Docker. Implementação de uma imagem personalizada para uma aplicação web precisa de publicação da sua imagem personalizada para um anfitrião do repositório, como o Hub de Docker. Se estiver familiarizado com o Docker e pode adicionar tarefas de Docker ao seu fluxo de trabalho de compilação, ou se já está a publicar a aplicação como uma imagem de Docker, uma imagem personalizada quase certamente é a melhor opção.
- **Arquitetura por várias camadas**: implementar vários contentores, tais como uma camada de aplicação web e uma camada de API para separar as capacidades utilizando multi contentor. 
- **Desempenho da aplicação**: aumentar o desempenho da sua aplicação de contentor multi através de uma camada de cache, tais como Redis. Selecione um contentor multi alcançar isto.
- **Requisitos de tempo de execução exclusivo**: as imagens de plataforma incorporadas foram concebidas para satisfazer as necessidades da maioria das aplicações web, mas são limitadas na respetiva customizability. A aplicação pode ter dependências exclusivas ou outros requisitos de tempo de execução que excedem o que são capazes das imagens incorporadas.
- **Criar requisitos**: com [a implementação contínua](../app-service-continuous-deployment.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), pode obter a aplicação em execução no Azure diretamente a partir do código de origem. Não é necessário nenhum processo de compilação ou publicação externo. No entanto, há um limite para o customizability e a disponibilidade das ferramentas de compilação dentro de [Kudu](https://github.com/projectkudu/kudu/wiki) motor de implementação. A aplicação poderá outgrow capacidades do Kudu medida que for crescendo na sua dependências ou requisitos para a lógica de compilação personalizado.
- **Requisitos de leitura/escrita do disco**: todas as aplicações web são atribuídas um volume de armazenamento de conteúdo web. Este volume, pelo Storage do Azure, está montado para `/home` no sistema de ficheiros da aplicação. Ao contrário dos ficheiros de sistema de ficheiros de contentor, ficheiros no volume de conteúdo estão acessíveis em todas as instâncias de escala de uma aplicação e modificações serão mantidas entre reinícios da aplicação. No entanto, a latência de disco do volume de conteúdo é superior e variável mais que a latência do sistema de ficheiros do contentor de locais e acesso pode ser afetado por atualizações de plataforma, tempo de inatividade não planeado e problemas de conectividade de rede. Aplicações que necessitam de muita acesso só de leitura aos ficheiros de conteúdo podem beneficiar da implementação da imagem personalizada, que coloca os ficheiros no sistema de ficheiros imagem em vez de no volume de conteúdo.
- **Utilização de recursos de compilação**: quando uma aplicação é implementada a partir da origem, os scripts de implementação executados através da utilização do Kudu os mesmos plano do App Service computação e recursos de armazenamento que a aplicação em execução. Implementações de aplicações de grandes dimensões podem consumir mais recursos ou o tempo que o pretendido. Em particular, muitos fluxos de trabalho de implementação geram atividade pesada disco no volume de conteúdo app, que não está optimizada para essa atividade. Uma imagem personalizada fornece todos os ficheiros e as dependências da sua aplicação no Azure num pacote único com não é necessário para transferências de ficheiros adicionais ou ações de implementação.
- **Necessário para iteração rápida**: Dockerizing uma aplicação requer passos de compilação adicionais. Para que as alterações entrem em vigor, tem de emitir a nova imagem para um repositório com cada atualização. Estas atualizações são solicitadas, em seguida, para o ambiente do Azure. Se um dos contentores incorporados se adeque às necessidades da sua aplicação, implementação a partir da origem poderão oferecer um fluxo de trabalho de desenvolvimento mais rápido.

## <a name="multi-container-apps-supportability"></a>Suporte de aplicações de várias contentor

### <a name="supported-docker-compose-configuration-options"></a>Opções de configuração Docker Compose suportadas
- Comando
- ponto de entrada
- environment
- Imagem
- Portas
- Reiniciar
- Serviços
- Volumes

### <a name="unsupported-docker-compose-configuration-options"></a>Opções de configuração Docker Compose não suportadas
- compilação (não é permitida)
- depends_on (ignorados)
- redes (ignorados)
- segredos (ignorados)
- portas que não sejam 80 e 8080 (ignorados)

> [!NOTE]
> Quaisquer outras opções não explicitamente referidas também são ignoradas em pré-visualização pública.

### <a name="supported-kubernetes-configuration-options"></a>Opções de configuração de Kubernetes suportadas
- args
- Comando
- containers
- Imagem
- nome
- Portas
- spec

> [!NOTE]
>Quaisquer outras opções de Kubernetes não explicitamente realçadas não são suportadas na pré-visualização pública.
>