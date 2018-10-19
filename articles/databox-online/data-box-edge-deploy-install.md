---
title: Tutorial sobre como instalar o dispositivo físico do Azure Data Box Edge | Microsoft Docs
description: O segundo tutorial para instalar o Azure Data Box Edge envolve como desembalar, montar em bastidor e ligar o dispositivo físico.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/08/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to install Data Box Edge in datacenter so I can use it to transfer data to Azure.
ms.openlocfilehash: 21ac3de793f5ce559c3a03de2a09f11ccb86b12a
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167363"
---
# <a name="tutorial-install-azure-data-box-edge-preview"></a>Tutorial: Instalar o Azure Data Box Edge (Pré-visualização)

Este tutorial descreve como instalar um dispositivo físico do Data Box Edge. O procedimento de instalação envolve desembalar, montar em bastidor e ligar o dispositivo. 

A instalação pode demorar cerca de 2 horas a concluir.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Desembalar o dispositivo
> * Montar o dispositivo em bastidor
> * Ligar o dispositivo

> [!IMPORTANT]
> O Data Box Edge está em pré-visualização. Reveja os [Termos de serviço do Azure para pré-visualização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de encomendar e implementar esta solução.

## <a name="prerequisites"></a>Pré-requisitos

Seguem-se os pré-requisitos para instalar um dispositivo físico.

### <a name="for-the-data-box-edge-resource"></a>Para o recurso do Data Box Edge

Antes de começar, certifique-se de que:

* Concluiu todos os passos descritos em [Preparar o portal para o Data Box Edge](data-box-edge-deploy-prep.md).
    * Criou o recurso do Data Box Edge para implementar o dispositivo.
    * Gerou a chave de ativação para ativar o dispositivo com o recurso do Data Box Edge.

 
### <a name="for-the-data-box-edge-physical-device"></a>Para o dispositivo físico do Data Box Edge

Antes de implementar um dispositivo:

- Certifique-se de que o dispositivo fica apoiado de forma segura numa superfície de trabalho plana, estável e uniforme (ou semelhante).
- Verifique se o local onde quer configurar o dispositivo tem:
    - Alimentação CA padrão de uma fonte independente, ou
    - uma unidade de distribuição de energia (PDU) em bastidor com uma fonte de alimentação ininterrupta (UPS).
- Certifique-se de que está disponível uma ranhura 1U no bastidor onde quer montar o dispositivo.

### <a name="for-the-network-in-the-datacenter"></a>Para a rede no datacenter

Antes de começar:

- Reveja os requisitos de rede para implementar um Data Box Edge e configure a rede do datacenter de acordo com os requisitos. Para obter mais informações, veja [Requisitos de rede do Data Box Edge](data-box-gateway-system-requirements.md#networking-requirements).
- Certifique-se de que a largura de banda mínima de Internet é 20 Mbps, para permitir o funcionamento otimizado do dispositivo.


## <a name="unpack-the-device"></a>Desembalar o dispositivo

Este dispositivo é enviado numa única caixa. Conclua os passos seguintes para desembalar o dispositivo. 

1. Coloque a caixa numa superfície plana e uniforme.
2. Inspecione a caixa e a espuma de empacotamento para verificar se existem partes esmagadas, cortes, danos causados por água ou quaisquer outros danos óbvios. Se a caixa ou embalagem estiver gravemente danificada, não abra a caixa. Contacte o Suporte da Microsoft para ajudar a avaliar se o dispositivo está funcional.
3. Desembale a caixa. Depois de desembalar a caixa, certifique-se de que tem:
    - Um dispositivo Edge de bastidor único
    - Dois cabos de alimentação
    - Um kit de montagem em bastidor deslizante sem ferramentas (duas calhas laterais e hardware de montagem incluídos)
4. Se não tiver recebido qualquer um dos itens listados acima, contacte o Suporte do Data Box Edge. O passo seguinte é montar o dispositivo em bastidor.


## <a name="rack-the-device"></a>Montar o dispositivo em bastidor

O dispositivo tem de ser instalado num bastidor de 19 polegadas padrão. Siga o procedimento seguinte para montar o dispositivo num bastidor de 19 polegadas padrão com suportes frontais e posteriores.

> [!IMPORTANT]
> Os dispositivos Data Box Edge têm de ser montados em bastidor para um funcionamento correto.


1. Solte o botão frontal para desbloquear a calha interior da montagem deslizante. Solte o bloqueio de detenção e pressione a calha central para dentro para a retrair. As calhas interiores e exteriores devem estar agora separadas.

    ![Instalar as calhas de montagem em bastidor](./media/data-box-edge-deploy-install/rack-mount-rail-1.png)

2. Agora, instale as calhas exteriores nos membros verticais do bastidor. Para ajudar na orientação, as partes deslizantes das calhas têm uma marca a indicar Front (frente) e essa extremidade é afixada em direção à parte frontal do bastidor. 
    
    1. Localize os pinos na parte frontal e posterior da montagem das calhas. Expanda a calha para encaixar entre os suportes do bastidor. Primeiro, fixe a calha exterior na parte posterior do bastidor. Ajuste o suporte de montagem posterior de forma a posicioná-lo no interior dos orifícios de montagem em bastidor posteriores.   
    2. Pressione e segure o gatilho no suporte posterior para expor os ganchos metálicos. Alinhe e insira nos orifícios de montagem e, em seguida, solte o gatilho.
    3. Alinhe o suporte frontal com o orifício de montagem.
    4. O suporte frontal deve estar agora fixado ao bastidor. Opcionalmente, é possível utilizar parafusos M5 X 10L para proteger as calhas com suportes, se necessário. 

    ![Instalar as calhas de montagem em bastidor](./media/data-box-edge-deploy-install/rack-mount-rail-2.png)

3. Para fixar a calhar interior na estrutura, certifique-se de que as aberturas dos orifícios na calha interior estão alinhadas com os pinos de localização na parte lateral da estrutura. Certifique-se de que as cabeças dos pinos de localização da estrutura estão salientes nas aberturas dos orifícios na calha interior. Puxe a calha para a frente da estrutura até ouvir um clique e a calha encaixar. Repita com a outra calha interior. Pressione a estrutura com a calha interior na parte deslizante para concluir a instalação no bastidor.

    ![Instalar as calhas de montagem em bastidor](./media/data-box-edge-deploy-install/rack-mount-rail-3.png)

## <a name="cable-the-device"></a>Ligar o dispositivo

Os procedimentos seguintes explicam como ligar o dispositivo Edge em termos de alimentação e rede.

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar a cablagem do dispositivo, é necessário:

- Desembalar e montar o dispositivo físico do Edge em bastidor.
- Dois cabos de alimentação. 
- Dois cabos de rede RJ-45 de 1 GbE e quatro cabos SFP + cobre de 25 GbE.
- Acesso a duas Unidades de Distribuição de Energia (recomendado).

O dispositivo Edge tem 8 SSDs NVMe. O painel frontal também tem LEDs de estado e botões de energia. O dispositivo inclui Unidades de Alimentação (PSUs) redundantes na parte posterior. O dispositivo tem seis interfaces de rede: duas interfaces de 1 Gbps e quatro interfaces de 25 Gbps. O dispositivo tem um controlador de gestão da placa base (BMC). Identifique as várias portas no painel posterior do dispositivo.
 
  ![Painel posterior de um dispositivo ligado por cabo](./media/data-box-edge-deploy-install/backplane-cabled.png)
 
Siga os passos seguintes para ligar o dispositivo em termos de alimentação e rede.

1. Ligue os cabos de alimentação a cada uma das PSUs no bastidor. Para garantir uma elevada disponibilidade, instale e ligue as PSUs a diferentes fontes de alimentação.
2. Ligue os cabos de alimentação às unidades de distribuição de energia (PDUs) em bastidor. Certifique-se de que as duas PSUs utilizam fontes de alimentação separadas.
3. Ligue a PORTA 1 da interface de rede de 1 GbE ao computador utilizado para configurar o dispositivo físico. A PORTA 1 é a interface de gestão dedicada.
4. Ligue a PORTA 2 da interface de rede de 1 GbE através dos cabos de rede RJ-45 à rede do datacenter/Internet. 
5. Ligue as PORTAS 3, 4, 5 e 6 das quatro interfaces de rede de 25 GbE através de cabos de SFP + cobre à rede do datacenter/Internet. 

> [!NOTE]
> - É necessário que pelo menos uma interface de rede de dados (PORTAS 2, 3, 4, 5 ou 6) esteja ligada à Internet (conectividade ao Azure). 
> - Recomendamos que utilize uma interface de rede de 25 GbE, como as PORTAS 3, 4, 5 ou 6, para enviar dados para o Azure. 
> - O dispositivo Edge deve estar ligado à rede do datacenter para que possa ingerir dados dos servidores de origem de dados.  


## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a conhecer tópicos do Data Box Edge, como:

> [!div class="checklist"]
> * Desembalar o dispositivo
> * Montar o dispositivo em bastidor
> * Ligar o dispositivo

Avance para o próximo tutorial para aprender a ligar, configurar e ativar o seu dispositivo.

> [!div class="nextstepaction"]
> [Ligar e configurar o Data Box Edge](./data-box-edge-deploy-connect-setup-activate.md)


