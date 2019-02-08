---
title: Acesso de dispositivo de Gateway de caixa de dados do Microsoft Azure, power e modo de conectividade | Documentos da Microsoft
description: Descreve como gerir o acesso, energia e modo de conectividade para o dispositivo de Gateway de caixa de dados do Azure que ajuda a transferir dados para o Azure
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 02/07/2019
ms.author: alkohli
ms.openlocfilehash: 0ad94799320e25d88f616117f1bfcf9f0513aadf
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55873024"
---
# <a name="manage-access-power-and-connectivity-mode-for-your-azure-data-box-gateway-preview"></a>Gerir o acesso, energia e modo de conectividade para o Gateway de caixa de dados do Azure (pré-visualização)

Este artigo descreve como gerir o modo de conectividade, power e acesso para o Gateway de caixa de dados do Azure. Estas operações são efetuadas através da IU da web local ou o portal do Azure.

Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Gerir o acesso do dispositivo
> * Gerenciar o modo de conectividade
> * Gerenciar a energia

> [!IMPORTANT]
> O Data Box Gateway está em pré-visualização. Reveja os [Termos de serviço do Azure para pré-visualização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de encomendar e implementar esta solução.

## <a name="manage-device-access"></a>Gerir o acesso do dispositivo

O acesso ao seu dispositivo de Gateway de caixa de dados é controlado pelo uso de uma palavra-passe de administrador do dispositivo. Pode alterar a palavra-passe de administrador através da IU da web local. Também pode repor a palavra-passe de administrador do dispositivo no portal do Azure.

### <a name="change-device-administrator-password"></a>Alterar senha de administrador do dispositivo

Siga estes passos na IU do local para alterar a palavra-passe de administrador do dispositivo.

1. Na IU da web local, aceda a **manutenção > alteração de palavra-passe**.
2. Introduza a palavra-passe atual e, em seguida, a nova palavra-passe. A palavra-passe fornecida tem de ter entre 8 e 16 carateres. A palavra-passe tem de ter 3 dos seguintes carateres: carateres em maiúsculas, minúsculas, numéricos e especiais. Certifique-se a nova palavra-passe.

    ![Alterar palavra-passe](media/data-box-gateway-manage-access-power-connectivity-mode/change-password-1.png)

3. Clique em **alterar palavra-passe**.
 
### <a name="reset-device-administrator-password"></a>Repor palavra-passe de administrador do dispositivo

O fluxo de trabalho de reposição não exige que o usuário de recuperar a palavra-passe antiga e é útil quando a palavra-passe é perdida. Este fluxo de trabalho é executado no portal do Azure.

1. No portal do Azure, aceda a **descrição geral > Repor a palavra-passe do administrador**.

    ![Repor palavra-passe](media/data-box-gateway-manage-access-power-connectivity-mode/reset-password-1.png)

 
2. Introduza a palavra-passe nova e, em seguida, confirme-lo. A palavra-passe fornecida tem de ter entre 8 e 16 carateres. A palavra-passe tem de ter 3 dos seguintes carateres: carateres em maiúsculas, minúsculas, numéricos e especiais. Clique em **repor**.

    ![Repor palavra-passe](media/data-box-gateway-manage-access-power-connectivity-mode/reset-password-2.png)

## <a name="manage-connectivity-mode"></a>Gerenciar o modo de conectividade

Além do modo normal padrão, o seu dispositivo também pode executar no modo desligado parcialmente ou desligado. Cada um desses modos é descrita abaixo:

- **Parcialmente desligado** – neste modo, o dispositivo não é possível carregar quaisquer dados para as partilhas no entanto podem ser gerenciados através do portal do Azure.

    Este modo é normalmente utilizado quando numa rede com tráfego limitado da satélite e o objetivo é minimizar o consumo de largura de banda de rede. Consumo de rede mínima ainda pode ocorrer para operações de monitorização de dispositivos.

- **Desligado** – neste modo, o dispositivo estiver totalmente desligado da cloud e ambos os cloud carregamentos e transferências estão desativadas. O dispositivo só pode ser gerido através da IU da web local.

    Este modo é normalmente utilizado quando deseja colocar offline o seu dispositivo.

Para alterar o modo do dispositivo, siga estes passos:

1. Na IU da web local do seu dispositivo, aceda a **configuração > definições da Cloud**.
2. Desativar a **Cloud carregamento e transferência**.
3. Para executar o dispositivo no modo desligado parcialmente, ative **do Azure management portal**.

    ![Modo de conetividade](media/data-box-gateway-manage-access-power-connectivity-mode/connectivity-mode-1.png)
 
4. Para executar o dispositivo no modo desligado, desative **do Azure management portal**. Agora, o dispositivo só pode ser gerido através da IU da web local.

    ![Modo de conetividade](media/data-box-gateway-manage-access-power-connectivity-mode/connectivity-mode-2.png)

## <a name="manage-power"></a>Gerenciar a energia

Pode encerrar ou reiniciar o seu dispositivo, físico e virtual, com a IU web local. Recomendamos que antes de reiniciar, coloque as partilhas offline no anfitrião e, em seguida, no dispositivo. Esta ação minimiza qualquer possibilidade de Corrupção de dados.

1. Na IU da web local, aceda a **manutenção > definições de energia**.
2. Clique em **encerramento** ou **reiniciar** consoante o que pretende fazer.

    ![Definições de energia](media/data-box-gateway-manage-access-power-connectivity-mode/shut-down-restart-1.png)

3. Quando lhe for pedida confirmação, clique em **Sim** para continuar.

> [!NOTE]
> Se encerrar o dispositivo virtual, terá de iniciar o dispositivo através do gerenciamento de hipervisor.
