---
title: Quando e como utilizar um fornecedor do multi-factor Auth do Azure?
description: Quando deve utilizar um fornecedor de autenticação com a MFA do Azure?
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 8e77a33667bd6794f667348958e0edb9c6a8fb0d
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44094982"
---
# <a name="when-to-use-an-azure-multi-factor-authentication-provider"></a>Quando utilizar um fornecedor de autenticação do multi-factor do Azure

A verificação de dois passos está disponível por predefinição para os administradores globais que tenham utilizadores do Azure Active Directory e do Office 365. No entanto, se pretender tirar partido das [funcionalidades avançadas](howto-mfa-mfasettings.md), deverá comprar a versão completa do Multi-Factor Authentication (MFA) do Azure.

Um fornecedor do multi-factor Auth do Azure é utilizado para tirar partido das funcionalidades fornecidas pelo Azure multi-factor Authentication para os utilizadores quem **não têm licenças**.

Se tiver licenças que abrangem todos os utilizadores na sua organização, em seguida, não terá um fornecedor do multi-factor Auth do Azure. Crie um fornecedor de autenticação do multi-factor do Azure apenas se também tem de fornecer a verificação de dois passos para alguns usuários que não têm licenças.

> [!NOTE]
> Em vigor a partir de 1 de Setembro de 2018 novos auth fornecedores já não podem ser criados. Fornecedores de autenticação existentes podem continuar a ser utilizado e atualizados. Multi-factor authentication irá continuar a ser uma funcionalidade disponível em licenças do Azure AD Premium.

## <a name="caveats-related-to-the-azure-mfa-sdk"></a>Avisos relacionados com o SDK de MFA do Azure

Para transferir o SDK, é preciso um fornecedor do Multi-Factor Auth do Azure. Tenha em atenção que o SDK foi preterido e já não é suportado para os novos clientes e só irá continuar a funcionar até 14 de Novembro de 2018. Após esse tempo, as chamadas para o SDK irão falhar.

Para transferir o SDK, crie um fornecedor do multi-factor Auth do Azure, mesmo que tenha o MFA do Azure, AAD Premium ou outras licenças agrupadas. Se criar um fornecedor do Multi-Factor Auth do Azure para esta finalidade e já tiver licenças, verifique se cria o Fornecedor com o modelo **Por Utilizador Ativado**. Em seguida, associe o fornecedor para o diretório que contém o MFA do Azure, Azure AD Premium ou outras licenças agrupadas. Esta configuração garante que só lhe será cobrado qualquer valor se tiver mais utilizadores exclusivos a efetuarem a verificação em dois passos do que o número de licenças.

## <a name="what-is-an-mfa-provider"></a>O que é um Fornecedor de MFA?

Existem dois tipos de fornecedores de autenticação, e a diferença em torno de como a sua subscrição do Azure é cobrada. A opção por autenticação calcula o número de autenticações executadas num mês relativamente ao seu inquilino. Esta é a melhor opção no caso de existirem vários utilizadores que são ocasionalmente autenticados. A opção por utilizador calcula o número de indivíduos no seu inquilino que executam a verificação de dois passos num mês. Esta é a melhor opção caso existam alguns utilizadores com licenças, mas tem de alargar a MFA a mais utilizadores para além dos limites da sua licença.

## <a name="manage-your-mfa-provider"></a>Gerir o Fornecedor de MFA

Não pode alterar o modelo de utilização (por utilizador ativado ou por autenticação) após a criação de um fornecedor do MFA.

Se tiver adquirido licenças suficientes para cobrir todos os utilizadores que estão ativados para a MFA, pode eliminar completamente o fornecedor do MFA.

Se o fornecedor do MFA não estiver associado a um inquilino do Azure AD ou associar o novo fornecedor do MFA a um inquilino diferente do Azure AD, as definições do utilizador e as opções de configuração não são transferidas. Além disso, os servidores de MFA do Azure existentes têm de ser reativados com credenciais de ativação geradas através do fornecedor de MFA. Reativar os servidores MFA para associá-los para o fornecedor do MFA não afeta chamadas telefónicas e de autenticação de mensagem de texto, mas as notificações da aplicação móvel deixam de funcionar para todos os utilizadores até que reativem a aplicação móvel.

## <a name="next-steps"></a>Passos Seguintes

[Configurar as Definições do Multi-Factor Authentication](howto-mfa-mfasettings.md)
