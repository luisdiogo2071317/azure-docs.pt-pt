---
title: Descrição geral do Microsoft Azure Data Box Gateway | Microsoft Docs
description: Descreve o Azure Data Box Gateway, uma solução de armazenamento de aplicações virtuais que permite transferir dados para o Azure
services: databox
author: alkohli
ms.service: databox
ms.topic: overview
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: 4f1ab6d955c81ce6f7b141eef42341f43bb379f6
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/12/2018
ms.locfileid: "49165322"
---
# <a name="what-is-azure-data-box-gateway-preview"></a>O que é o Azure Data Box Gateway (pré-visualização)? 

O Azure Data Box Gateway é uma solução de armazenamento que permite enviar facilmente dados para o Azure. Este artigo fornece uma descrição geral da solução Azure Data Box Gateway, os benefícios, as principais funções e os cenários nos quais pode implementar este dispositivo. 

O Data Box Gateway é um dispositivo virtual com base numa máquina virtual aprovisionada no seu ambiente virtualizado ou hipervisor. O dispositivo virtual reside no local e escreve dados no mesmo com os protocolos NFS e SMB. O dispositivo transfere os dados para o blob de blocos do Azure, o blob de páginas ou Ficheiros do Azure. 

> [!IMPORTANT]
> O Data Box Gateway está em Pré-visualização. Reveja os [termos de utilização da pré-visualização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de implementar esta solução.

## <a name="use-cases"></a>Casos de utilização

Pode tirar-se partido do Data Box Gateway para transferir dados para a cloud, como arquivo na cloud, recuperação após desastre ou, se for necessário, para processar os dados à escala da cloud. Eis os vários cenários em que o Data Box Gateway pode ser utilizado para transferência de dados.

- **Arquivo na cloud** - copie centenas de TBs de dados para o armazenamento do Azure com o Data Box Gateway de forma segura e eficiente. Os dados podem ser ingeridos uma vez ou de forma contínua para cenários de arquivo.

- **Agregação de dados** - agregue dados de várias origens numa única localização no Armazenamento do Azure para processamento e análise de dados.  

- **Integração com cargas de trabalho no local** - integre com cargas de trabalho no local, como a cópia de segurança e o restauro, que utilizam o armazenamento na cloud e precisam de acesso local para os ficheiros mais utilizados. 

## <a name="benefits"></a>Benefícios

O Data Box Gateway possui os benefícios seguintes:

- **Transferência de dados fácil** - facilita mover dados de e para o armazenamento do Azure, sendo tão fácil como trabalhar numa partilha de rede local.  
- **Elevado desempenho** - elimina todos os problemas de transporte de dados de rede, com transferências de elevado desempenho de e para o Azure. 
- **Acesso rápido** - coloca em cache os ficheiros mais recentes para um acesso rápido aos ficheiros no local.  
- **Utilização de largura de banda limitada** - os dados podem ser escritos no Azure, mesmo quando a rede é limitada, para limitar a utilização durante o horário de pico.  

## <a name="key-capabilities"></a>Principais capacidades

O Data Box Gateway possui as seguintes funções:

|Capacidade |Descrição  |
|---------|---------|
|Velocidade     | Transferência de dados e largura de banda totalmente automatizadas e altamente otimizadas.|
|Protocolos suportados     | Suporte para protocolos SMB e NFS standard de ingestão de dados. <br> Para obter mais informações sobre as versões suportadas, aceda a [Requisitos de sistema do Data Box Gateway](data-box-gateway-system-requirements.md).|
|Acesso a dados     | Direcione o acesso aos dados de Blobs de Armazenamento do Azure e Ficheiros do Azure através de APIs da cloud para processamento adicional de dados na cloud.|
|Acesso rápido     | Cache local no dispositivo para um acesso rápido aos ficheiros utilizados mais recentemente.|
|Carregamento offline     | O modo desligado suporta cenários de carregamento offline.|
|Atualização de dados     | Capacidade de atualizar os ficheiros locais com a versão mais recente da cloud.|
|Encriptação    | Suporte de BitLocker para encriptar localmente os dados e proteger a transferência de dados para a cloud através de *https*       |
|Resiliência     | Resiliência de rede incorporada        |


## <a name="specifications"></a>Especificações

O dispositivo virtual do Data Box Gateway tem as seguintes especificações:

| Especificações                                          | Descrição              |
|---------------------------------------------------------|--------------------------|
| Processadores virtuais (núcleos)   | Mínimo de 4 |            
| Memória  | Mínimo de 8 GB|
| Disponibilidade|Nó único|
| Discos| Disco do SO: 250 GB <br> Disco de dados: mínimo de 2 TB, com aprovisionamento dinâmico e tem de ser apoiado por SSDs|
| Interfaces de rede|1 ou mais interfaces de rede virtual|
| Protocolos de partilha de ficheiros nativos|SMB e NFS  |
| Segurança| Autenticação para desbloquear o acesso ao dispositivo e aos dados <br> Dados em trânsito encriptados com a encriptação AES de 256 bits|
| Gestão| IU da Web local - configuração inicial, diagnóstico e gestão de energia do dispositivos <br> Portal do Azure - gestão diária de dispositivos do Data Box Gateway       |


## <a name="components"></a>Componentes

A solução Data Box Gateway inclui o recurso Data Box Gateway, o dispositivo virtual Data Box Gateway e uma IU da Web local.

* **Dispositivo virtual Data Box Gateway** - um dispositivo com base numa máquina virtual aprovisionada no seu ambiente virtualizado ou hipervisor que permite enviar dados para o Azure. 
    
* **Recurso Data Box Gateway** – um recurso no portal do Azure que permite gerir um dispositivo Data Box Gateway a partir de uma interface Web à qual pode aceder a partir de localizações geográficas diferentes. Utilize o recurso Data Box Gateway para criar e gerir recursos, ver e gerir dispositivos e alertas, e gerir partilhas.  

    <!--![The Data Box Gateway service in Azure portal](media/data-box-overview/data-box-Gateway-service1.png)-->

    <!--For more information, go to [Use the Data Box Gateway service to administer your Data Box Gateway device](data-box-gateway-portal-ui-admin.md).-->

* **IU da Web local do Data Box** – utilize a IU da Web local para executar diagnósticos, encerrar e reiniciar o dispositivo Data Box Gateway, ver registos de cópias e entrar em contacto com o Suporte da Microsoft para fazer um pedido de serviço.

    <!--![The Data Box Gateway local web UI](media/data-box-gateway-overview/data-box-gateway-local-web-ui.png)-->

    <!-- For information about using the web-based UI, go to [Use the web-based UI to administer your Data Box](data-box-gateway-portal-ui-admin.md).-->


## <a name="region-availability"></a>Disponibilidade de região

O dispositivo físico Data Box Edge, o recurso do Azure e a conta de armazenamento de destino para a qual transfere dados não têm de estar todos na mesma região.

- **Disponibilidade de recursos** - nesta versão, o recurso Data Box Edge está disponível nas seguintes regiões:
    - **Estados Unidos** - E.U.A. Oeste2 e E.U.A. Leste
    - **União Europeia** - Europa Ocidental
    - **Ásia Oriental** - Sudoeste Asiático

- **Contas de Armazenamento de Destino** - as contas de armazenamento que armazenam os dados estão disponíveis em todas as regiões do Azure. 

    As regiões nas quais as contas de armazenamento armazenam os dados do Data Box devem estar localizadas próximo de onde estiver o dispositivo, para um desempenho ideal. Uma conta de armazenamento localizada longe do dispositivo resulta em latências longas e desempenho mais lento. 


## <a name="next-steps"></a>Passos seguintes

- Reveja os [requisitos de sistema do Data Box Gateway](data-box-gateway-system-requirements.md).
- Compreenda os [Limites do Data Box Gateway](data-box-gateway-limits.md).
- Implemente o [Azure Data Box Gateway](data-box-gateway-deploy-prep.md) no portal do Azure.




