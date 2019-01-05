---
title: Utilizar o Habitat para implementar a aplicação no Azure
description: Aprenda a implementar de forma consistente a sua aplicação para máquinas virtuais do Azure e contentores
keywords: Azure, chef, devops, máquinas virtuais, visão geral, automatizar, habitat
ms.service: virtual-machines-linux
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: 4847d9ce551c9acf1e4fb6325c770187b2cfd89f
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54052296"
---
# <a name="use-habitat-to-deploy-your-application-to-azure"></a>Utilizar o Habitat para implementar a aplicação no Azure
[Habitat](https://www.habitat.sh/) é um primeiro de seu projeto de código aberto tipo, que oferece uma abordagem totalmente nova à gestão de aplicações. Habitat faz com que o aplicativo e sua automação a unidade de implementação. Quando os aplicativos são compactados num simples "habitat", o ambiente de tempo de execução, quer se trate de um contentor, bare-metal ou PaaS, já não é o foco e não restrinja o aplicativo. 

Este artigo descreve as vantagens de utilizar o Habitat.

## <a name="support-for-the-modern-application"></a>Suporte para a aplicação moderna
Pacotes do habitat incluem tudo o que um aplicativo precisa ser executado em todo o ciclo. Componentes de núcleo do habitat são:
- Formato de empacotamento - aplicativos num pacote de Habitat são atômicas, imutável e auditáveis.
- O supervisor de Habitat executa pacotes de aplicações com deteção de elemento de rede dos pacotes de relações, estratégia de atualização e as políticas de segurança. O supervisor de Habitat configura e gere a aplicação para qualquer ambiente está presente.
- O ecossistema de Habitat também fornece um serviço de compilação que usa um plano de compilação do Habitat, cria o pacote de Habitat e publica-o para um depósito.

## <a name="run-any-application-anywhere"></a>Executar qualquer aplicação em qualquer lugar
Com o Habitat, aplicativos podem ser executados sem modificações em qualquer ambiente de tempo de execução. O aplicativo pode ser qualquer coisa do bare-metal e máquinas virtuais para contentores (por exemplo, o Docker), sistemas de gerenciamento de cluster (por exemplo, Mesosphere ou Kubernetes) e sistemas de PaaS (por exemplo, o Cloud Foundry da Pivotal).

## <a name="easily-port-legacy-applications"></a>Facilmente porta aplicativos herdados
Quando os aplicativos herdados são compactados num pacote de Habitat, os aplicativos são independentes do ambiente para o qual foram originalmente criados. Os pacotes rapidamente podem ser movidos para ambientes mais modernos, como a cloud ou contentores. Além disso, como pacotes do Habitat têm uma interface com acesso padrão, exterior, os aplicativos herdados tornam-se muito mais fácil de gerir.

## <a name="improve-the-container-experience"></a>Melhorar a experiência de contentor
Habitat reduz a complexidade da gestão de contentores em ambientes de produção. Ao automatizar a configuração da aplicação dentro de um contêiner, Habitat resolve os desafios enfrentados pelos desenvolvedores ao mover aplicações baseadas em contentores de ambientes de desenvolvimento para a produção.

## <a name="integrate-into-the-chef-devops-workflow"></a>Integrar o fluxo de trabalho de DevOps do Chef
O projeto de Habitat é patrocinado pela Chef. Habitat tira partido experiência do Chef em automatização de infraestruturas para trazer capacidades de automatização sem precedentes para os aplicativos. Chef irá oferecer suporte comercial para Habitat e certifique-se de uma integração perfeita entre Habitat e entrega do Chef para automatizar o ciclo de lançamento de aplicação, desde o desenvolvimento para a implementação.

## <a name="next-steps"></a>Passos Seguintes
* [Criar uma máquina virtual do Windows no Azure utilizando o Chef](/azure/virtual-machines/windows/chef-automation)