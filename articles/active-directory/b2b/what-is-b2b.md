---
title: O que é a colaboração B2B do Azure Active Directory do? | Microsoft Docs
description: Colaboração do Azure Active Directory B2B suporta as relações entre empresas, permitindo a parceiros de negócios seletivamente aceder a aplicações empresariais.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/26/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 2bc405e6356113e0423f833868c86890c0c3d5d2
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="what-is-azure-ad-b2b-collaboration"></a>O que é a colaboração B2B do Azure AD?

Azure Active Directory (Azure AD) empresa-empresa (B2B) colaboração as capacidades de ativar qualquer organização utilizar o Azure AD a trabalhar com segurança e de forma segura com os utilizadores de qualquer outra organização grande ou pequeno. As organizações podem ser com ou sem do Azure AD e não precisam sequer de ter um departamento de TI.

As organizações a utilizar o Azure AD podem fornecer acesso a documentos, recursos e aplicações para os respetivos parceiros, mantendo o controlo total sobre os seus próprios dados empresariais. Os programadores podem utilizar a Azure AD empresa-empresa APIs para escrever as aplicações que reunir as duas organizações no mais segura. Além disso, é mais fácil para os utilizadores finais navegar.

O vídeo seguinte fornece uma descrição geral de útil.
>[!VIDEO https://www.youtube.com/embed/AhwrweCBdsc]

## <a name="key-benefits-of-azure-ad-b2b-collaboration"></a>Principais benefícios da colaboração B2B do Azure AD

### <a name="work-with-any-user-from-any-partner"></a>Trabalhar com qualquer utilizador a partir de qualquer parceiro

- Parceiros de utilizam as suas próprias credenciais
- Sem requisito de parceiros ao utilizar o Azure AD
- Não existem diretórios externos ou complexa configuração necessária

### <a name="simple-and-secure-collaboration"></a>Colaboração segura e Simple

- Fornecer acesso a quaisquer aplicações empresariais ou dados, ao aplicar sofisticadas, Azure AD com tecnologia políticas de autorização de
- Mais fácil para os utilizadores
- Segurança de nível empresarial para aplicações e dados

### <a name="no-management-overhead"></a>Não existem custos de gestão

- Nenhuma gestão de conta ou palavra-passe externo
- Nenhuma gestão de ciclo de vida de conta de sincronização ou manual
- Nenhuma sobrecarga administrativa externa

## <a name="easily-add-b2b-collaboration-users"></a>Facilmente adicionar utilizadores de colaboração do B2B

Como administrador, pode facilmente adicionar utilizadores de colaboração (convidado) de B2B a sua organização no [portal do Azure](https://portal.azure.com).

![Adicionar utilizadores convidados](media/what-is-b2b/adding-b2b-users-admin.png)

### <a name="enable-your-collaborators-to-bring-their-own-identity"></a>Ativar os funcionários colocar as seus próprios identidade

B2B colaboradores podem iniciar sessão com uma identidade da sua eleição. Se o utilizador não tiver uma conta Microsoft ou uma conta do Azure AD – é criada uma para os mesmos totalmente integrada no momento de resgate de oferta.

### <a name="delegate-to-application-and-group-owners"></a>Delegar a aplicação e proprietários de grupo

Como uma aplicação ou o proprietário do grupo, pode adicionar utilizadores de B2B diretamente para qualquer aplicação que mais lhe interessam, se se trata de uma aplicação da Microsoft ou não. Os administradores podem delegar permissões para adicionar utilizadores de B2B para não administradores. Não-os administradores podem utilizar o [painel de acesso de aplicação do Azure AD](https://myapps.microsoft.com) para adicionar utilizadores de colaboração do B2B a aplicações ou grupos.

![Painel de acesso](media/what-is-b2b/access-panel.png)

![Adicionar membro](media/what-is-b2b/add-member.png)

### <a name="authorization-policies-protect-your-corporate-content"></a>Políticas de autorização de protegem os seus conteúdos de empresa

Políticas de acesso condicional, tais como a autenticação multifator, podem ser impostas:
- Ao nível do inquilino
- Ao nível da aplicação
- Para utilizadores específicos proteger aplicações e dados empresariais

### <a name="use-apis-and-sample-code-to-easily-build-applications-to-onboard"></a>Utilizar as APIs e código para facilmente criar aplicações para carregar de exemplo

Traga os seus parceiros externos no board formas personalizada para necessidades da sua organização.

Utilize o [convite de colaboração B2B APIs](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation) personalizar a sua integração experiências, incluindo a criação de portais de inscrição self-service. Podemos fornecer o código de exemplo para um portal self-service [no Github](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

![portal de inscrição](media/what-is-b2b/sign-up-portal.png)

Com a colaboração B2B do Azure AD, pode obter o poder do Azure AD para proteger as relações com parceiros de forma a que os utilizadores finais encontrar fácil e intuitivo.

## <a name="next-steps"></a>Passos Seguintes

- [Como é que os administradores de Azure Active Directory adicionar utilizadores de colaboração do B2B?](add-users-administrator.md)
- [Como é que os infotrabalhadores adicionar utilizadores de colaboração do B2B?](add-users-information-worker.md)
- [Resgate de convite de colaboração B2B](redemption-experience.md)
- [Licenciamento e colaboração do Azure AD B2B](licensing-guidance.md)
- E, como sempre, estabelecer ligação com a equipa de produto para quaisquer comentários, debates e sugestões através do nosso [Microsoft técnico Comunidade](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).