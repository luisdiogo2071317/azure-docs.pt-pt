---
title: O Azure multi-factor Authentication - como funciona
description: A Multi-Factor Authentication do Azure ajuda a salvaguardar o acesso a dados e a aplicações, satisfazendo, em simultâneo, a necessidade dos utilizadores de terem um processo de início de sessão simples.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: ad9d517be930f68dcddba87fc59eab8b830a2b1c
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/19/2018
ms.locfileid: "39159156"
---
# <a name="how-it-works-azure-multi-factor-authentication"></a>Como funciona: Azure multi-factor Authentication

A segurança de verificação de dois passos está na sua abordagem em camadas. Comprometer a vários fatores de autenticação apresenta um desafio significativo dos invasores. Mesmo que um atacante conseguir saber a senha do usuário, é inútil sem também ter posse do método de autenticação adicional. Ele funciona exigindo dois ou mais dos seguintes métodos de autenticação:

* Algo que saiba (normalmente uma palavra-passe)
* Algo que tenha (um dispositivo fidedigno que não seja duplicado facilmente, como um telemóvel)
* Algo que seja (biometria)

<center>![Imagem de métodos de autenticação conceitual](./media/concept-mfa-howitworks/methods.png)</center>

O Azure multi-factor Authentication (MFA) ajuda a salvaguardar o acesso a dados e aplicativos, mantendo a simplicidade para os utilizadores. Ele fornece segurança adicional, exigindo que uma segunda forma de autenticação e proporciona uma autenticação segura através de um conjunto de fácil de usar [métodos de autenticação](concept-authentication-methods.md).

## <a name="how-to-get-multi-factor-authentication"></a>Como obter a multi-factor Authentication?

Autenticação Multifator é fornecido como parte das ofertas seguintes:

* **Licenças do Azure Active Directory Premium** -completo do uso de serviço do Azure multi-factor Authentication (Cloud) ou Azure multi-factor Authentication Server (no local).
   * **Serviço de MFA do Azure (nuvem)** - **esta opção é o caminho recomendado para novas implementações**. Azure MFA na nuvem não exige nenhuma infraestrutura no local e pode ser utilizado com os seus utilizadores federados ou apenas na cloud.
   * **O servidor MFA do Azure** -se a sua organização quer gerenciar os elementos da infraestrutura associada e implementou o AD FS no seu ambiente lhes é desta forma pode ser uma opção.
* **Multi-factor Authentication para Office 365** -um subconjunto de capacidades multi-factor Authentication do Azure estão disponíveis como parte da sua subscrição. Para obter mais informações sobre o MFA para o Office 365, consulte o artigo [planear a autenticação multifator para implementações do Office 365](https://support.office.com/article/plan-for-multi-factor-authentication-for-office-365-deployments-043807b2-21db-4d5c-b430-c8a6dee0e6ba).
* **Administradores do Azure Active Directory Global** -um subconjunto de capacidades multi-factor Authentication do Azure estão disponíveis como um meio para proteger contas de administrador global.

### <a name="auth-provider-or-mfa-license"></a>Licença de fornecedor de autenticação ou de MFA

Se tiver o Azure AD Premium ou um pacote de licença que inclui o Azure AD Premium, já tem MFA do Azure. Sua organização não precise de nada adicional para expandir a capacidade de verificação de dois passos para todos os utilizadores. Só tem de atribuir uma licença a um utilizador e, em seguida, pode ativar o MFA.

Se não tiver licenças que incluem o MFA do Azure ou não tiverem suficiente licenças para abranger todos os seus utilizadores, em seguida, pode criar uma [fornecedor de autenticação de MFA](concept-mfa-authprovider.md) para expandir as capacidades completas do MFA para os utilizadores que necessitam deles. 

> [!IMPORTANT]
> Se não tiver licenças suficientes para todos os seus utilizadores, pode criar um fornecedor de autenticação do multi-factor de por utilizador para cobrir o resto da sua organização. Não crie um fornecedor do multi-factor Auth por autenticação. Se o fizer, poderia acabar por pagar para pedidos de verificação de utilizadores que já tem licenças.

## <a name="supportability"></a>Suportabilidade

Uma vez que a maioria dos usuários estão acostumados a utilizar apenas as palavras-passe para autenticar, é importante que sua organização se comunica a todos os utilizadores em relação a esse processo. Reconhecimento pode reduzir a probabilidade de que os utilizadores, ligue o suporte técnico para problemas secundários relacionados com a MFA. No entanto, existem alguns cenários em que é necessário desativar temporariamente a MFA. Utilize as diretrizes seguintes para compreender como lidar com esses cenários:

* Treine sua equipe de suporte para manipular os cenários em que o utilizador não pode iniciar sessão uma vez que não têm acesso aos seus métodos de autenticação ou que não estão a funcionar corretamente.
   * Utilizar políticas de acesso condicional para o serviço de MFA do Azure, a equipe de suporte pode adicionar um utilizador a um grupo que esteja excluído de uma política que exija a MFA.
   * A equipe de suporte pode ativar uma omissão de uso individual temporária para os utilizadores do servidor MFA do Azure permitir que um utilizador sejam autenticados sem verificação de dois passos. A omissão é temporária e expira após um número de segundos especificado.
   * Utilizar políticas de acesso condicional para o serviço de MFA do Azure sua equipe pode adicionar um utilizador a um grupo que esteja excluído de uma política que exija a MFA.
* Considere a utilização de IPs fidedignos ou localizações com nome como uma forma para minimizar os pedidos de verificação de dois passos. Com esta funcionalidade, os administradores de um inquilino gerido ou Federado podem ignorar a verificação de dois passos para os utilizadores que a sessão iniciada a partir de uma localização de rede fidedigna, como na intranet da organização.
* Implementar [do Azure AD Identity Protection](../active-directory-identityprotection.md) e acionar a verificação de dois passos com base em eventos de risco.

## <a name="next-steps"></a>Passos Seguintes

- Encontrar detalhes sobre [licenciamento os seus utilizadores](concept-mfa-licensing.md)

- Obter detalhes [a versão a implementar](concept-mfa-whichversion.md)

- Encontre respostas para [perguntas mais frequentes](multi-factor-authentication-faq.md)