---
title: Utilizar Habitat para implementar a aplicação no Azure
description: Saiba como implementar consistentemente a sua aplicação para máquinas virtuais do Azure e contentores
keywords: Azure chef, devops, máquinas virtuais, a descrição geral, automatizar habitat
ms.service: virtual-machines-linux
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: 73c6834eb53c0496fa673dd25ab90abc18a6a139
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="use-habitat-to-deploy-your-application-to-azure"></a>Utilizar Habitat para implementar a aplicação no Azure
[Habitat](https://www.habitat.sh/) é uma primeira do respetivo tipo projeto open source que oferece uma abordagem de nova à gestão de aplicações. Habitat faz com que a aplicação e a automatização a unidade de implementação. Quando as aplicações são moldadas num "habitat" simples, o ambiente de tempo de execução, se se trata de um contentor, bare-metal ou PaaS, já não é o foco e não a restringir a aplicação. 

Este artigo descreve as vantagens de utilizar Habitat.

## <a name="support-for-the-modern-application"></a>Suporte para a aplicação moderna
Pacotes de habitat incluem tudo o que precisa de uma aplicação para ser executada em todo o ciclo. Componentes do núcleo do habitat são:
- Formato de empacotamento - aplicações num pacote Habitat são atómico, imutável e auditável.
- O supervisor Habitat executa pacotes de aplicações com deteção automática do elemento dos pacotes de relações, estratégia de atualização e as políticas de segurança. O supervisor Habitat configura e gere a aplicação para qualquer ambiente está presente.
- A ecossistema de Habitat também fornece um serviço de compilação que assume um plano de compilação Habitat, cria o pacote de Habitat e publica-a para um depósito.

## <a name="run-any-application-anywhere"></a>Executar qualquer aplicação em qualquer lugar
Com Habitat, as aplicações podem ser executadas inalteradas em qualquer ambiente de tempo de execução. Isto inclui tudo de máquinas virtuais e bare-metal contentores (como Docker), sistemas de gestão de clusters (como Mesosphere ou Kubernetes) e sistemas de PaaS (por exemplo, Pivotal Foundry de nuvem).

## <a name="easily-port-legacy-applications"></a>Facilmente porta aplicações legadas
Quando as aplicações antigas são eliminadas num pacote Habitat, as aplicações são independentes do ambiente para o qual foram originalmente concebidas. Os pacotes rapidamente podem ser movidos para ambientes mais modernos como nuvem ou contentores. Além disso, porque os pacotes de Habitat tem uma interface com acesso de outward padrão, as aplicações legadas tornar-se muito mais fácil de gerir.

## <a name="improve-the-container-experience"></a>Melhorar a experiência de contentor
Habitat reduz a complexidade da gestão de contentores em ambientes de produção. Ao automatizar a configuração da aplicação num contentor, Habitat aborda a desafios os programadores de letra quando mover a aplicações baseadas no contentor de ambientes de desenvolvimento em produção.

## <a name="integrate-into-the-chef-devops-workflow"></a>Integrar o fluxo de trabalho do Chef DevOps
O projeto de Habitat é patrocinado por Chef. Habitat tira partido da experiência de profunda do Chef com a automatização de infraestrutura para colocar as capacidades de automatização inédita para aplicações. Chef irá oferecer suporte comercial para Habitat e certifique-se de uma integração perfeita entre Habitat e entrega Chef para automatizar completamente o ciclo de lançamento de aplicação, do desenvolvimento a implementação.

## <a name="next-steps"></a>Passos Seguintes
* [Criar uma máquina virtual do Windows no Azure utilizando Chef](/azure/virtual-machines/windows/chef-automation)