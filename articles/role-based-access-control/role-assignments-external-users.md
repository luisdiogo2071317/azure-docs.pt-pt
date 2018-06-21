---
title: Gerir o acesso de utilizadores externos, utilizar o RBAC no Azure | Microsoft Docs
description: Saiba como gerir o acesso de utilizadores externos à organização utilizar o controlo de acesso baseado em funções (RBAC) no Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 03/20/2018
ms.author: rolyon
ms.reviewer: skwan
ms.custom: it-pro
ms.openlocfilehash: 98eb104981051bd5e7440954470960977b38286d
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296219"
---
# <a name="manage-access-for-external-users-using-rbac"></a>Gerir o acesso de utilizadores externos através do RBAC

O controlo de acesso baseado em funções (RBAC) permite a gestão de segurança melhor para organizações grandes e para PMEs trabalhar com colaboradores externos, fornecedores ou freelancers que precisam de acesso a recursos específicos no seu ambiente, mas não necessariamente a toda a infraestrutura ou qualquer âmbitos relacionadas com a faturação. RBAC permite a flexibilidade de proprietário de uma subscrição do Azure gerida pela conta de administrador (função de administrador de serviço a um nível de subscrição) e têm vários utilizadores convidados funcione na mesma subscrição, mas sem quaisquer direitos administrativos para o mesmo.

> [!NOTE]
> Subscrições do Office 365 ou licenças do Azure Active Directory (por exemplo: o acesso ao Azure Active Directory) aprovisionado a partir do System center não se qualificam para utilizar o RBAC do Office 365 Admin.

## <a name="assign-rbac-roles-at-the-subscription-scope"></a>Atribuir funções RBAC no âmbito de subscrição

Existem dois exemplos comuns quando RBAC é utilizada (mas não se limitando a):

* (Que não faz parte do inquilino do Azure Active Directory do utilizador de admin) com utilizadores externos das organizações convidou para gerir determinados recursos ou a subscrição completa
* Trabalhar com os utilizadores dentro da organização (que fazem parte do inquilino do Azure Active Directory do utilizador) mas faz parte equipas diferentes ou grupos que precisam de acesso granular para a subscrição completa ou para determinados grupos de recursos ou âmbitos de recurso no ambiente

## <a name="grant-access-at-a-subscription-level-for-a-user-outside-of-azure-active-directory"></a>Conceder acesso a um nível de subscrição de um utilizador fora do Azure Active Directory

Funções do RBAC podem ser concedidas apenas pelo **proprietários** da subscrição. Por conseguinte, o administrador tem de ter sessão iniciado como um utilizador que tenha esta função previamente atribuído ou tiver criado a subscrição do Azure.

No portal do Azure, depois de iniciar sessão como administrador, selecione "Subscrições" e escolha um pretendido.
![Painel de subscrição no portal do Azure](./media/role-assignments-external-users/0.png) por predefinição, se o utilizador de administrador tiver comprado a subscrição do Azure, o utilizador irá aparecer como **administrador da conta**, isto ser a função de subscrição. Para obter mais informações sobre as funções da subscrição do Azure, consulte [adicionar ou alterar funções de administrador do Azure que gerem a subscrição ou serviços](../billing/billing-add-change-azure-subscription-administrator.md).

Neste exemplo, o utilizador "alflanigan@outlook.com" é o **proprietário** de "Avaliação gratuita" subscrição do AAD inquilino "Predefinido inquilino do Azure". Uma vez que este utilizador é o criador da subscrição do Azure com a Account Microsoft inicial "Outlook" (Microsoft Account = Outlook, etc. Live) o nome de domínio predefinido para todos os outros utilizadores adicionado neste inquilino será **"\@ alflaniganuoutlook.onmicrosoft.com"**. Por predefinição, a sintaxe do novo domínio está formada por colocando, em conjunto, o nome de domínio e nome de utilizador do utilizador que criou o inquilino e adicionar a extensão **". c o m"**.
Além disso, os utilizadores podem iniciar sessão com um nome de domínio personalizado no inquilino depois de adicionar e verificá-lo para o novo inquilino. Para obter mais informações sobre a forma de verificar um nome de domínio personalizado num inquilino do Azure Active Directory, consulte [adicionar um nome de domínio personalizado ao seu diretório](/active-directory/active-directory-add-domain).

Neste exemplo, o diretório de "Predefinição inquilino do Azure" contém apenas os utilizadores com o nome de domínio "\@alflanigan.onmicrosoft.com".

Depois de selecionar a subscrição, o utilizador de administrador tem de clicar em **controlo de acesso (IAM)** e, em seguida, **adicionar uma nova função**.

