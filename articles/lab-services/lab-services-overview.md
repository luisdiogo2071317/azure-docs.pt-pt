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
ms.date: 04/19/2018
ms.author: spelluru
ms.openlocfilehash: 8f8de53f03cc1378e8aaafa9a2f486eadc23fe14
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34637858"
---
# <a name="an-introduction-to-azure-lab-services"></a>Introdução ao Azure Lab Services
O Azure Lab Services permite-lhe configurar rapidamente um ambiente para a sua equipa (por exemplo: ambiente de desenvolvimento, ambiente de teste, ambiente de laboratório de sala de aula) na cloud. O proprietário do laboratório cria um laboratório, aprovisiona máquinas virtuais do Linux ou Windows, instala as ferramentas e o software necessário e disponibiliza-os aos utilizadores do laboratório. Os utilizadores do laboratório ligam-se a máquinas virtuais (VMs) no laboratório e utilizam-nas para o trabalho diário, projetos de curto prazo ou para fazer exercícios da sala de aula. Quando os utilizadores começam a utilizar recursos no laboratório, o administrador do laboratório pode analisar os custos e utilização em múltiplos laboratórios e definir políticas abrangentes para otimizar os custos da equipa ou organização.

> [!IMPORTANT]
> O **Azure DevTest Labs** está a ser expandido com novos tipos de laboratórios (Azure Lab Services)! 
> 
> O Azure Lab Services permite-lhe criar laboratórios geridos, tal como laboratórios de sala de aula. O próprio serviço processa toda a gestão da infraestrutura de um laboratório gerido, desde gerar as VMs até processar os erros e dimensionar a infraestrutura. Os laboratórios geridos estão atualmente em pré-visualização. Assim que a pré-visualização terminar, os novos tipos de laboratório e o DevTest Labs existente serão apresentados no Azure Lab Services onde todos os tipos de laboratórios continuam a evoluir. 

## <a name="key-capabilities"></a>Principais capacidades
O Azure Lab Services suporta as seguintes funcionalidades-chave: 

- **Configuração rápida e flexível de um laboratório**. Ao utilizar o Azure Lab Services, os proprietários do laboratório podem configurar rapidamente um laboratório para as suas necessidades. O serviço oferece a opção para tratar de todo o trabalho da infraestrutura do Azure para laboratórios geridos ou para permitir que os proprietários do laboratório façam a gestão e personalizem a infraestrutura na subscrição do proprietário do laboratório. O serviço fornece dimensionamento incorporado e resiliência da infraestrutura para laboratórios que o serviço gere automaticamente. 
- **Experiência simplificada para utilizadores do laboratório**. Num laboratório gerido, como um laboratório de sala de aula, os utilizadores de laboratório podem registar-se num laboratório com um código de registo e aceder ao laboratório em qualquer altura para utilizar os recursos do laboratório. Num laboratório criado no DevTest Labs, o proprietário do laboratório pode dar permissões para os utilizadores do laboratório criarem e acederem a máquinas virtuais, gerirem e reutilizarem discos de dados e configurarem segredos reutilizáveis.  
- **Análise e otimização de custos**. O proprietário do laboratório pode definir agendas de laboratório para encerrar e iniciar automaticamente as máquinas virtuais. O proprietário do laboratório pode definir uma agenda para especificar os blocos de tempo em que as máquinas virtuais do laboratório estão acessíveis para os utilizadores, definir políticas de utilização por utilizador ou laboratório para otimizar os custos e analisar as tendências de atividade e utilização num laboratório. Para laboratórios geridos, tal como laboratórios de sala de aula, estão disponíveis subconjuntos mais pequenos de opções de análise e otimização dos custos. 
- **Segurança incorporada**. O proprietário do laboratório pode configurar uma rede virtual privada e uma subrede para um laboratório e ativar um endereço IP público partilhado. Os utilizadores do laboratório podem aceder aos recursos de forma segura com a rede virtual configurada com o ExpressRoute ou rede de VPNs. (apenas disponível no DevTest Labs)
- **Integração com os fluxos de trabalho e ferramentas**. O Azure Lab Services permite-lhe integrar os laboratórios nos sistemas de gestão e site da organização. Pode aprovisionar automaticamente ambientes a partir das ferramentas de integração contínua/implementação contínua (CI/CD). (apenas disponível no DevTest Labs)

## <a name="scenarios"></a>Cenários
Eis alguns dos cenários que o Azure Lab Services suporta: 

