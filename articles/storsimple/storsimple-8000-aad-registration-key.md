---
title: "Utilize a autenticação de novo para o serviço do Gestor de dispositivos de 8000 do StorSimple no Azure | Microsoft Docs"
description: "Explica como utilizar a autenticação do AAD com base para o seu serviço, gerar nova chave de registo e efetuar o registo manual dos dispositivos."
services: storsimple
documentationcenter: 
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: alkohli
ms.openlocfilehash: 37f44538d94ed78509bbcb09e726dc34a9e92e95
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="use-the-new-authentication-for-your-storsimple"></a>Utilizar a autenticação de novo para do StorSimple

## <a name="overview"></a>Descrição geral

O serviço do Gestor de dispositivos do StorSimple é executado no Microsoft Azure e se liga a diversos dispositivos StorSimple. Até à data, o serviço do Gestor de dispositivos do StorSimple utilizou um serviço de controlo de acesso (ACS) para autenticar o serviço para o dispositivo StorSimple. O mecanismo de ACS será preterido em breve e substituído por uma autenticação do Azure Active Directory (AAD). Para obter mais informações, avance para os anúncios seguintes para descontinuação dos ACS e a utilização da autenticação do AAD.

- [O futuro da Azure ACS é o Azure Active Directory](https://cloudblogs.microsoft.com/enterprisemobility/2015/02/12/the-future-of-azure-acs-is-azure-active-directory/)
- [Alterações futuras ao serviço de controlo de acesso da Microsoft](https://azure.microsoft.com/en-in/blog/acs-access-control-service-namespace-creation-restriction/)

Este artigo descreve os detalhes da autenticação do AAD associado nova chave de registo de serviço e modificações para as regras de firewall, conforme aplicável aos dispositivos StorSimple. As informações contidas neste artigo são aplicáveis apenas a dispositivos série 8000 do StorSimple.

A autenticação do AAD ocorre no dispositivo de série 8000 do StorSimple com Update 5 ou posterior. Devido à introdução da autenticação do AAD, ocorrem alterações no:

- Padrões de URL para as regras de firewall.
- Chave de registo do serviço.

Estas alterações são abordadas em detalhe nas secções seguintes.

## <a name="url-changes-for-aad-authentication"></a>Alterações de URL para a autenticação do AAD

Para garantir que o serviço utiliza autenticação baseada no AAD, todos os utilizadores têm de incluir novos URLs de autenticação nas regras de firewall.

Se utilizar série 8000 do StorSimple, certifique-se de que o seguinte URL está incluído nas regras da firewall:

| Padrão de URL                         | Nuvem | Componente/funcionalidade         |
|------------------------------------|-------|----------------------------------|
| `https://login.windows.net`        | Público do Azure |Serviço de autenticação do AAD      |
| `https://login.microsoftonline.us` | Governo dos Estados Unidos da América |Serviço de autenticação do AAD      |

Para uma lista completa de URL padrões para dispositivos de série 8000 do StorSimple, aceda a [padrões de URL para as regras de firewall](storsimple-8000-system-requirements.md#url-patterns-for-firewall-rules).

Se o URL de autenticação não está incluído nas regras da firewall para além da data de preterição e o dispositivo está em execução Update 5, os utilizadores veem um alerta de URL. Os utilizadores têm de incluir o novo URL de autenticação. Se o dispositivo estiver em execução uma versão antes da atualização 5, os utilizadores veem um alerta de heartbeat. Em cada caso, não é possível autenticar o dispositivo StorSimple com o serviço e o serviço não é capaz de comunicar com o dispositivo.

## <a name="device-version-and-authentication-changes"></a>Alterações de versão e a autenticação do dispositivo

Se utilizar um dispositivo de série 8000 do StorSimple, utilize a tabela seguinte para determinar que ação precisa de tomar com base na versão do software de dispositivo que está a executar.

| Se o dispositivo estiver em execução| Execute a ação seguinte                                    |
|--------------------------|------------------------|--------------------|--------------------------------------------------------------|
| Atualização 5 ou posterior e o dispositivo está offline. <br> Verá um alerta se o URL não está na lista de permissões.| Modificar as regras de firewall para incluir o URL de autenticação.<br> Consulte [URLs de autenticação](#url-changes-for-aad-authentication). |
| Atualização 5 ou posterior e o dispositivo online.| É necessária nenhuma ação.                                       |
| Atualização 4 ou anterior e o dispositivo está offline. | Modificar as regras de firewall para incluir o URL de autenticação.<br>[Transferir a atualização 5 através do servidor de catálogo](storsimple-8000-install-update-5.md#download-updates-for-your-device).<br>[Aplicar a atualização 5 através do método de correção](storsimple-8000-install-update-5.md#install-update-5-as-a-hotfix). <br> [Obter a chave de registo do AAD do serviço de](#aad-based-registration-keys). <br> [Ligar a interface do Windows PowerShell do dispositivo de série 8000 do StorSimple](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console). <br>Utilize `Redo-DeviceRegistration` cmdlet para registar o dispositivo através do Windows PowerShell. Forneça a chave que obteve no passo anterior.|
| Atualização 4 ou anterior e o dispositivo está online. |Modificar as regras de firewall para incluir o URL de autenticação.<br> Instale a atualização 5 através do portal do Azure.              |
| Repor definições de fábrica para uma versão antes da atualização 5.      |O portal mostra uma chave de registo com base do AAD, enquanto o dispositivo está a executar o software mais antigo. Siga os passos do cenário anterior para quando o dispositivo está em execução atualização 4 ou anterior.              |

## <a name="aad-based-registration-keys"></a>Chaves de registo baseados no AAD

A partir do Update 5 para dispositivos de série 8000 do StorSimple, novo registo com base no AAD as chaves são utilizadas. Pode utilizar as chaves de registo para registar o seu serviço do Gestor de dispositivos do StorSimple com o dispositivo.

Não é possível utilizar as novas chaves de registo do serviço AAD se estiver a utilizar um dispositivo de série 8000 do StorSimple, executar a atualização 4 ou anterior (inclui um dispositivo mais antigo que está a ser ativado agora).
Neste cenário, terá de voltar a gerar a chave de registo do serviço. Assim que voltar a gerar a chave, a nova chave é utilizada para registar todos os dispositivos subsequentes. A chave antiga já não é válida.

- A nova chave de registo do AAD expira após 3 dias.
- As AAD as chaves de registo funcionam apenas com dispositivos de série 8000 do StorSimple com Update 5 ou posteriores.
- As chaves de registo do AAD são mais do que as chaves de registo de ACS correspondentes.

Execute os seguintes passos para gerar uma chave de registo do serviço AAD.

#### <a name="to-generate-the-aad-service-registration-key"></a>Para gerar a chave de registo do serviço AAD

1. No **Gestor de dispositivos do StorSimple**, aceda a **gestão &gt;**  **chaves**. Também pode utilizar a barra de procura para procurar _chaves_.
    
2. Clique em **gerar chave**.

    ![Clique em voltar a gerar](./media/storsimple-8000-aad-registration-key/aad-click-generate-registration-key.png)

3. Copie a chave de novo. A chave anterior deixará de funcionar.

    ![Confirmar voltar a gerar](./media/storsimple-8000-aad-registration-key/aad-registration-key2.png)

    > [!NOTE] 
    > Se estiver a criar uma aplicação de nuvem do StorSimple no serviço registado para o seu dispositivo de série 8000 do StorSimple, não gere uma chave de registo enquanto está em curso a criação. Aguarde que a criação seja concluída e, em seguida, gerar a chave de registo.

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre como implementar [dispositivos de série 8000 do StorSimple](storsimple-8000-deployment-walkthrough-u2.md).