![funcionalidade de IAM de controlo de acesso no portal do Azure](./media/role-assignments-external-users/1.png)

![Adicionar novo utilizador na funcionalidade IAM de controlo de acesso no portal do Azure](./media/role-assignments-external-users/2.png)

O passo seguinte consiste em selecionar qual será atribuída a função RBAC para a função a ser atribuída e o utilizador. No **função** menu pendente, o utilizador de administrador verá apenas as RBAC funções incorporadas que estão disponíveis no Azure. Para obter uma explicação de cada função e os respetivos âmbitos atribuíveis mais detalhadas, consulte [funções incorporadas](built-in-roles.md).

O utilizador de administrador, em seguida, tem de adicionar o endereço de e-mail do utilizador externo. É o comportamento esperado para o utilizador externo não apareçam no inquilino existente. Depois do utilizador externo tem foi convidado, ele vai estar visível em **subscrições > controlo de acesso (IAM)** com todos os utilizadores atuais que estão atualmente atribuídos uma função RBAC no âmbito de subscrição.

![adicionar permissões a nova função RBAC](./media/role-assignments-external-users/3.png)

![lista de funções do RBAC ao nível da subscrição](./media/role-assignments-external-users/4.png)

O utilizador "chessercarlton@gmail.com" tem foi convidado para ser um **proprietário** para a subscrição de "Avaliação gratuita". Depois de enviar o convite, o utilizador externo irá receber uma confirmação de e-mail com uma ligação de ativação.
![e-mail de convite para a função RBAC](./media/role-assignments-external-users/5.png)

Ser externas à organização, o novo utilizador não tem quaisquer atributos existentes no diretório "Predefinição inquilino do Azure". Estes serão criados depois do utilizador externo forneceu consentimento para serem gravados no diretório que está associado à subscrição foi atribuído uma função para ele.

![mensagem de convite de correio eletrónico para a função RBAC](./media/role-assignments-external-users/6.png)

Mostra o utilizador externo no inquilino do Azure Active Directory a partir agora no como utilizador externo e isto pode ser visualizado no portal do Azure.

![portal do Azure do Active Directory do azure do painel de utilizadores](./media/role-assignments-external-users/7.png)

No **utilizadores** vista, os utilizadores externos podem ser reconhecidos pelo tipo de ícone diferente no portal do Azure.

No entanto, conceder **proprietário** ou **contribuinte** acesso a um utilizador externo no **subscrição** âmbito, não permite o acesso ao diretório do utilizador administrador, a menos que o **Administrador Global** permite. No proprieties utilizador, o **tipo de utilizador**, que tem dois parâmetros comuns, **membro** e **convidado** podem ser identificados. Um membro é um utilizador que está registado no diretório, enquanto um convidado é um utilizador convidado para o diretório a partir de uma origem externa. Para obter mais informações, consulte [como os administradores do Azure Active Directory adicionar utilizadores de colaboração do B2B](../active-directory/active-directory-b2b-admin-add-users.md).

> [!NOTE]
> Certifique-se de que, depois de introduzir as credenciais no portal, o utilizador externo seleciona o diretório correto para iniciar sessão no. O mesmo utilizador pode ter acesso a vários diretórios e selecione um dos mesmos, clicando em nome de utilizador no lado superior direito no portal do Azure e, em seguida, selecione o diretório adequado na lista pendente.

Ao ser convidado no diretório, o utilizador externo pode gerir todos os recursos da subscrição do Azure, mas não é possível aceder ao diretório.

![acesso restringido ao portal do Azure do azure do Active Directory](./media/role-assignments-external-users/9.png)

Azure Active Directory e uma subscrição do Azure não tem uma relação principal-subordinado como outros recursos do Azure (por exemplo: máquinas virtuais, redes virtuais, as aplicações web de armazenamento, etc.) com uma subscrição do Azure. Todos os última opção é criada, geridos e faturada numa subscrição do Azure, enquanto uma subscrição do Azure é utilizada para gerir o acesso a um diretório do Azure. Para obter mais informações, consulte [subscrição como um Azure está relacionada com o Azure AD](/active-directory/active-directory-how-subscriptions-associated-directory).

De todos os as RBAC funções incorporadas, **proprietário** e **contribuinte** oferecem acesso de gestão completo a todos os recursos no ambiente, a diferença é que um contribuinte não é possível criar e eliminar funções RBAC novo. As outras funções incorporadas, como **contribuinte de Máquina Virtual** oferecem acesso completa de gestão apenas para os recursos indicados pelo nome, independentemente do **grupo de recursos** estão a ser criadas numa.

