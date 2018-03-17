---
title: "GDPR no proxy de aplicações do Azure Active Directory | Microsoft Docs"
description: "Saiba mais sobre GDPR no proxy de aplicações do Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: c7186f98-dd80-4910-92a4-a7b8ff6272b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2018
ms.author: markvi
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 5e99fba2f226e1e9b401bd8ef5b3b85a5e8c6a7c
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
---
# <a name="gdpr-in-the-azure-active-directory-application-proxy"></a>GDPR no proxy de aplicações do Azure Active Directory  

Proxy de aplicações do Azure Active Directory (Azure AD) está em conformidade GDPR juntamente com todos os outros serviços Microsoft e funcionalidades. Para saber mais sobre o suporte GDPR da Microsoft, consulte [os termos de licenciamento e a documentação](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31).

Porque esta funcionalidade inclui conectores nos seus computadores, existem alguns eventos que precisar de monitorizar para se manter GDPR em conformidade. Proxy de aplicações, cria os seguintes tipos de registo, que podem conter EUII:

- Registos de eventos do conector
- Registos de eventos do Windows

Existem duas formas das manter GDPR em conformidade:

- Eliminar e exportar pedidos que possam surgir

- Desativar o registo

Para:

- Obter informações sobre como configurar a retenção de dados para os registos de eventos do Windows, consulte [as definições de registos de eventos](https://technet.microsoft.com/library/cc952132.aspx). 
- Informações gerais sobre o registo de eventos do Windows, consulte [utilizando o registo de eventos do Windows](https://msdn.microsoft.com/library/windows/desktop/aa385772.aspx).


Pode encontrar os registos de eventos de ligar ao `C:\ProgramData\Microsoft\Microsoft AAD Application Proxy Connector\Trace`. As secções seguintes fornecem os passos relacionados para os registos de eventos do conector. Tem de concluir estes passos em todos os computadores de conector.
 

## <a name="processing-requests"></a>Processamento de pedidos

Existem três tipos diferentes de pedidos que é responsabilizados por: 

- Eliminar
- Vista
- Exportar
 
Para processar a vista / exportar pedidos, tem de passar por cada um dos ficheiros de registo para procurar as entradas relacionadas. 

Porque os registos são ficheiros de texto, pode procurar por, por exemplo, utilizando o `findstr` comando para encontrar as entradas relacionadas com o utilizador. Procurar os seguintes campos, porque podem ser nos registos: 

- ID de utilizador
- O tipo de nome de utilizador configurado para todas as aplicações utilizando a delegação restrita Kerberos:
    - Nome principal de utilizador
    - Nome principal de utilizador no local
    - Parte do nome de utilizador do nome principal de utilizador
    - Parte do nome de utilizador do nome principal de utilizador no local
    - Nome da conta SAM no local 

 
Em seguida, pode recolher estes campos e partilhá-los com o utilizador.
Para processar pedidos de eliminação, tem de eliminar os registos relevantes. Pode reiniciar o serviço do conector (Microsoft Azure AD aplicação conector do Proxy) para gerar um novo ficheiro de registo. O novo ficheiro de registo permite-lhe eliminar os ficheiros de registo antigos. Pode, em seguida, siga o processo para ver / exportar para localizar todos os registos relevantes e seletivamente eliminar esses campos ou ficheiros. Pode eliminar todos os ficheiros de registo antigos também sempre apenas se não precisar delas.


## <a name="turn-off-connector-logs"></a>Desativar os registos de conector

Para desativar os registos de conector, terá de ajustar `C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config` ao remover a linha realçada: 


![Configuração](./media/active-directory-application-proxy-gdpr/01.png)


## <a name="next-steps"></a>Passos Seguintes

Para obter uma descrição geral do proxy de aplicações do Azure AD, consulte [como fornecer acesso remoto seguro a aplicações no local](active-directory-application-proxy-get-started.md).

