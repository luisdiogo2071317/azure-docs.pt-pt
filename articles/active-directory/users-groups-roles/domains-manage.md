---
title: Gerir nomes de domínio personalizados no Azure Active Directory | Documentos da Microsoft
description: Conceitos de gestão e guias de instruções para a gestão de um nome de domínio no Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 11/14/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.openlocfilehash: b503ef4c5cd922d4f7b58940cfc98a83a78ae986
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450083"
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Gerir nomes de domínio personalizados no Azure Active Directory
Um nome de domínio é uma parte importante do identificador para muitos recursos de diretório: ele faz parte de um endereço de e-mail ou nome de utilizador para um utilizador, a parte do endereço de um grupo e podem fazer parte da URI de ID de aplicação para uma aplicação. Um recurso no Azure Active Directory (Azure AD) pode incluir um nome de domínio que já foi verificado como pertencentes ao diretório que contém o recurso. Apenas um administrador global pode efetuar tarefas de gestão de domínio no Azure AD.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>Defina o nome de domínio primário de diretório do Azure AD
Quando é criado o seu diretório, o nome de domínio inicial, como "contoso.onmicrosoft.com", também é o nome de domínio primário. O domínio principal é o nome de domínio predefinido para um novo utilizador ao criar um novo utilizador. Definir um nome de domínio primário simplifica o processo para um administrador criar novos utilizadores no portal. Para alterar o nome de domínio primário:

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) com uma conta que seja um administrador global do diretório.
2. Selecione **Azure Active Directory**.
3. Selecione **nomes de domínio personalizado**.
     
   ![Gestão de utilizadores de abertura](./media/domains-manage/add-custom-domain.png)
4. Selecione o nome do domínio que pretende que o domínio principal.
5. Selecione o **tornar o primário** comando. Certifique-se à sua escolha quando lhe for pedido.
   
   ![Criar um nome de domínio primário](./media/domains-manage/make-primary-domain.png)

Pode alterar o nome de domínio primário para o diretório para qualquer domínio personalizado verificado que não está federado. Alterar o domínio principal para o seu diretório não irá alterar os nomes de utilizador para todos os utilizadores existentes.

## <a name="add-custom-domain-names-to-your-azure-ad-tenant"></a>Adicionar nomes de domínio personalizado ao seu inquilino do Azure AD
Pode adicionar um máximo de 900 nomes de domínio gerido. Se estiver a configurar todos os seus domínios para federação com o Active Directory no local, pode adicionar um máximo de 450 nomes de domínio em cada diretório. Para obter mais informações, consulte [federados e nomes de domínio geridos](https://docs.microsoft.com/azure/active-directory/active-directory-add-domain-concepts#federated-and-managed-domain-names).

## <a name="add-subdomains-of-a-custom-domain"></a>Adicionar subdomínios de um domínio personalizado
Se pretender adicionar um nome de domínio de terceiro nível, como 'europe.contoso.com' para o seu diretório, deve primeiro adicionar e verificar o domínio de segundo nível, como contoso.com. O subdomínio será verificado automaticamente pelo Azure AD. Para ver que o subdomínio que acabou de adicionar foi confirmado, atualize a página no navegador que lista os domínios.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>O que fazer se alterar a entidade de registo DNS para o seu nome de domínio personalizado
Se alterar a entidade de registo DNS para o nome de domínio personalizado, pode continuar a utilizar o seu nome de domínio personalizado com o Azure AD em si sem interrupção e sem as tarefas de configuração adicionais. Se utilizar o seu nome de domínio personalizado com o Office 365, Intune, ou outros serviços que dependem de nomes de domínio personalizados no Azure AD, consulte a documentação para os serviços.

## <a name="delete-a-custom-domain-name"></a>Eliminar um nome de domínio personalizado
Pode eliminar um nome de domínio personalizado do Azure AD, se sua organização já não utiliza esse nome de domínio ou se precisar de utilizar esse nome de domínio com o outro Azure AD.

Para eliminar um nome de domínio personalizado, primeiro tem de se certificar de que não existem recursos no seu diretório contam com o nome de domínio. Não é possível eliminar um nome de domínio do diretório se:

* Qualquer utilizador tem um nome de utilizador, o endereço de e-mail ou o endereço de proxy que inclui o nome de domínio.
* Qualquer grupo tem um endereço de e-mail ou o endereço de proxy que inclui o nome de domínio.
* Qualquer aplicação no seu Azure AD tem um URI de ID, que inclui o nome de domínio do aplicativo.

Tem de alterar ou eliminar todos os tais recursos no diretório do Azure AD antes de poder eliminar o nome de domínio personalizado.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>Utilize o PowerShell ou Graph API para gerir os nomes de domínio
Também é possível concluir a maioria das tarefas de gestão para nomes de domínio no Azure Active Directory através do PowerShell da Microsoft, ou usando programaticamente o Azure AD Graph API.

* [Utilizar o PowerShell para gerir nomes de domínio no Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Com o Graph API para gerir nomes de domínio no Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Passos Seguintes
* [Adicionar nomes de domínio personalizados](../fundamentals/add-custom-domain.md)

