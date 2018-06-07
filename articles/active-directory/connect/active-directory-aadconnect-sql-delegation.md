---
title: Instalar o Azure AD Connect com permissões de administrador do SQL Server delegado | Microsoft Docs
description: Este tópico descreve uma atualização para o Azure AD Connect, que permite a instalação utilizando uma conta que tenha apenas permissões de dbo do SQL Server.
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.reviewer: jparsons
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: d0cfce1dfe6482397e421d85d59494e6d75cc441
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34591162"
---
# <a name="install-azure-ad-connect-using-sql-delegated-administrator-permissions"></a>Instalar o Azure AD Connect utilizando as permissões de administrador delegado de SQL
Antes de compilação mais recente do Azure AD Connect, administrativa delegação, ao implementar as configurações necessárias do SQL Server, não era suportada.  Os utilizadores que pretendem instalar o Azure AD Connect é necessário ter permissões de administrador (SA) no SQL server.

Com a versão mais recente do Azure AD Connect, o aprovisionamento a base de dados pode agora ser efetuado fora de banda pelo administrador do SQL Server e, em seguida, instalado pelo administrador do Azure AD Connect com direitos de proprietário da base de dados.

## <a name="before-you-begin"></a>Antes de começar
Para utilizar esta funcionalidade, terá de tenha em atenção que existem várias partes mover e cada um deles pode envolver um administrador diferente na sua organização.  A tabela seguinte resume as funções individuais e os seus deveres respetivos na implementação do Azure AD Connect com esta funcionalidade.

|Função|Descrição|
|-----|-----|
|Domínio ou o administrador do AD de floresta|Cria a conta de serviço de nível de domínio que é utilizada pelo Azure AD Connect para executar o serviço de sincronização.  Para obter mais informações sobre as contas de serviço, consulte [contas e permissões](active-directory-aadconnect-accounts-permissions.md).
|Administrador do SQL|Cria a base de dados ADSync e atribui o início de sessão + dbo acesso ao administrador do Azure AD Connect e a conta de serviço criado pelo administrador floresta/domínio.|
Administrador do Azure AD Connect|Instala o Azure AD Connect e especifica a conta de serviço durante a instalação personalizada.

## <a name="steps-for-installing-azure-ad-connect-using-sql-delegated-permissions"></a>Passos para instalar o Azure AD Connect com o SQL permissões delegadas
Para aprovisionar a base de dados fora de banda e instalar o Azure AD Connect com permissões de proprietário da base de dados, utilize os seguintes passos.

>[!NOTE]
>Embora não seja necessário, é **altamente recomendado** que o agrupamento de Latin1_General_CI_AS está selecionado ao criar a base de dados.


1.  Tem do administrador do SQL Server crie a base de dados ADSync com uma sequência de agrupamento sensível **(Latin1_General_CI_AS)**.  A base de dados tem de ter um nome **ADSync**.  O modelo de recuperação, o nível de compatibilidade e o tipo de contenção são atualizados para os valores corretos quando o Azure AD Connect está instalado.  No entanto, a sequência de agrupamento tem de ser definida corretamente pelo administrador do SQL do Azure AD Connect irá bloquear a instalação.  Para recuperar a SA tem de eliminar e recriar a base de dados.</br>
![Agrupamento](media/active-directory-aadconnect-sql-delegation/sql4.png)
2.  Conceda ao administrador do Azure AD Connect e a conta de serviço de domínio as seguintes permissões:
    - Início de sessão do SQL 
    - **base de dados owner(dbo)** direitos.  </br>
![Permissões](media/active-directory-aadconnect-sql-delegation/sql3.png)
3.  Envie um e-mail para o administrador do Azure AD Connect que indica o nome de servidor e instância SQL que deve ser utilizado ao instalar o Azure AD Connect.

## <a name="additional-information"></a>Informações adicionais
Assim que a base de dados é aprovisionado, o administrador do Azure AD Connect pode instalar e configurar a sincronização de no local no respetiva conveniência.  

O **/UseExistingDatabase** sinalizador é necessário quando utilizar uma base de dados previamente criada.  Não é apenas utilizado em situações de recuperação.

Para além de suporte para novas instalações do Azure AD Connect, esta funcionalidade permite também que a delegação para qualquer cenário relacionados com o **/UseExistingDatabase** sinalizador.  Para obter mais informações sobre como instalar o Azure AD Connect com uma base de dados existente, consulte [instalar o Azure AD Connect utilizando uma base de dados ADSync](active-directory-aadconnect-existing-database.md)


## <a name="next-steps"></a>Passos Seguintes
- [Introdução ao Azure AD Connect com as definições rápidas](active-directory-aadconnect-get-started-express.md)
- [Instalação personalizada do Azure AD Connect](active-directory-aadconnect-get-started-custom.md)
- [Instalar o Azure AD Connect, utilizando uma base de dados ADSync](active-directory-aadconnect-existing-database.md)  
