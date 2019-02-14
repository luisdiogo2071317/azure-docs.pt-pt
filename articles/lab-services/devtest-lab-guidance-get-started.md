---
title: Introdução ao Azure DevTest Labs
description: Este artigo fornece o scnearios primário para utilizar o Azure DevTest Labs e dois caminhos gerais para começar a utilizar o serviço na sua organização.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/11/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: 21837265a963829539f12cf7911191a61be43fbc
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2019
ms.locfileid: "56243060"
---
# <a name="get-started-with-using-azure-devtest-labs"></a>Introdução ao Azure DevTest Labs
Depois de decidir explore o DevTest Labs, existem dois caminhos gerais para a frente – prova de conceito vs implementação aumentados. 

R **dimensionado implementação** consiste em semanas/meses de revisão e planejamento com um objetivo de implementação de DevTest Labs para toda a empresa com centenas ou milhares de desenvolvedores. 

R **prova da implementação do conceito** se concentra num esforço concentrarem de uma única equipa para estabelecer o valor organizacional. Embora possa ser tentador pensar numa implantação dimensionada, a abordagem tende a falhar com mais frequência do que a comprovação da opção de conceito. Por conseguinte, recomendamos que comece por algo pequeno, aprenda com a equipe primeiro, repetir a mesma abordagem com duas ou três equipes de adicionais e, em seguida, planejar uma implantação dimensionada com base nos dados recolhidos. Para uma bem-sucedida prova de conceito, recomendamos que escolha uma ou duas equipes e identificar os seus cenários (vs de ambiente de desenvolvimento os ambientes de teste), seus casos de utilização atual de documentos e implementar o DevTest Labs. 

## <a name="scenarios"></a>Cenários
Existem três cenários principais para uma implementação de DevTest Labs: 

- Áreas de trabalho de programador
- Ambientes de teste
- Laboratórios/treinamento/hackathon

## <a name="developer-desktops"></a>Áreas de trabalho de programador
Os desenvolvedores geralmente têm diferentes requisitos para máquinas de desenvolvimento de projetos diferentes. DevTest Labs, os desenvolvedores podem ter acesso às VMs a pedido que estão pré-configuradas para atender às suas situações mais comuns. DevTest Labs fornece as seguintes vantagens:

- As organizações podem fornecer comuns de desenvolvimento e ambientes de teste, garantindo a consistência entre as equipes
- Os desenvolvedores podem aprovisionar suas máquinas de desenvolvimento a pedido
- Os desenvolvedores podem aprovisionar os recursos de uma forma de self-service sem precisarem de permissões de nível de assinatura
- IT ou administradores previamente podem definir a topologia de rede e os desenvolvedores podem diretamente aproveitá-lo de uma forma simples, intuitiva e sem a necessidade de qualquer acesso especial
- Os desenvolvedores podem personalizar facilmente seus computadores de desenvolvimento, conforme necessário
- Os administradores podem controlar os custos, assegurando que:
    - Os desenvolvedores não é possível obter as VMs de mais do que o necessário para o desenvolvimento
    - As VMs são encerradas quando não está em utilização
    - Permitir que apenas um subconjunto de tamanhos de instância VM para cargas de trabalho específicas

## <a name="test-environments"></a>Ambientes de teste
Criar e gerir ambientes de teste de uma empresa podem exigir um esforço significativo. DevTest Labs, ambientes de teste podem ser facilmente criados, atualizados ou duplicados, que permite que as equipes aceder a um ambiente totalmente configurado quando for necessário. Neste cenário, o DevTest Labs fornece as seguintes vantagens:

- Testadores podem testar a versão mais recente de seu aplicativo ao aprovisionar rapidamente ambientes Windows e Linux utilizando modelos e artefactos reutilizáveis.
- Testadores podem aumentar verticalmente o seu teste ao aprovisionar vários agentes de teste de carga
- Os administradores podem ligar-se o laboratório para DevOps do Azure para ativar cenários de DevOps
- Os administradores podem controlar os custos, assegurando que:
    - Os testadores não é possível obter as VMs de mais do que o necessário
    - As VMs são encerradas quando não está em utilização
    - Permitir que apenas um subconjunto de tamanhos de instância VM para cargas de trabalho específicas

## <a name="labs-workshops-trainings-and-hackathons"></a>Laboratórios, workshops, treinamentos e programadores dos encontros  
Um laboratório no Azure DevTest Labs funciona como um contêiner excelente para atividades transitórias como workshops, laboratórios práticos, treinamento ou programadores dos encontros. O serviço permite-lhe criar um laboratório em que pode fornecer modelos personalizados que cada trainee pode utilizar para criar ambientes idênticos e isoladas para treinamento. Pode aplicar políticas para garantir que os ambientes de treinamento estão disponíveis para cada trainee apenas quando precisam e contêm recursos suficientes - como máquinas virtuais - necessários para o treinamento. Por fim, pode partilhar facilmente o laboratório com os formandos, que podem aceder num só clique. Após tem concluíram a classe, é mais fácil eliminar o laboratório e todos os recursos relacionados.


## <a name="next-steps"></a>Passos Seguintes
Consulte o artigo seguinte nesta série: [Aumentar verticalmente a sua implementação do DevTest Labs](devtest-lab-guidance-scale.md)
