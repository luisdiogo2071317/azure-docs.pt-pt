---
title: Como manter um endereço IP virtual constante para um serviço cloud do Azure | Documentos da Microsoft
description: Saiba como garantir que o endereço IP virtual (VIP) do seu serviço cloud do Azure não são alterados.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 4a58e2c6-7a79-4051-8a2c-99182ff8b881
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/21/2017
ms.author: ghogen
ms.openlocfilehash: 2f82663f6b53c6d4e32b8d655dcbd67a321d91ed
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42062071"
---
# <a name="retain-a-constant-virtual-ip-address-for-an-azure-cloud-service"></a>Manter um endereço IP virtual constante para um serviço cloud do Azure
Quando atualizar um serviço cloud que está alojado no Azure, poderá ter de se certificar de que o endereço IP virtual (VIP) do serviço não é alterado. Muitos serviços de gestão de domínio utilizam o sistema de nomes de domínio (DNS) para registar nomes de domínio. DNS só funciona se o VIP permanece o mesmo. Pode utilizar o **Assistente de publicação** nas ferramentas do Azure para se certificar de que o VIP do seu serviço cloud não é alterado quando efetua a atualização. Para obter mais informações sobre como utilizar a gestão de domínios DNS para serviços em nuvem, consulte [configurar um nome de domínio personalizado para um serviço cloud do Azure](cloud-services/cloud-services-custom-domain-name-portal.md).

## <a name="publish-a-cloud-service-without-changing-its-vip"></a>Publicar um serviço em nuvem sem alterar o respetivo VIP
O VIP de um serviço cloud é alocado quando primeiro implantá-lo para o Azure num ambiente específico, como o ambiente de produção. O VIP é alterado apenas se excluir explicitamente a implementação ou a implementação é implicitamente eliminada pelo processo de atualização de implementação. Para manter o VIP, não tem de eliminar a implementação, e deve certificar-se de que Visual Studio não elimina a implementação automaticamente. 

Pode especificar definições de implementação no **Assistente de publicação**, que suporta várias opções de implementação. Pode especificar uma nova implementação ou uma implementação de atualização, que pode ser incremental ou simultâneas. Os dois tipos de implementação de atualização de mantém o VIP. Para obter definições desses diferentes tipos de implementação, consulte [publicar Assistente da aplicação Azure](vs-azure-tools-publish-azure-application-wizard.md). Além disso, pode controlar se a implementação anterior de um serviço cloud é eliminada se ocorrer um erro. Se não definir essa opção corretamente, o VIP poderá ser alterado inesperadamente.

## <a name="update-a-cloud-service-without-changing-its-vip"></a>Atualizar um serviço em nuvem sem alterar o respetivo VIP
1. Crie ou abra um projeto do serviço cloud do Azure no Visual Studio. 

2. Na **Explorador de soluções**, com o botão direito no projeto. No menu de atalho, selecione **publicar**.

    ![Menu Publicar](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/solution-explorer-publish-menu.png)

3. Na **publicar a aplicação do Azure** diálogo caixa, selecione a subscrição do Azure para o qual pretende implementar. Iniciar sessão, se necessário e selecione **seguinte**.

    ![Publicar início de sessão de aplicação de Azure na página](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-signin.png)

4. Na **definições comuns** separador, certifique-se de que o nome da cloud de serviço para que estiver implantando, o **ambiente**, o **configuração de compilação**e a **Configuração do serviço** estão corretos.

    ![Publicar o separador de definições comuns de aplicação do Azure](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-common-settings.png)

5. Sobre o **definições avançadas** separador, certifique-se de que o **etiqueta de implementação** e o **conta de armazenamento** estão corretos. Certifique-se de que o **eliminação da implementação em caso de falha** caixa de verificação está desmarcada e certifique-se de que o **atualização da implementação** caixa de verificação está selecionada. Ao desmarcar a **eliminação da implementação em caso de falha** caixa de verificação, certifique-se de que o VIP não perdido se ocorrer um erro durante a implementação. Ao selecionar o **atualização da implementação** caixa de verificação, garante que a implementação não é eliminada e o VIP não é perdido quando voltar a publicar seu aplicativo. 

    ![Publicar o separador de definições avançadas da aplicação do Azure](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-advanced-settings.png)

6. Para especificar como pretende que as funções para ser atualizado, selecione **configurações** junto a **atualização da implementação**. Selecione **atualização Incremental** ou **atualização em simultâneo**e selecione **OK**. Escolher **atualização Incremental** atualizar cada instância do seu aplicativo, um após o outro, para que o aplicativo estará sempre disponível. Escolher **atualização em simultâneo** para atualizar todas as instâncias da sua aplicação ao mesmo tempo. A atualização simultânea é mais rápido, mas o seu serviço pode não estar disponível durante o processo de atualização. Quando tiver terminado, selecione **seguinte**.

    ![Publicar a página de definições de implementação de aplicação do Azure](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-deployment-update-settings.png)

7. Na **publicar a aplicação do Azure** caixa de diálogo, selecione **próxima** até que o **resumo** é apresentada a página. Verificar as definições e, em seguida, selecione **publicar**.
   
    ![Publicar a página de resumo de aplicações do Azure](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-summary.png)

## <a name="next-steps"></a>Passos Seguintes
- [Com o Visual Studio publicar Assistente da aplicação do Azure](vs-azure-tools-publish-azure-application-wizard.md)

