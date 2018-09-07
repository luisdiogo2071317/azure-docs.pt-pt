---
title: incluir ficheiro
description: incluir ficheiro
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 09/06/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: 2ca4916d48da6fe8a2c061056a1ea0fed9a78bb6
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/06/2018
ms.locfileid: "44058372"
---
1. Execute o ficheiro de instalação Configuração Unificada.
2. Na **antes de começar**, selecione **instalar o servidor de configuração e o servidor de processos**.

    ![Antes de começar](./media/site-recovery-add-configuration-server/combined-wiz1.png)

3. Em **Licença de Software de Terceiros**, clique em **Aceito**, para transferir e instalar o MySQL.

    ![Software de terceiros](./media/site-recovery-add-configuration-server/combined-wiz2.png)
4. Em **Registo**, selecione a chave de registo que transferiu a partir do cofre.

    ![Registo](./media/site-recovery-add-configuration-server/combined-wiz3.png)
5. Em **Definições da Internet**, especifique a forma como o Fornecedor em execução no servidor estabelece ligação ao Azure Site Recovery através da Internet. Certifique-se de que ter permitido os URLs necessários.

    - Se pretender estabelecer ligação com o proxy que está atualmente configurado no computador, selecione **ligar ao Azure Site Recovery com um servidor proxy**.
    - Se pretender que o fornecedor se ligue diretamente, selecione **ligar diretamente ao Azure Site Recovery sem um servidor proxy**.
    - Se o seu proxy existente requer autenticação ou se pretender utilizar um proxy personalizado para a ligação de fornecedor, selecione **ligar com definições de proxy personalizado**e especifique o endereço, porta e credenciais.
     ![Firewall](./media/site-recovery-add-configuration-server/combined-wiz4.png)
6. Em **Verificação de Pré-requisitos**, a Configuração executa uma verificação para se certificar de que a instalação pode ser executada. Se for apresentado um aviso sobre a **Verificação de sincronização de hora global**, certifique-se de que a hora no relógio do sistema (definições de **Data e Hora**) é a mesma que o fuso horário.

    ![Pré-requisitos](./media/site-recovery-add-configuration-server/combined-wiz5.png)
7. Em **Configuração do MySQL**, crie as credenciais para iniciar sessão na instância de servidor do MySQL que está instalada.

    ![MySQL](./media/site-recovery-add-configuration-server/combined-wiz6.png)
8. Na **detalhes do ambiente**, selecione não se está a replicar VMs do Azure Stack ou servidores físicos. 
9. Em **Localização de Instalação**, selecione onde pretende instalar os binários e armazenar a cache. A unidade que selecionar tem de ter, pelo menos, 5 GB de espaço disponível no disco, mas recomenda-se uma unidade de cache com, pelo menos, 600 GB de espaço livre.

    ![Localização de instalação](./media/site-recovery-add-configuration-server/combined-wiz8.png)
10. Em **Seleção de Rede**, especifique o serviço de escuta (placa de rede e porta SSL) no qual o servidor de configuração envia e recebe dados de replicação. A porta 9443 é a porta predefinida utilizada para envio e receção de tráfego de replicação, mas pode modificar este número de porta para melhor corresponda às necessidades do seu ambiente. Além da porta 9443, podemos também abrir a porta 443, que é utilizada por um servidor Web para orquestrar operações de replicação. Não utilize a porta 443 para enviar ou receber tráfego de replicação.

    ![Seleção de rede](./media/site-recovery-add-configuration-server/combined-wiz9.png)


11. Em **Resumo**, consulte as informações e clique em **Instalar**. Quando a instalação estiver concluída, é gerada uma frase de acesso. Irá precisar dela, quando ativar a replicação, por isso, copie-a e mantenha-a numa localização segura.

    ![Resumo](./media/site-recovery-add-configuration-server/combined-wiz10.png)

Após a conclusão de registo, o servidor é apresentado no painel **Definições** > **Servidores** no cofre.
