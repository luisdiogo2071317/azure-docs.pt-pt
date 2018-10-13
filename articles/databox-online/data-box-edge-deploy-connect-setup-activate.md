---
title: Ligue-se para configurar e ativar o Edge de caixa de dados do Azure no portal do Azure | Documentos da Microsoft
description: Terceiro tutorial para implementar dados de caixa de borda instrui a ligar, configurar e ativar o dispositivo físico.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 10/08/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to connect and activate Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 3d6742bec54e612b5dca6d9ef6c4f67a33929448
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166581"
---
# <a name="tutorial-connect-set-up-activate-azure-data-box-edge-preview"></a>Tutorial: Ligar, configurar, ative o Edge de caixa de dados do Azure (pré-visualização) 

Este tutorial descreve como ligar a configurar e ativar o seu dispositivo do Edge de caixa de dados com a IU web local. 

O processo de instalação e ativação pode demorar cerca de 20 minutos a concluir. 

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Ligar ao dispositivo físico
> * Configurar e ativar o dispositivo físico

> [!IMPORTANT]
> Edge de caixa de dados está em pré-visualização. Reveja os [Termos de serviço do Azure para pré-visualização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de encomendar e implementar esta solução. 


## <a name="prerequisites"></a>Pré-requisitos

Antes de configurar e configurar o seu limite de caixa de dados, certifique-se de que:

* Instalou o dispositivo físico, conforme detalhado no [Edge de caixa de dados instalar](data-box-edge-deploy-install.md).
* Tem a chave de ativação do serviço do Edge de caixa de dados que criou para gerir o dispositivo de limite de caixa de dados. Para obter mais informações, aceda a [preparar a implementação de borda de caixa de dados do Azure](data-box-edge-deploy-prep.md).

## <a name="connect-to-the-local-web-ui-setup"></a>Ligue-se para a configuração de interface do Usuário da local web 

1. Configure o adaptador Ethernet no computador que está a utilizar para ligar ao dispositivo de limite com um endereço IP estático de 192.168.100.5 e sub-rede 255.255.255.0.
2. Liga o computador a 1 de porta no seu dispositivo. 
3. Abra uma janela do browser e o acesso da interface do Usuário do dispositivo no local da web https://192.168.100.10. Esta ação pode demorar alguns minutos depois que tiver ativado no dispositivo. 
4. Verá um erro ou um aviso que indica que existe um problema com o certificado de segurança do Web site. Clique em **continuar para esta página da Web**. (Estes passos podem ser diferentes com base no navegador utilizado.)
   
    ![](./media/data-box-edge-deploy-connect-setup-activate/image2.png)

2. Inicie sessão para a web da interface do Usuário do seu dispositivo. A palavra-passe predefinido é *Password1*. 
   
    ![](./media/data-box-edge-deploy-connect-setup-activate/image3.png)

3. Lhe for pedido para alterar a palavra-passe de administrador do dispositivo. Escreva uma nova palavra-passe que contenha entre 8 e 16 carateres. A palavra-passe tem de conter 3 dos seguintes carateres: carateres em maiúsculas, minúsculas, numéricos e especiais.

Agora, está no **Dashboard** do seu dispositivo.

## <a name="set-up-and-activate-the-physical-device"></a>Configurar e ativar o dispositivo físico
 
1. No dashboard, pode aceder a várias definições necessárias para configurar e registar o dispositivo físico com o serviço de borda de caixa de dados. O **nome do dispositivo**, **as definições de rede**, **definições de proxy Web**, e **definições da hora** são opcionais. As definições necessárias apenas são **definições da Cloud**.
   
    ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-1.png)

2. Na **nome do dispositivo** página, configure um nome amigável para o seu dispositivo. O nome amigável pode ter 1 a 15 carateres e pode conter letras, números e hífenes.

    ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-2.png)

3. (Opcionalmente) configurar seus **as definições de rede**. No seu dispositivo físico, verá seis interfaces de rede. PORTA 1 e 2 de porta são interfaces de rede de 1 Gbps. PORTA 3, 4 de porta, porta 5 e 6 de porta são todas as interfaces de rede de 25 Gbps. PORTA 1 é automaticamente configurada como porta de gestão só enquanto porta 2 a 6 de porta são todas as portas de dados. O **as definições de rede** página conforme mostrado abaixo.
    
    ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-3.png)
   
    Quando configurar as definições de rede, tenha em mente:

    - Se o DHCP estiver ativado no seu ambiente, as interfaces de rede são configuradas automaticamente. Um endereço IP, sub-rede, gateway e DNS é atribuído automaticamente.
    - Se o DHCP não está ativado, pode atribuir IPs estáticos, se necessário.
    - Pode configurar a sua interface de rede como IPv4.
   
4. (Opcionalmente)-configure o servidor de proxy da web. Apesar de configuração do proxy web é opcional, se utilizar um proxy de web, pode apenas configurá-lo aqui.
   
   ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-4.png)
   
   Na **Web proxy** página:
   
   1. Forneça o **URL de proxy Web** neste formato: `http://host-IP address or FDQN:Port number`. Não são suportados HTTPS URLs.
   2. Especifique **autenticação** como **básica** ou **None**.
   3. Se utilizar a autenticação, também precisará fornecer uma **nome de utilizador** e **palavra-passe**.
   4. Clique em **aplicar** para validar e aplicar as definições de proxy da web configurado.

5. (Opcionalmente) configure as definições de hora para o seu dispositivo, como fuso horário e os servidores NTP primário e secundários. Os servidores NTP são necessários porque o dispositivo tem de sincronizar tempo para que ele pode autenticar com o seu fornecedor de serviços cloud.
    
    ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-5.png)
    
    Na **definições da hora** página:
    
    1. Na lista pendente, selecione o **fuso horário** com base na localização geográfica na qual o dispositivo está a ser implementado. O fuso horário predefinido para o seu dispositivo é PST. O dispositivo utiliza este fuso horário para todas as operações agendadas.
    2. Especifique um **servidor NTP primário** para o seu dispositivo ou aceite o valor predefinido de time.windows.com. Verifique se a sua rede permite que o tráfego NTP passe do datacenter para a Internet.
    3. Opcionalmente, especifique um **servidor NTP secundário** para o seu dispositivo.
    4. Clique em **aplicar** para validar e aplicar as definições de tempo configurado.

6. Na **definições da Cloud** página, ative o seu dispositivo com o serviço de borda de caixa de dados no portal do Azure.
    
    1. Introduza o **chave de ativação** obtido [obter a chave de ativação](data-box-edge-deploy-prep.md#get-the-activation-key) para o Edge de caixa de dados.

    2. Clique em **Aplicar**. 
       
         ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-6.png)
    
    3. Depois do dispositivo é ativado com êxito, é apresentada com opções de modo de conectividade. Estas definições são configuradas, se precisar de trabalhar com o dispositivo no modo parcialmente desconectado ou desligado. 

        ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-7.png)    

A configuração do dispositivo foi concluída. Agora pode adicionar partilhas no seu dispositivo.


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu sobre tópicos de borda de caixa de dados, tais como:

> [!div class="checklist"]
> * Ligar ao dispositivo físico
> * Configurar e ativar o dispositivo físico


Avance para o próximo tutorial para saber como transferir dados com dados de caixa de extremidade.

> [!div class="nextstepaction"]
> [Transferir dados com dados de caixa de borda](./data-box-edge-deploy-add-shares.md).