Atribuir a função incorporada do RBAC do **contribuinte de Máquina Virtual** a um nível de subscrição, significa que a função atribuída ao utilizador:

* Pode ver todas as máquinas virtuais, independentemente respetiva data da implementação e os grupos de recursos que fazem parte do
* Tem acesso de gestão completa para as máquinas virtuais na subscrição
* Não é possível ver outros tipos de recursos na subscrição
* Não é possível operar quaisquer alterações de uma perspetiva de faturação

## <a name="assign-a-built-in-rbac-role-to-an-external-user"></a>Atribuir uma função incorporada do RBAC para um utilizador externo

Para um cenário de diferentes neste teste, o utilizador externo "alflanigan@gmail.com" é adicionado como um **contribuinte de Máquina Virtual**.

![função incorporada de contribuinte de máquina virtual](./media/role-assignments-external-users/11.png)

É o comportamento normal para este utilizador externo com esta função incorporada para ver e gerir apenas máquinas virtuais e os respetivos adjacentes Gestor de recursos apenas recursos necessário durante a implementação. Por predefinição, estas funções limitadas oferecem acesso apenas aos respetivos recursos correspondente criado no portal do Azure.

![Descrição geral da função de contribuinte de máquina virtual no portal do Azure](./media/role-assignments-external-users/12.png)

## <a name="grant-access-at-a-subscription-level-for-a-user-in-the-same-directory"></a>Conceder acesso a um nível de subscrição de um utilizador no mesmo diretório

O fluxo de processo é idêntico ao adicionar um utilizador externo, da perspetiva de administração concedendo a função RBAC, bem como o utilizador ser concedido acesso à função. A diferença aqui é que o utilizador convidado não receberão qualquer convites para correio eletrónico como todos os âmbitos de recursos dentro da subscrição estará disponíveis no dashboard, depois de iniciar sessão.

## <a name="assign-rbac-roles-at-the-resource-group-scope"></a>Atribuir funções RBAC no âmbito do grupo de recursos

Atribuir uma função do RBAC um **grupo de recursos** âmbito tem um processo idêntico para atribuir a função ao nível da subscrição, para ambos os tipos de utilizadores - internos ou externos (parte do mesmo diretório). Os utilizadores que estão atribuídos a função RBAC é ver no seu ambiente, apenas o grupo de recursos foram atribuídos acesso a partir de **grupos de recursos** ícone no portal do Azure.

## <a name="assign-rbac-roles-at-the-resource-scope"></a>Atribuir funções RBAC no âmbito do recurso

Atribuir uma função RBAC num âmbito de recursos no Azure tem um processo idêntico para atribuir a função ao nível da subscrição ou ao nível do grupo de recursos, seguindo o mesmo fluxo de trabalho para ambos os cenários. Novamente, os utilizadores que estão atribuídos a função RBAC podem ver apenas os itens que tenham sido atribuídos acesso a, tem de **todos os recursos** separador ou diretamente no seu dashboard.

Um aspeto importante para RBAC tanto no âmbito do grupo de recursos ou âmbito de recursos é para os utilizadores para se certificar-se de que inicie sessão no diretório correto.

![início de sessão de diretório no portal do Azure](./media/role-assignments-external-users/13.png)

## <a name="assign-rbac-roles-for-an-azure-active-directory-group"></a>Atribuir funções RBAC para um grupo do Azure Active Directory

Todos os cenários utilizando o RBAC nos três âmbitos diferentes no Azure oferecem o privilégio de gerir, implementar e administrar vários recursos como um utilizador atribuído sem a necessidade de gerir uma subscrição pessoal. Independentemente de já está atribuída a função RBAC para uma subscrição, o grupo de recursos ou o âmbito de recursos, todos os recursos criados adicionais pelos utilizadores atribuídos são faturados sob a uma subscrição do Azure onde os utilizadores têm acesso a. Desta forma, os utilizadores que têm permissões de administrador para essa subscrição do Azure completa de faturação não tem uma descrição geral completa no consumo, independentemente que está a gerir os recursos.

Para organizações de maior, as funções do RBAC podem ser aplicadas da mesma forma para grupos do Azure Active Directory considerar que o utilizador de administrador pretende conceder o acesso granular para equipas ou departamentos de todos, não individualmente para cada utilizador, considerar, assim como extremamente hora e a gestão eficiente opção perspetiva. Para ilustrar neste exemplo, o **contribuinte** função foi adicionada a um dos grupos no inquilino ao nível da subscrição.

![adicionar a função RBAC para grupos do AAD](./media/role-assignments-external-users/14.png)

Estes grupos são grupos de segurança, que são aprovisionados e geridos apenas dentro do Azure Active Directory.

