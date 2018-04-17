---
title: Gerir inscrições de dispositivos com o portal do Azure | Microsoft Docs
description: Como gerir inscrições de dispositivos para o seu serviço DPS no Portal do Azure
services: iot-dps
keywords: ''
author: dsk-2015
ms.author: dkshir
ms.date: 04/05/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 55486d9a37968351f5313c708e9ef26e5b89063c
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="how-to-manage-device-enrollments-with-azure-portal"></a>Como gerir inscrições de dispositivos com o Portal do Azure

A *inscrição de dispositivos* cria um registo de um único dispositivo ou um grupo de dispositivos que poderão, a determinada altura registar com o serviço de aprovisionamento de dispositivos do Azure IoT Hub. O registo de inscrição contém a configuração pretendida inicial para os dispositivos como parte do que a inscrição, incluindo o hub IoT pretendido. Este artigo mostra como gerir inscrições de dispositivos para o seu serviço de aprovisionamento.


## <a name="create-a-device-enrollment"></a>Criar uma inscrição de dispositivos

Existem duas formas de poder inscrever os seus dispositivos com o serviço de aprovisionamento:

* Um **grupo inscrição** é uma entrada para um grupo de dispositivos que partilham um mecanismo de atestado comuns dos certificados x. 509, assinados pelo mesmo certificado de assinatura, que pode ser o [certificado de raiz](https://docs.microsoft.com/azure/iot-dps/concepts-security#root-certificate) ou o [intermediária](https://docs.microsoft.com/azure/iot-dps/concepts-security#intermediate-certificate), utilizada para produzir o certificado de dispositivo no dispositivo físico. Recomendamos que utilize um grupo de inscrição para um grande número de dispositivos que partilham uma configuração desejada inicial ou para dispositivos que todos os mesmo inquilino. Tenha em atenção que só podem inscrever dispositivos que utilizam o mecanismo de atestado de x. 509 como *grupos inscrição*. 

    Pode criar um grupo de inscrição no portal para um grupo de dispositivos utilizando os seguintes passos:

    1. Inicie sessão no portal do Azure e clique em **todos os recursos** no menu esquerdo.  
    2. Clique no serviço de aprovisionamento de dispositivos de mensagens em fila que pretende inscrever o seu dispositivo a partir da lista de recursos.  
    3. No seu serviço de aprovisionamento:  
       a. Clique em **gerir inscrições**, em seguida, selecione o **inscrição grupos** separador.  
       b. Clique no botão **+ Adicionar**, na parte superior.  
       c. Quando for apresentado o painel "Adicionar grupo de inscrição", introduza as informações para a entrada da lista de inscrição.  **Nome do grupo** é necessária. Também selecionar "AC ou intermédio" para **tipo de certificado**e carregue a raiz **certificado principal** para o grupo de dispositivos.  
       d. Clique em **Guardar**. Na criação com êxito do seu grupo de inscrição, deverá ver o nome do grupo aparecer sob o **inscrição grupos** separador.  

       [![Grupo de inscrição no portal](./media/how-to-manage-enrollments/group-enrollment.png)] (. / media/how-to-manage-enrollments/group-enrollment.png#lightbox)
    

* Um **inscrição individuais** é uma entrada para um único dispositivo pode registar. As inscrições individuais podem utilizar qualquer um dos x509 certificados ou SAS tokens (a partir de um TPM físico ou virtual) como mecanismos de atestado. Recomendamos que utilize inscrições individuais para os dispositivos que requerem configurações iniciais exclusivas, ou para dispositivos que só podem utilizar os tokens SAS através do TPM ou virtual TPM como o mecanismo de atestado. As inscrições individuais podem ter o ID de dispositivo do hub IoT pretendido especificado.

    Pode criar um registo individual no portal utilizando os seguintes passos:

    1. Inicie sessão no portal do Azure e clique em **todos os recursos** no menu esquerdo.
    2. Clique no serviço de aprovisionamento de dispositivos de mensagens em fila que pretende inscrever o seu dispositivo a partir da lista de recursos.
    3. No seu serviço de aprovisionamento:  
       a. Clique em **gerir inscrições**, em seguida, selecione o **inscrições individuais** separador.  
       b. Clique no botão **+ Adicionar**, na parte superior.   
       c. Quando for apresentado o painel "Adicionar inscrição", introduza as informações para a entrada da lista de inscrição. Selecione primeiro o atestado **mecanismo** para o dispositivo (x. 509 ou TPM). Atestado de x. 509 requer a carregar a folha **certificado principal** para o dispositivo. TPM requer que introduza o **atestado de chave** e **ID de registo** para o dispositivo.  
       d. Clique em **Guardar**. Na criação com êxito do seu grupo de inscrição, deverá ver o seu dispositivo são apresentados no **inscrições individuais** separador.  

       [![Inscrição individuais no portal](./media/how-to-manage-enrollments/individual-enrollment.png)](./media/how-to-manage-enrollments/individual-enrollment.png#lightbox)

## <a name="update-an-enrollment-entry"></a>Atualizar uma entrada de inscrição
Pode atualizar uma entrada de registo existente no portal utilizando os seguintes passos:

1. Abra o seu serviço de aprovisionamento de dispositivos no portal do Azure e clique em **gerir inscrições**. 
2. Navegue para a entrada de inscrição que pretende modificar. Clique na entrada, o que abre uma informações de resumo sobre a inscrição do dispositivo. 
3. Nesta página, pode modificar os itens que não seja o tipo de segurança e credenciais, tais como o IoT hub do dispositivo devem ser associadas a, bem como o ID de dispositivo. Também pode modificar o estado de duplo de dispositivos inicial. 
4. Depois de concluída, clique em **guardar** para atualizar a inscrição do dispositivo. 

    ![Inscrição de atualização no portal](./media/how-to-manage-enrollments/update-enrollment.png)

## <a name="remove-a-device-enrollment"></a>Remover uma inscrição de dispositivos
Em casos onde os dispositivos não têm de ser aprovisionada para qualquer hub IoT, pode remover a entrada de inscrição relacionado no portal utilizando os seguintes passos:

1. Abra o seu serviço de aprovisionamento de dispositivos no portal do Azure e clique em **gerir inscrições**. 
2. Navegue para e selecione a entrada de inscrição que pretende remover. 
3. Clique em de **eliminar** botão na parte superior e, em seguida, selecione **Sim** quando lhe for pedido para confirmar. 
5. Assim que a ação for concluída, verá a entrada removida da lista de inscrições de dispositivos. 
 
    ![Remova a inscrição no portal](./media/how-to-manage-enrollments/remove-enrollment.png)


