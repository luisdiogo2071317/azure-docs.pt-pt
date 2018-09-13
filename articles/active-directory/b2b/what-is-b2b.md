---
title: O que é a colaboração do Azure Active Directory B2B? | Microsoft Docs
description: A colaboração B2B do Active Directory do Azure suporta as relações entre empresas ao permitir que os parceiros de negócios acedam seletivamente às suas aplicações empresariais.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/26/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 51a969ae215583a0be8d75ff1de11173e0696a22
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35649259"
---
# <a name="what-is-azure-ad-b2b-collaboration"></a>O que é a colaboração B2B do Azure AD?

Azure recursos de colaboração do Active Directory (Azure AD) empresa-empresa (B2B) permitem que qualquer organização utilizar o Azure AD para trabalhar com segurança e de forma segura com os utilizadores de qualquer outra organização, pequeno ou grande. Essas organizações podem ser com ou sem o Azure AD e até mesmo não precisam de ter um departamento de TI.

As organizações que utilizam o Azure AD podem fornecer acesso a documentos, recursos e aplicações aos seus parceiros, enquanto mantém o controle completo sobre os seus próprios dados empresariais. Os programadores podem utilizar a Azure AD para empresas APIs para escrever aplicativos que reúnem duas organizações em obter mais informações em segurança. Além disso, é fácil para os utilizadores finais navegar.

O vídeo seguinte apresenta uma visão geral útil.
>[!VIDEO https://www.youtube.com/embed/AhwrweCBdsc]

## <a name="key-benefits-of-azure-ad-b2b-collaboration"></a>Principais benefícios da colaboração B2B do Azure AD

### <a name="work-with-any-user-from-any-partner"></a>Trabalhar com qualquer utilizador a partir de qualquer parceiro

- Os parceiros usar suas próprias credenciais
- Nenhum requisito para os parceiros de utilizar o Azure AD
- Nenhuma diretórios externos ou configuração complexa necessária

### <a name="simple-and-secure-collaboration"></a>Colaboração segura e Simple

- Fornecer acesso a qualquer aplicação empresarial ou a dados, ao aplicar sofisticadas, o Azure AD baseada em diretivas de autorização de
- Mais fácil para os utilizadores
- Segurança de nível empresarial para aplicações e dados

### <a name="no-management-overhead"></a>Não existem custos de gestão

- Sem gestão de conta ou palavra-passe externo
- Sem gestão de ciclo de vida de conta de sincronização ou manual
- Nenhuma sobrecarga administrativa externa

## <a name="easily-add-b2b-collaboration-users"></a>Adicione facilmente os utilizadores de colaboração B2B

Como administrador, pode adicionar facilmente utilizadores de colaboração (convidado) B2B para sua organização no [portal do Azure](https://portal.azure.com).

![Adicionar utilizadores convidados](media/what-is-b2b/adding-b2b-users-admin.png)

### <a name="enable-your-collaborators-to-bring-their-own-identity"></a>Ativar seus colaboradores trazer a sua própria identidade

B2B colaboradores podem iniciar sessão com uma identidade de sua preferência. Se o utilizador não tem uma conta Microsoft ou uma conta do Azure AD – é criada uma para os mesmos forma totalmente integrada ao tempo para resgate de oferta.

### <a name="delegate-to-application-and-group-owners"></a>Delegar para a aplicação e os proprietários do grupo

Como uma aplicação ou o proprietário do grupo, pode adicionar utilizadores B2B diretamente para qualquer aplicativo que se preocupa, quer se trate de uma aplicação da Microsoft ou não. Os administradores podem delegar permissão para adicionar utilizadores B2B para não administradores. Os não administradores podem utilizar o [painel de acesso de aplicações do Azure AD](https://myapps.microsoft.com) para adicionar utilizadores de colaboração B2B para aplicações ou grupos.

![Painel de acesso](media/what-is-b2b/access-panel.png)

![Adicionar membro](media/what-is-b2b/add-member.png)

### <a name="authorization-policies-protect-your-corporate-content"></a>Diretivas de autorização de protegem os seus conteúdos empresariais

Políticas de acesso condicional, como a autenticação multifator, podem ser impostas:
- Ao nível do inquilino
- Ao nível da aplicação
- Para utilizadores específicos proteger aplicações e dados empresariais

### <a name="use-apis-and-sample-code-to-easily-build-applications-to-onboard"></a>Utilizar APIs e código de exemplo para criar facilmente aplicativos para carregar

Traga os seus parceiros externos na organização de formas personalizadas para as necessidades da sua organização.

Utilize o [convite de colaboração do B2B APIs](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation) para personalizar a sua integração experiências, incluindo a criação de portais de inscrição self-service. Disponibilizamos o código de exemplo para um portal self-service [no Github](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

![portal de inscrição](media/what-is-b2b/sign-up-portal.png)

Com a colaboração B2B do Azure AD, pode tirar partido da tecnologia do Azure AD para proteger suas relações com parceiros de forma que os utilizadores finais localizar simples e intuitivas.

## <a name="next-steps"></a>Passos Seguintes

- [Como é que os administradores do Azure Active Directory adicionar utilizadores de colaboração B2B?](add-users-administrator.md)
- [Como é que os operadores de informações adicionar utilizadores de colaboração B2B?](add-users-information-worker.md)
- [Resgate de convite de colaboração B2B](redemption-experience.md)
- [Licenciamento de colaboração B2B do AD do Azure](licensing-guidance.md)
- E, como sempre, conecte-se a equipe do produto para quaisquer comentários, discussões e sugestões por meio de nosso [Comunidade tecnológica da Microsoft](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).