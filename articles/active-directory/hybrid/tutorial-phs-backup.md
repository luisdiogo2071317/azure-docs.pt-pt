---
title: 'Tutorial:  Configurar o PHS como cópia de segurança para o AD FS no Azure AD Connect | Documentos da Microsoft'
description: Demonstra como ativar a sincronização de hash de palavra-passe como uma cópia de segurança e para o AD FS.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: tutorial
ms.date: 01/30/2019
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: fb35f72d45cbdf1690329b0bf146fce4d47d0bd6
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55490112"
---
# <a name="tutorial--setting-up-phs-as-backup-for-ad-fs-in-azure-ad-connect"></a>Tutorial:  Configurar o PHS como cópia de segurança para o AD FS no Azure AD Connect

O tutorial seguinte irá guiá-lo ao configurar a sincronização de hash de palavra-passe como uma cópia de segurança e a ativação pós-falha para o AD FS.  Este documento também demonstrarei como ativar a sincronização de hash de palavra-passe como o método de autenticação primária, se o AD FS tem falhou ou se tornar indisponível.

## <a name="prerequisites"></a>Pré-requisitos
Neste tutorial tem por base o [Tutorial: Federar um ambiente de floresta único do AD para a cloud](tutorial-federation.md) e é um por-requisitos antes de tentar este tutorial.  Se não tiver concluído este tutorial, faça-o antes de repetir os passos neste documento.

## <a name="enable-phs-in-azure-ad-connect"></a>Ativar o PHS no Azure AD Connect
É a primeira etapa, agora que temos um ambiente do Azure AD Connect que está a utilizar o Federação ativar a sincronização de hash de palavra-passe e permitir que o Azure AD Connect para sincronizar os hashes.

Faça o seguinte:

1.  Faça duplo clique no ícone do Azure AD Connect que foi criado no ambiente de trabalho
2.  Clique em **Configurar**.
3.  Na página tarefas adicionais, selecione **personalizar as opções de sincronização** e clique em **próxima**.
4.  Introduza o nome de utilizador e palavra-passe para o seu administrador global.  Esta conta foi criada [aqui](tutorial-federation.md#create-a-global-administrator-in-azure-ad) no tutorial anterior.
5.  Sobre o **ligar os seus diretórios** ecrã, clique em **próxima**.
6.  Sobre o **domínio e UO filtragem** ecrã, clique em **próxima**.
7.  Sobre o **funcionalidades opcionais** ecrã, verificação **sincronização de hash de palavra-passe** e clique em **seguinte**.
![Selecionar](media/tutorial-phs-backup/backup1.png)</br>
8.  Sobre o **pronto para configurar** ecrã clique **configurar**.
9.  Uma vez concluída a configuração, clique em **saída**.
10. Já está!  Isso é tudo.  Sincronização de hash de palavra-passe agora ocorrerá e pode ser utilizada como uma cópia de segurança se do AD FS ficar indisponível.

## <a name="switch-to-password-hash-synchronization"></a>Mude para a sincronização de hash de palavra-passe
Agora, mostraremos como mudar para a sincronização de hash de palavra-passe se for necessário.  Faça o seguinte:

1. Faça duplo clique no ícone do Azure AD Connect que foi criado no ambiente de trabalho
2.  Clique em **Configurar**.
3.  Selecione **alterar utilizador inicie sessão** e clique em **próxima**.
![Alteração](media/tutorial-phs-backup/backup2.png)</br>
4.  Introduza o nome de utilizador e palavra-passe para o seu administrador global.  Esta conta foi criada [aqui](tutorial-federation.md#create-a-global-administrator-in-azure-ad) no tutorial anterior.
5.  Na **sessão do utilizador** ecrã, selecione **sincronização de Hash de palavra-passe** e coloque uma marca no **não converter contas de utilizador** caixa.  
6.  Deixe a predefinição **ativar o início de sessão único** selecionada e clique em **próxima**.
7.  Sobre o **ativar o início de sessão único** ecrã clique **seguinte**.
8.  Sobre o **pronto para configurar** ecrã, clique em **configurar**.
9.  Assim que a configuração estiver concluída, clique em **saída**.
10. Os utilizadores podem agora utilizar as palavras-passe para iniciar sessão no Azure e Azure services.

## <a name="test-signing-in-with-one-of-our-users"></a>Testar início de sessão com um dos nossos usuários

1.  Navegue para [http://myapps.microsoft.com](http://myapps.microsoft.com)
2. Inicie sessão com uma conta de utilizador que foi criada no nosso novo inquilino.  Terá de iniciar sessão com o seguinte formato: (user@domain.onmicrosoft.com). Utilize a mesma palavra-passe que o utilizador utiliza para iniciar sessão no local.</br>
![Certifique-se](media/tutorial-password-hash-sync/verify1.png)</br>

## <a name="next-steps"></a>Próximos Passos


- [Hardware e pré-requisitos](how-to-connect-install-prerequisites.md) 
- [Definições rápidas](how-to-connect-install-express.md)
- [Sincronização de hash de palavra-passe](how-to-connect-password-hash-synchronization.md)|
