---
title: Remover os dados pessoais - Proxy de aplicações do Azure Active Directory | Microsoft Docs
description: Remova dados pessoais conectores instalados em dispositivos para o Proxy de aplicações do Azure Active Directory.
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 94cf0464aca3c46e0c6425b0fb3e24fcd767f95c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655331"
---
# <a name="remove-personal-data-for-azure-active-directory-application-proxy"></a>Remover dados pessoais para o Proxy de aplicações do Azure Active Directory  

Proxy de aplicações do Active Directory do Azure necessita da instalação conectores nos seus dispositivos, o que significa que poderá haver dados pessoais nos seus dispositivos. Este artigo fornece os passos sobre como eliminar os dados pessoais para melhorar a privacidade. 


## <a name="where-is-the-personal-data"></a>É onde os dados pessoais?
É possível que o Proxy da aplicação escrever dados pessoais os seguintes tipos de registo:

- Registos de eventos do conector
- Registos de eventos do Windows

## <a name="remove-personal-data-from-windows-event-logs"></a>Remover dados pessoais de registos de eventos do Windows

Para obter informações sobre como configurar a retenção de dados para os registos de eventos do Windows, consulte [as definições de registos de eventos](https://technet.microsoft.com/library/cc952132.aspx). Para saber mais sobre os registos de eventos do Windows, consulte [utilizando o registo de eventos do Windows](https://msdn.microsoft.com/library/windows/desktop/aa385772.aspx).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-hybrid-note.md)]

## <a name="remove-personal-data-from-connector-event-logs"></a>Remover dados pessoais de registos de eventos do conector

Para garantir que os registos de Proxy da aplicação não tem dados pessoais, pode:

- Elimine ou ver dados quando for necessário, ou
- Desativar o registo

Utilize as secções seguintes para remover dados pessoais de registos de eventos do conector. Tem de concluir o processo de remoção de todos os dispositivos em que o conector está instalado.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

### <a name="view-or-export-specific-data"></a>Ver ou exportar dados específicos

Para ver ou exportar dados específicos, procure as entradas relacionadas em cada um dos registos de eventos do conector. Os registos estão localizados em `C:\ProgramData\Microsoft\Microsoft AAD Application Proxy Connector\Trace`. 

Uma vez que os registos de ficheiros de texto, pode utilizar [findstr](https://docs.microsoft.com/windows-server/administration/windows-commands/findstr) para procurar entradas de texto relacionado com um utilizador.  

Para encontrar dados pessoais, procure os ficheiros de registo UserID. 

Para encontrar dados pessoais registados por uma aplicação que utiliza a delegação restrita de Kerberos, procure destes componentes do tipo de nome de utilizador:

- Nome principal de utilizador no local
- Parte do nome de utilizador do nome principal de utilizador
- Parte do nome de utilizador do nome principal de utilizador no local
- Nome de conta de gestor (SAM) de contas de segurança de no local 


### <a name="delete-specific-data"></a>Eliminar dados específicos

Para eliminar dados específicos:

1. Reinicie o serviço do conector do Proxy de aplicações do Microsoft Azure AD para gerar um novo ficheiro de registo. O novo ficheiro de registo permite-lhe eliminar ou modificar os ficheiros de registo antigos. 
2. Siga o [dados específicos, ver ou exportação](#view-or-export-specific-data) processo descrito anteriormente para localizar as informações que precisam de ser eliminado. Procura todos os registos de conector.
3. Elimine os ficheiros de registo relevantes ou eliminar seletivamente os campos que contêm dados pessoais. Também pode eliminar todos os ficheiros de registo antigos se não precisar delas.

### <a name="turn-off-connector-logs"></a>Desativar os registos de conector

É uma opção para garantir que os registos de conector não contêm dados pessoais desativar a geração de registo. Parar geração de registos de conector, remova a seguinte linha realçada de `C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config`. 

![Configuração](./media/application-proxy-remove-personal-data/01.png)


## <a name="next-steps"></a>Passos Seguintes

Para obter uma descrição geral do Proxy da aplicação, consulte [como fornecer acesso remoto seguro a aplicações no local](application-proxy.md).

