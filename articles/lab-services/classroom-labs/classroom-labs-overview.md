---
title: Sobre o Azure Lab Services | Microsoft Docs
description: Saiba como o Lab Services pode tornar mais fácil criar, gerir e proteger laboratórios com máquinas virtuais que podem ser utilizadas pelos programadores, técnicos de teste, educadores, estudantes, entre outros.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 05/21/2018
ms.author: spelluru
ms.openlocfilehash: e9c3cae7c7129cc489ddd38b5b2de18dd6f52e58
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660594"
---
# <a name="introduction-to-classroom-labs"></a>Introdução aos laboratórios de sala de aula
O Azure Lab Services permite-lhe configurar rapidamente um ambiente de laboratório de sala de aula na cloud. O educador cria um laboratório de sala de aula, aprovisiona máquinas virtuais do Linux ou Windows, instala os laboratórios de ferramentas e software necessários na sala de aula e disponibiliza-os aos utilizadores. Os alunos na sala de aula ligam-se a máquinas virtuais (VMs) no laboratório e utilizam-nas para os projetos, tarefas ou para fazer exercícios da sala de aula. 

Os laboratórios de sala de aula são laboratórios geridos, com gestão do Azure. O próprio serviço processa toda a gestão da infraestrutura de um laboratório gerido, desde gerar as máquinas virtuais (VMs) até processar os erros e dimensionar a infraestrutura. Especifica o tipo de infraestrutura que precisa e instala quaisquer ferramentas ou software necessário para a sala de aula. Os laboratórios geridos estão atualmente em pré-visualização.  

## <a name="scenarios"></a>Cenários
Este é o cenário principal que suporta os laboratórios de sala de aula do Azure Lab Services: 

### <a name="set-up-a-resizable-computer-lab-in-the-cloud-for-your-classroom"></a>Configurar um laboratório de computadores redimensionável na cloud para a sala de aula  

- Crie um laboratório de sala de aula gerido. Basta indicar ao serviço exatamente aquilo de que precisa e este cria e gere a infraestrutura do laboratório automaticamente, para que se possa focar em dar a aula e não nos detalhes técnicos de um laboratório. 
- Forneça aos estudantes um laboratório com máquinas virtuais configuradas com o que é necessário para uma aula. Dê a cada estudante um número limitado de horas para utilizar as VMs para trabalhos da aula.  
- Mova o laboratório de computadores físico da escola para a cloud. Dimensione automaticamente o número de VMs apenas para o limite máximo de custo e utilização definido no laboratório. 
- Elimine o laboratório com um único clique quando terminar. 

## <a name="user-profiles"></a>Perfis de utilizador
Este artigo descreve perfis de utilizador diferentes no Azure Lab Services. 

### <a name="lab-account-owner"></a>Proprietário da conta de laboratório
Normalmente, o administrador de TI dos recursos de cloud da organização que é o proprietário da subscrição do Azure age como o proprietário da conta de laboratório e realiza as seguintes tarefas:   

- Configura uma conta de laboratório para a organização.
- Gere e configura políticas em todos os laboratórios.
- Dá permissões a pessoas na organização para criarem um laboratório na conta de laboratório.

### <a name="educator"></a>Educador
Normalmente, os utilizadores como um professor ou um formador online criam laboratórios de sala de aula numa conta de laboratório. Um educador realiza as seguintes tarefas: 

- Cria um laboratório de sala de aula.
- Cria máquinas virtuais no laboratório. 
- Instala o software adequado em máquinas virtuais.
- Especifica quem pode aceder ao laboratório.
- Fornece a ligação de registo para o laboratório aos estudantes.

### <a name="student"></a>Estudante
O aluno realiza as seguintes tarefas:

- Utiliza a ligação de registo que o utilizador do laboratório recebe do criador do laboratório para se registar no laboratório. 
- Liga-se a uma máquina virtual no laboratório e utiliza-a para tarefas, projetos ou para fazer trabalho da aula. 

## <a name="next-steps"></a>Passos seguintes
Introdução à configuração de uma conta de laboratório necessária para criar um laboratório de sala de aula com o Azure Lab Services:

- [Configurar uma conta de laboratório](tutorial-setup-lab-account.md)
