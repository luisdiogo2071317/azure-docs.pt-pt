---
title: "Do Azure AD Connect e regulamento de proteção de dados gerais | Microsoft Docs"
description: Este documento descreve como obter a conformidade de era GDPR com o Azure AD Connect.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2018
ms.author: billmath
ms.openlocfilehash: 68b36e1806d3338b07d4c2c400de233aff270132
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="gdpr-compliance-and-azure-ad-connect"></a>Conformidade GDPR e do Azure AD Connect 

Na Maio de 2018, leis de privacidade Europa, o [regulamento gerais de proteção de dados (GDPR)](http://ec.europa.eu/justice/data-protection/reform/index_en.htm), deve entrem em vigor. O GDPR impõe novas regras em empresas, agências governamentais, não lucros e outras organizações que oferta bens serviços e para as pessoas da União Europeia (EU) ou que recolher e analisam dados associados ao residentes de EU. O GDPR aplica-se, independentemente de onde estão localizadas. 

Produtos e serviços Microsoft, estão disponíveis atualmente para o ajudar a satisfazer os requisitos de GDPR. Saiba mais sobre a política de Microsoft Privacy [Centro de confiança](https://www.microsoft.com/trustcenter)

>[!NOTE] 
>Este artigo lida com o Azure AD Connect e GDPR compatibilidade.  Para obter informações sobre o Azure AD Connect Health e GDPR compatibilidade, consulte o artigo [aqui](../../active-directory/connect-health/active-directory-aadconnect-health-gdpr.md).

Pode ser contactada a conformidade de regulamento de proteção de dados gerais para o Azure AD Connect instalações de duas formas:

1.  Mediante pedido, a extrair dados para uma pessoa e remover dados dessa pessoa das instalações
2.  Certifique-se de que não existem dados são mantidos para além de 48 horas.

A equipa do conselhos o Azure AD Connect recomenda a segunda opção.  O motivo que está a ser, é que, é por até que ponto o facilitar a implementar e manter.

Um servidor de sincronização do Azure AD Connect armazena os dados que se encontra no âmbito de compatibilidade de GDPR:
1.  Dados sobre uma pessoa no **base de dados do Azure AD Connect**
2.  Dados de **registo de eventos do Windows** ficheiros que podem conter informações sobre uma pessoa
3.  Dados de **ficheiros de registo de instalação do Azure AD Connect** que pode conter sobre uma pessoa

GDPR em conformidade, os clientes do Azure AD Connect devem utilizar as seguintes diretrizes:
1.  Elimine o conteúdo da pasta que contém os ficheiros de registo de instalação do Azure AD Connect regularmente – pelo menos a cada 48 horas
2.  Este produto também pode criar registos de eventos.  Para saber mais sobre os registos de registos de eventos, consulte o [documentação aqui](https://msdn.microsoft.com/library/windows/desktop/aa385780.aspx).

Dados sobre uma pessoa são removidos automaticamente da base de dados do Azure AD Connect quando dados essa pessoa são removidos do sistema de origem onde foi originado. É necessária qualquer ação específica de administradores para ser GDPR em conformidade.  No entanto, requerem que os dados do Azure AD Connect estão sincronizados com a origem de dados, pelo menos, a cada dois dias.

## <a name="delete-the-azure-ad-connect-installation-log-file-folder-contents"></a>Elimina os conteúdos de pastas de ficheiro de registo de instalação de Azure AD Connect
Regularmente Verifique e elimine o conteúdo da **c:\programdata\aadconnect** pasta –, exceto para o **PersistedState.Xml** ficheiro. Este ficheiro mantém o estado da instalação anterior do Azure A ligar e é utilizado quando é efetuada uma instalação de atualização. Este ficheiro não contém quaisquer dados sobre uma pessoa e não deve ser eliminado.

>[!IMPORTANT]
>Não elimine o ficheiro PersistedState.xml.  Este ficheiro contém não existem informações de utilizador e mantém o estado da instalação anterior.

Pode rever e eliminar estes ficheiros através do Explorador do Windows ou pode utilizar um script, como o seguinte para executar as ações necessárias:


```
$Files = ((Get-childitem -Path "$env:programdata\aadconnect" -Recurse).VersionInfo).FileName
Foreach ($file in $files) {
If ($File.ToUpper() -ne "$env:programdata\aadconnect\PERSISTEDSTATE.XML".toupper()) # Do not delete this file
    {Remove-Item -Path $File -Force}
    } 
```

### <a name="schedule-this-script-to-run-every-48-hours"></a>Agendar este script para executar cada 48 horas
Utilize os seguintes passos para agendar o script para executar cada 48 horas.

1.  Guarde o script num ficheiro com a extensão **&#46; PS1**, em seguida, abra o painel de controlo e clique em **sistemas e segurança**.
    ![Sistema](media\active-directory-aadconnect-gdpr\gdpr2.png)

2.  Sob o cabeçalho de ferramentas administrativas, clique em **agenda de tarefas**.
    ![Tarefa](media\active-directory-aadconnect-gdpr\gdpr3.png)
3.  No programador de tarefas, clique em **biblioteca de agendamento de tarefas** e clique em **tarefas criar básicas...**
4.  Introduza o nome para a nova tarefa e clique em **seguinte**.
5.  Selecione **diária** para o acionador da tarefa e clique em **seguinte**.
6.  Definir a periodicidade **2 dias** e clique em **seguinte**.
7.  Selecione **iniciaram um programa** como a ação e clique em **seguinte**.
8.  Tipo **PowerShell** na caixa para o programa/script e, na caixa de etiqueta **adicionar argumentos (opcionais)**, introduza o caminho completo para o script que criou anteriormente, em seguida, clique em **seguinte**.
9.  O aparecimento do ecrã mostra um resumo da tarefa que está prestes a criar. Verifique os valores e clique em **concluir** para criar a tarefa.



## <a name="next-steps"></a>Passos Seguintes
- [Integrar as identidades no local ao Azure Active Directory](active-directory-aadconnect.md).
- [Estado de funcionamento e GDPR do Azure AD Connect](../../active-directory/connect-health/active-directory-aadconnect-health-gdpr.md)