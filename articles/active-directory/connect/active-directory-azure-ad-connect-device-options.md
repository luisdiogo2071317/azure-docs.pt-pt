---
title: 'Do Azure AD Connect: Opções de dispositivo | Microsoft Docs'
description: Este documento fornece detalhes sobre as opções de dispositivos disponíveis no Azure AD Connect
services: active-directory
documentationcenter: ''
author: anandy
manager: samueld
editor: billmath
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: anandy
ms.openlocfilehash: d592cbb55643099b6b628a44f182d0a079a6c71e
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
#<a name="azure-ad-connect-device-options"></a>Do Azure AD Connect: Opções de dispositivo

A seguinte documentação fornece informações sobre as várias opções de dispositivo disponíveis no Azure AD Connect. Pode utilizar o Azure AD Connect para configurar as seguintes duas operações: 
* **A associação do Azure AD híbrido**: se no local para o seu ambiente tem requisitos de espaço do AD e também queira benefício as capacidades fornecidas pelo Azure Active Directory, pode implementar híbrida do Azure AD associado dispositivos. Estes são os dispositivos que estejam ambos, associado ao seu no local do Active Directory e o Azure Active Directory.
* **Repetição de escrita do dispositivo**: repetição de escrita do dispositivo é utilizada para ativar o acesso condicional com base nos dispositivos para o AD FS (2012 R2 ou superior) protegido dispositivos

## <a name="configure-device-options-in-azure-ad-connect"></a>Configurar opções de dispositivo no Azure AD Connect

1.  A execução do Azure AD Connect. No **tarefas adicionais** página, selecione **configurar opções de dispositivo**.
    ![Configurar opções de dispositivo](./media/active-directory-aadconnect-device-options/deviceoptions.png) 

    Ao clicar em seguinte, um **descrição geral** é apresentada a página, que fornece detalhes sobre as operações que podem ser executadas.
    ![Descrição geral](./media/active-directory-aadconnect-device-options/deviceoverview.png)

    >[!NOTE]
    > As novas opções de dispositivo configurar está disponível apenas na versão 1.1.819.0 e mais recente.

2.  Depois de fornecer as credenciais para o Azure AD, pode selecionar a operação ser efetuada na página de opções de dispositivo.
    ![Operações de dispositivo](./media/active-directory-aadconnect-device-options/deviceoptionsselection.png)

## <a name="next-steps"></a>Passos Seguintes

* [Configurar a associação do Azure AD híbrido](../device-management-hybrid-azuread-joined-devices-setup.md)
* [Configure / desativar a repetição de escrita do dispositivo](./active-directory-aadconnect-feature-device-writeback.md)

