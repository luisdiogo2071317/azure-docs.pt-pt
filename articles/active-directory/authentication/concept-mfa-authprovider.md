---
title: Quando e como utilizar um fornecedor do multi-factor Auth do Azure?
description: Quando deve utilizar um fornecedor de autenticação com a MFA do Azure?
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 4a6ce07bfe641d9efdbe0eac841bb4f27f468b34
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/19/2018
ms.locfileid: "39161469"
---
# <a name="when-to-use-an-azure-multi-factor-authentication-provider"></a>Quando utilizar um fornecedor de autenticação do multi-factor do Azure

A verificação de dois passos está disponível por predefinição para os administradores globais que tenham utilizadores do Azure Active Directory e do Office 365. No entanto, se pretender tirar partido das [funcionalidades avançadas](howto-mfa-mfasettings.md), deverá comprar a versão completa do Multi-Factor Authentication (MFA) do Azure.

É utilizado um Fornecedor do Multi-Factor Auth do Azure para tirar partido das funcionalidades fornecidas pela versão completa do MFA do Azure. É para os utilizadores quem **não têm licenças do MFA do Azure, Azure AD Premium ou os pacotes que incluem o Azure AD Premium ou o MFA do Azure**. Azure MFA e o Azure AD Premium incluem a versão completa do MFA do Azure por predefinição.

Se tiver licenças que abrangem todos os utilizadores na sua organização, em seguida, não terá um fornecedor do multi-factor Auth do Azure. Crie um fornecedor de autenticação do multi-factor do Azure apenas se também tem de fornecer a verificação de dois passos para alguns usuários que não têm licenças.

## <a name="caveats-related-to-the-azure-mfa-sdk"></a>Avisos relacionados com o SDK de MFA do Azure

Para transferir o SDK, é preciso um fornecedor do Multi-Factor Auth do Azure. Tenha em atenção que o SDK foi preterido e já não é suportado para os novos clientes e só irá continuar a funcionar até 14 de Novembro de 2018. Após esse tempo, as chamadas para o SDK irão falhar.

Para transferir o SDK, crie um fornecedor do multi-factor Auth do Azure, mesmo que tenha o MFA do Azure, AAD Premium ou outras licenças agrupadas. Se criar um fornecedor do Multi-Factor Auth do Azure para esta finalidade e já tiver licenças, verifique se cria o Fornecedor com o modelo **Por Utilizador Ativado**. Em seguida, associe o fornecedor para o diretório que contém o MFA do Azure, Azure AD Premium ou outras licenças agrupadas. Esta configuração garante que só lhe será cobrado qualquer valor se tiver mais utilizadores exclusivos a efetuarem a verificação em dois passos do que o número de licenças.

## <a name="what-is-an-mfa-provider"></a>O que é um Fornecedor de MFA?

Se não tiver licenças para o Azure multi-factor Authentication, pode criar um fornecedor de autenticação que exija uma verificação para os seus utilizadores.

Existem dois tipos de fornecedores de autenticação, e a diferença em torno de como a sua subscrição do Azure é cobrada. A opção por autenticação calcula o número de autenticações executadas num mês relativamente ao seu inquilino. Esta é a melhor opção no caso de existirem vários utilizadores que são ocasionalmente autenticados. A opção por utilizador calcula o número de indivíduos no seu inquilino que executam a verificação de dois passos num mês. Esta é a melhor opção caso existam alguns utilizadores com licenças, mas tem de alargar a MFA a mais utilizadores para além dos limites da sua licença.

## <a name="create-an-mfa-provider"></a>Criar um Fornecedor de MFA

Utilize os passos seguintes para criar um Fornecedor do Multi-Factor Authentication no portal do Azure:

1. Inicie sessão no [Portal do Azure](https://portal.azure.com) como administrador global.
2. Selecione **Azure Active Directory** > **Servidor MFA** > **Fornecedores**.

   ![Fornecedores][Providers]

3. Selecione **Adicionar**.
4. Preencha os campos seguintes e, em seguida, selecione **Adicionar**:
   - **Nome** – o nome do Fornecedor.
   - **Modelo de Utilização** – Escolha uma das duas opções:
      * Por Autenticação – Modelo de compra que cobra por autenticação. Normalmente utilizado para cenários que utilizam o Multi-Factor Authentication do Azure numa aplicação direcionada para o consumidor.
      * Por Utilizador Ativado – Modelo de compra que cobra por utilizador ativado. Normalmente utilizado para acesso dos empregados a aplicações, como o Office 365. Escolha esta opção se tiver alguns utilizadores licenciados do MFA do Azure.
   - **Subscrição** – a subscrição do Azure que é cobrada para a atividade de verificação de dois passos através do Fornecedor.
   - **Diretório** – o inquilino do Azure Active Directory ao qual o Fornecedor está associado.
      * Não é necessário um diretório do Azure AD para criar um Fornecedor. Deixe esta caixa em branco se estiver apenas a planear transferir o Servidor Multi-Factor Authentication do Azure.
      * O Fornecedor tem de estar associado a um diretório do Azure AD para tirar partido das funcionalidades avançadas.
      * Só pode estar associado um Fornecedor a qualquer diretório do Azure AD.

## <a name="manage-your-mfa-provider"></a>Gerir o Fornecedor de MFA

Não pode alterar o modelo de utilização (por utilizador ativado ou por autenticação) após a criação de um fornecedor do MFA. No entanto, pode eliminar o fornecedor do MFA e, em seguida, criar um com um modelo de utilização diferente.

Se o Fornecedor do Multi-Factor Auth atual estiver associado a um diretório do Azure AD (também conhecido como inquilino do Azure AD), pode eliminar o fornecedor do MFA com segurança e criar um que esteja associado ao mesmo inquilino do Azure AD. Em alternativa, se tiver adquirido suficiente MFA, Azure AD Premium ou os pacotes que incluem licenças do Azure AD Premium ou o MFA do Azure para abranger todos os utilizadores que estão ativados para a MFA, é possível eliminar o fornecedor do MFA completamente.

Se o fornecedor do MFA não estiver associado a um inquilino do Azure AD ou associar o novo fornecedor do MFA a um inquilino diferente do Azure AD, as definições do utilizador e as opções de configuração não são transferidas. Além disso, os Servidores MFA do Azure existentes têm de ser reativados com as credenciais de ativação geradas através do novo Fornecedor do MFA. Reativar os servidores MFA para associá-los para o novo fornecedor do MFA não afeta chamadas telefónicas e de autenticação de mensagem de texto, mas as notificações da aplicação móvel deixam de funcionar para todos os utilizadores até que reativem a aplicação móvel.

## <a name="next-steps"></a>Passos Seguintes

[Configurar as Definições do Multi-Factor Authentication](howto-mfa-mfasettings.md)

[Providers]: ./media/concept-mfa-authprovider/add-providers.png "Adicionar Fornecedores MFA"
