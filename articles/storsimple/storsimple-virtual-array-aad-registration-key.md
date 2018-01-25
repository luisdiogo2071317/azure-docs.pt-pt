---
title: "Nova autenticação para as matrizes de Virtual StorSimple | Microsoft Docs"
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
ms.openlocfilehash: 8d033cc09de8e115324067d7bbdf052751730d63
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="use-the-new-authentication-for-your-storsimple"></a>Utilizar a autenticação de novo para do StorSimple

## <a name="overview"></a>Descrição geral

O serviço do Gestor de dispositivos do StorSimple é executado no Microsoft Azure e liga-se a várias matrizes de Virtual StorSimple. Até à data, o serviço do Gestor de dispositivos do StorSimple utilizou um serviço de controlo de acesso (ACS) para autenticar o serviço para o dispositivo StorSimple. O mecanismo de ACS será preterido em breve e substituído por uma autenticação do Azure Active Directory (AAD).

As informações contidas neste artigo são aplicáveis a ambos os StorSimple 1200 série Virtual matrizes só. Este artigo descreve os detalhes da autenticação do AAD associado nova chave de registo de serviço e modificações para as regras de firewall, conforme aplicável aos dispositivos StorSimple.

A autenticação do AAD ocorre em matrizes de Virtual StorSimple (modelo 1200) com o Update 1 ou posterior.

Devido à introdução da autenticação do AAD, ocorrem alterações no:

- Padrões de URL para as regras de firewall.
- Chave de registo do serviço.

Estas alterações são abordadas em detalhe nas secções seguintes.

## <a name="url-changes-for-aad-authentication"></a>Alterações de URL para a autenticação do AAD

Para garantir que o serviço utiliza autenticação baseada no AAD, todos os utilizadores têm de incluir novos URLs de autenticação nas regras de firewall.

Se utilizar matriz Virtual StorSimple, certifique-se de que o seguinte URL está incluído nas regras da firewall:

| Padrão de URL                         | Nuvem | Componente/funcionalidade         |
|------------------------------------|-------|---------------------------------|
| `https://login.windows.net`        | Público do Azure |Serviço de autenticação do AAD      |
| `https://login.microsoftonline.us` | Governo dos Estados Unidos da América |Serviço de autenticação do AAD      |

Para uma lista completa de URL padrões para matrizes Virtual StorSimple, aceda a [padrões de URL para as regras de firewall](storsimple-ova-system-requirements.md#url-patterns-for-firewall-rules).

Se o URL de autenticação não está incluído nas regras da firewall para além da data de preterição, os utilizadores veem um alerta crítico que o dispositivo StorSimple não foi possível autenticar com o serviço. O serviço não conseguirá comunicar com o dispositivo. Se os utilizadores veem este alerta, tem de incluir o novo URL de autenticação. Para obter mais informações sobre o alerta, aceda a [utilizar alertas para monitorizar o dispositivo StorSimple](storsimple-virtual-array-manage-alerts.md#networking-alerts).

## <a name="device-version-and-authentication-changes"></a>Alterações de versão e a autenticação do dispositivo

Se utilizar uma matriz de Virtual StorSimple, utilize a tabela seguinte para determinar que ação precisa de tomar com base na versão do software de dispositivo que está a executar.

| Se o dispositivo estiver em execução  | Execute a ação seguinte                                    |
|----------------------------|--------------------------------------------------------------|
| Atualização 1.0 ou posterior e está offline. <br> Verá um alerta se o URL não está na lista de permissões.| Modificar as regras de firewall para incluir o URL de autenticação. Consulte [URLs de autenticação](#url-changes-for-aad-authentication). |
| Atualização 1.0 ou posterior e o dispositivo está online.| É necessária nenhuma ação.                                       |
| Atualização 0,6 ou anterior e o dispositivo está offline. | [Transferir a atualização 1.0 através do servidor de catálogo](storsimple-virtual-array-install-update-1.md#download-the-update-or-the-hotfix).<br>[Aplicar atualização 1.0 através da IU da web local](storsimple-virtual-array-install-update-1.md#install-the-update-or-the-hotfix). <br> [Obter a chave de registo do AAD do serviço de](#aad-based-registration-keys). <br> Execute passos 1 a 5 para [ligar a interface do Windows PowerShell da matriz virtual](storsimple-virtual-array-deploy2-provision-hyperv.md#step-2-provision-a-virtual-array-in-hypervisor).<br> Utilize `Invoke-HcsReRegister` cmdlet para registar o dispositivo através do Windows PowerShell. Forneça a chave que obteve no passo anterior.|
| Atualização 0,6 ou anterior e o dispositivo está online | Modificar as regras de firewall para incluir o URL de autenticação.<br> Instale a atualização 1.0 através do portal do Azure. |

## <a name="aad-based-registration-keys"></a>Chaves de registo baseados no AAD

A partir da atualização 1.0 para matrizes de Virtual StorSimple, novo registo com base no AAD as chaves são utilizadas. Pode utilizar as chaves de registo para registar o seu serviço do Gestor de dispositivos do StorSimple com o dispositivo.

Não é possível utilizar as novas chaves de registo do serviço AAD se estiver a utilizar um matrizes de Virtual StorSimple atualização 0,6 ou anterior em execução. Terá de voltar a gerar a chave de registo do serviço. Assim que voltar a gerar a chave, a nova chave é utilizada para registar todos os dispositivos subsequentes. A chave antiga já não é válida.

- A nova chave de registo do AAD expira após 3 dias.
- Matrizes do trabalho de chaves de registo do AAD apenas com StorSimple 1200 série virtual em execução Update 1 ou posterior. A chave de registo do AAD de um dispositivo de série 8000 do StorSimple não funcionará.
- As chaves de registo do AAD são mais do que as chaves de registo de ACS correspondentes.

Execute os seguintes passos para gerar uma chave de registo do serviço AAD.

#### <a name="to-generate-the-aad-service-registration-key"></a>Para gerar a chave de registo do serviço AAD

1. No **Gestor de dispositivos do StorSimple**, aceda a **gestão &gt;**  **chaves**.
    
    ![Aceda às chaves](./media/storsimple-virtual-array-aad-registration-key/aad-registration-key1.png)

2. Clique em **gerar chave**.

    ![Clique em voltar a gerar](./media/storsimple-virtual-array-aad-registration-key/aad-click-generate-registration-key.png)

3. Copie a chave de novo. A chave anterior já não funciona.

    ![Confirmar voltar a gerar](./media/storsimple-virtual-array-aad-registration-key/aad-registration-key2.png)

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre como implementar [matriz Virtual StorSimple](storsimple-virtual-array-deploy1-portal-prep.md)
