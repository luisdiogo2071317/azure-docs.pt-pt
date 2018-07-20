---
title: Como configurar a repetição de escrita de palavra-passe para o Azure AD SSPR
description: Utilize o Azure AD e o Azure AD Connect para palavras-passe de repetição de escrita para um diretório no local
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: e613ff742096077fe1765d4b855b6c7d409cc228
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/19/2018
ms.locfileid: "39158952"
---
# <a name="how-to-configure-password-writeback"></a>Como: Configurar a repetição de escrita de palavra-passe

Recomendamos que utilize a funcionalidade de atualização automática do [do Azure AD Connect](./../connect/active-directory-aadconnect-get-started-express.md) ao utilizar a repetição de escrita de palavra-passe.

Os passos seguintes partem do princípio de já ter configurado o Azure AD Connect no seu ambiente utilizando o [Express](./../connect/active-directory-aadconnect-get-started-express.md) ou [personalizado](./../connect/active-directory-aadconnect-get-started-custom.md) definições.

1. Para configurar e ativar a repetição de escrita de palavra-passe, inicie sessão no seu servidor do Azure AD Connect e iniciar o **do Azure AD Connect** Assistente de configuração.
2. Sobre o **bem-vindo** página, selecione **configurar**.
3. Sobre o **tarefas adicionais** página, selecione **personalizar as opções de sincronização**e, em seguida, selecione **seguinte**.
4. Sobre o **ligar para o Azure AD** página, introduza uma credencial de administrador global e, em seguida, selecione **próxima**.
5. Sobre o **ligar diretórios** e **domínio/UO** filtragem de páginas, selecione **seguinte**.
6. Sobre o **funcionalidades opcionais** página, selecione a caixa junto a **repetição de escrita de palavra-passe** e selecione **seguinte**.
   ![Ativar a repetição de escrita de palavra-passe no Azure AD Connect][Writeback]
7. Sobre o **pronto para configurar** página, selecione **configurar** e aguarde o conclusão do processo.
8. Quando vir a configuração concluir, selecione **saída**.

Para tarefas de resolução de problemas comuns relacionados com a repetição de escrita de palavra-passe, consulte a secção [resolver problemas de repetição de escrita de palavra-passe](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) no nosso artigo de resolução de problemas.

## <a name="active-directory-permissions"></a>Permissões do Active Directory

A conta especificada no utilitário do Azure AD Connect tem os seguintes itens definiu se quiser ser no âmbito do SSPR:

* **Repor palavra-passe** 
* **Alterar palavra-passe** 
* **Permissões de escrita** no `lockoutTime`
* **Permissões de escrita** no `pwdLastSet`
* **Expandido direitos** em qualquer uma:
   * O objeto raiz de *cada domínio* nessa floresta
   * As unidades organizacionais (UOs) de utilizador que pretende no escopo para SSPR

Se não tiver a certeza sobre a conta a conta descrita refere-se, abra a IU de configuração do Azure Active Directory Connect e selecione o **ver configuração atual** opção. A conta que tem de adicionar permissão está listado em **diretórios sincronizados**.

Se definir estas permissões, a conta de serviço MA para cada floresta pode gerir as palavras-passe em nome das contas de utilizador nessa floresta. 

> [!IMPORTANT]
> Se não atribuir estas permissões, em seguida, mesmo que a repetição de escrita parece estar configurada corretamente, os utilizadores receberão erros quando tentarem gerir as respetivas palavras-passe no local na nuvem.
>

> [!NOTE]
> Pode demorar até uma hora ou mais até que estas permissões sejam replicadas para todos os objetos no seu diretório.
>

Para configurar as permissões adequadas para a repetição de escrita de palavra-passe ocorrer, conclua os seguintes passos:

1. Abra o Active Directory utilizadores e computadores com uma conta que possua as permissões de administração do domínio adequado.
2. Do **View** menu, certifique-se **funcionalidades avançadas** está ativada.
3. No painel esquerdo, clique no objeto que representa a raiz do domínio e selecione **propriedades** > **segurança** > **avançadas**.
4. Partir do **permissões** separador, selecione **Add**.
5. Escolha a conta que as permissões estão a ser aplicadas a (a partir do programa de configuração do Azure AD Connect).
6. Na **aplica-se ao** na lista pendente, selecione **utilizadores descendentes** objetos.
7. Sob **permissões**, selecione as caixas para o seguinte:
    * **Repor palavra-passe**
    * **Alterar palavra-passe**
    * **Escrever lockoutTime**
    * **Escrever pwdLastSet**
8. Selecione **aplicar/OK** para aplicar as alterações e sair quaisquer caixas de diálogo de abertura.

## <a name="next-steps"></a>Passos Seguintes

[O que é a repetição de escrita de palavra-passe?](concept-sspr-writeback.md)

[Writeback]: ./media/howto-sspr-writeback/enablepasswordwriteback.png "Ativar a repetição de escrita de palavra-passe no Azure AD Connect"