### <a name="set-up-a-resizable-computer-lab-in-the-cloud-for-your-classroom"></a>Configurar um laboratório de computadores redimensionável na cloud para a sala de aula  

- Crie um laboratório de sala de aula gerido. Basta indicar ao serviço exatamente aquilo de que precisa e este irá criar e gerir a infraestrutura do laboratório automaticamente, para que se possa focar em dar a aula e não nos detalhes técnicos de um laboratório. 
- Forneça aos estudantes um laboratório com máquinas virtuais configuradas com o que é necessário para uma aula. Dê a cada estudante um número limitado de horas para utilizar as VMs para trabalhos da aula.  
- Mova o laboratório de computadores físico da escola para a cloud. Dimensione automaticamente o número de VMs apenas para o limite máximo de custo e utilização definido no laboratório. 
- Elimine o laboratório com um único clique quando terminar. 

### <a name="use-devtest-labs-for-development-environments"></a>Utilizar o DevTest Labs para ambientes de desenvolvimento 
O Azure DevTest Labs pode ser utilizado para implementar vários cenários-chave, mas apenas os cenários principais envolvem a utilização do DevTest Labs para alojar computadores de desenvolvimento para programadores. Neste cenário, o DevTest Labs fornece estas vantagens: 

- Permita que os programadores aprovisionem rapidamente as computadores de desenvolvimento a pedido.
- Aprovisione ambientes do Windows e Linux com modelos e artefactos reutilizáveis.
- Os programadores podem personalizar facilmente os seus computadores de desenvolvimento quando for necessário.
- Os administradores podem controlar os custos ao garantir que os programadores não podem obter mais VMs do que as que necessitam para desenvolvimento e as VMs são encerradas quando não estão a ser utilizadas. 

Para obter mais informações, veja [Utilizar o DevTest Labs para desenvolvimento](devtest-lab-developer-lab.md). 

### <a name="use-devtest-labs-for-test-environments"></a>Utilizar o DevTest Labs para ambientes de teste
Pode utilizar o Azure DevTest Labs para implementar vários cenários-chave, mas apenas os cenários principais envolvem a utilização do DevTest Labs para alojar computadores para técnicos de teste. Neste cenário, o DevTest Labs fornece estas vantagens:

- Os técnicos de teste podem testar a última versão da sua aplicação ao aprovisionar rapidamente ambientes do Windows e do Linux através de modelos e artefactos reutilizáveis.
- Os técnicos de teste podem dimensionar o teste de carga ao aprovisionar múltiplos agentes de teste.
- Os administradores podem controlar os custos ao garantir que os técnicos de teste não podem obter mais VMs do que as que necessitam para testes e as VMs são encerradas quando não estão a ser utilizadas.

Para obter mais informações, veja [Utilizar o DevTest Labs para testes](devtest-lab-test-env.md).

## <a name="user-profiles"></a>Perfis de utilizador
Este artigo descreve perfis de utilizador diferentes no Azure Lab Services. 

### <a name="lab-account-owner"></a>Proprietário da conta de laboratório
Normalmente, o administrador de TI dos recursos de cloud da organização que é o proprietário da subscrição do Azure age como o proprietário da conta de laboratório e realiza as seguintes tarefas:   

- Configura uma conta de laboratório para a organização.
- Gere e configura políticas em todos os laboratórios.
- Dá permissões a pessoas na organização para criarem um laboratório na conta de laboratório.

### <a name="lab-creator"></a>Criador do laboratório 
Normalmente, são utilizadores tais como um gestor/líder de desenvolvimento, um professor, um anfitrião de hackathon ou um formador online que criam laboratórios na conta de laboratório. O criador do laboratório realiza as seguintes tarefas: 

- Cria um laboratório.
- Cria máquinas virtuais no laboratório. 
- Instala o software adequado em máquinas virtuais.
- Especifica quem pode aceder ao laboratório.
- Fornece aos utilizadores a ligação para o laboratório.

### <a name="lab-user"></a>Utilizador do laboratório
O utilizador do laboratório realiza as seguintes tarefas:

- Utiliza a ligação de registo que o utilizador do laboratório recebe do criador do laboratório para se registar no laboratório. 
- Liga-se a uma máquina virtual no laboratório e utiliza-a para desenvolvimento, testes ou para fazer trabalho da aula. 

## <a name="next-steps"></a>Passos seguintes
Introdução à configuração de um laboratório com o Azure Lab Services:

- [Configurar um laboratório de sala de aula](classroom-labs/tutorial-setup-classroom-lab.md)
- [Configurar um laboratório](tutorial-create-custom-lab.md)
