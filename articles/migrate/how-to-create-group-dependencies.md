---
title: "Refinar um grupo de avaliação com mapeamento de dependência de grupo no Azure migrar | Microsoft Docs"
description: "Descreve como refinar uma avaliação utilizando o mapeamento de grupo de dependência no serviço Azure migrar."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/22/2017
ms.author: raynew
ms.openlocfilehash: 3b10765894501791004e3a9221363f196cc0c91d
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2018
---
# <a name="refine-a-group-using-group-dependency-mapping"></a>Refinar um grupo utilizando o mapeamento de grupo de dependência

Este artigo descreve como refinar um grupo ao visualizar as dependências de todas as máquinas no grupo. Normalmente, utiliza este método quando pretender adicione refinar associação para um grupo existente, a verificação entre dependências de grupo, antes de executar uma avaliação. Limitar um grupo através de visualização de dependência pode ajudará a planear efetivamente a migração para Azure.You pode detetar todos os sistemas interdependentes que precisam de migrar em conjunto. Ajuda a garantir que nada for deixado e surprise não ocorrerem quando estiver a migrar para o Azure. 


> [!NOTE]
> Grupos para o qual pretende visualizar as dependências não devem conter mais de 10 máquinas. Se tiver mais de 10 máquinas do grupo, recomendamos que dividi-lo para grupos mais pequenos para tirar partido da funcionalidade de visualização de dependência.


# <a name="prepare-the-group-for-dependency-visualization"></a>Preparar o grupo para visualização de dependência
Para ver as dependências de um grupo, tem de transferir e instalar agentes em cada máquina no local que faz parte do grupo. Além disso, se tiver máquinas sem conectividade internet, terá de transferir e instalar [OMS gateway](../log-analytics/log-analytics-oms-gateway.md) nos mesmos.

### <a name="download-and-install-the-vm-agents"></a>Transferir e instalar os agentes VM
1. No **descrição geral**, clique em **gerir** > **grupos**, vá para o grupo necessário.
2. Na lista de máquinas, no **agente de dependência** coluna, clique em **requer a instalação** para ver instruções sobre como transferir e instalar os agentes.
3. No **dependências** página, transfira e instale o Microsoft Monitoring Agent (MMA) e o agente de dependência em cada VM que faz parte do grupo.
4. Copie o ID da área de trabalho e a chave. Terá de estes quando instalar o MMA nas máquinas no local.

### <a name="install-the-mma"></a>Instalar o MMA

Para instalar o agente num computador Windows:

1. Faça duplo clique o agente transferido.
2. Na página **Bem-vindo**, clique em **Seguinte**. No **termos de licenciamento** página, clique em **concordo** para aceitar a licença.
3. No **pasta de destino**, manter ou modificar a pasta de instalação predefinida > **seguinte**. 
4. No **opções de configuração do agente**, selecione **análise de registos do Azure (OMS)** > **seguinte**. 
5. Clique em **adicionar** para adicionar uma nova área de trabalho do OMS. Colar o ID da área de trabalho e a chave que copiou do portal. Clique em **Seguinte**.


Para instalar o agente num computador Linux:

1. Transfira o pacote adequado (x86 ou x64) para o seu computador Linux utilizando scp/sftp.
2. Instalar o pacote utilizando o – argumento de instalação.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```


### <a name="install-the-dependency-agent"></a>Instalar o agente de dependência
1. Para instalar o agente de dependência num computador Windows, clique duas vezes o ficheiro de configuração e siga o assistente.
2. Para instalar o agente de dependência num computador Linux, instale como raiz utilizando o seguinte comando:

    ```sh InstallDependencyAgent-Linux64.bin```

[Saiba mais](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) sobre sistemas operativos suportados pelo agente de dependência. 

## <a name="refine-the-group-based-on-dependency-visualization"></a>Otimizar o grupo com base na visualização de dependência
Depois de instalar os agentes em todas as máquinas do grupo, pode visualizar as dependências do grupo e refiná-la seguindo os passos abaixo.

1. O Azure migrar em projeto, **gerir**, clique em **grupos**e selecione o grupo.
2. Na página de grupo, clique em **dependências de vista**, para abrir o mapa de dependência do grupo.
3. O mapa de dependência para o grupo mostra os seguintes detalhes:
    - Entrada (clientes) e de ligações de saída (servidores) TCP para/de todas as máquinas que fazem parte do grupo
        - As máquinas dependentes que não tenham MMA e dependência instalado o agente estão agrupadas por números de porta
        - As máquinas de dependenct que tenham o MMA e instalado o agente de dependência são apresentadas como caixas separadas 
    - Processos em execução dentro da máquina, pode expandir cada caixa de máquina para ver os processos
    - Propriedades de nome de domínio completamente qualificado, o sistema operativo, etc. de endereços MAC de cada máquina, pode clicar em cada caixa de máquina para ver estes detalhes

     ![Dependências do grupo de vista](./media/how-to-create-group-dependencies/view-group-dependencies.png)

3. Para ver mais granulares dependências, clique em modificar o intervalo de tempo. Por predefinição, o intervalo é uma hora. Pode modificar o intervalo de tempo, ou especificar o início e fim datas e duração.
4. Verifique se as máquinas dependentes, o processo em execução dentro de cada máquina e identificar as máquinas que devem ser adicionadas ou removidas do grupo.
5. Utilize Ctrl + clique para selecionar máquinas no mapa para adicionar ou removê-los do grupo.
    - Só é possível adicionar máquinas que tenham sido detetadas.
    - Adição e remoção de um grupo de máquinas invalidam passado avaliações do mesmo.
    - Opcionalmente, pode criar uma nova avaliação quando modificar o grupo.
5. Clique em **OK** para guardar o grupo.

    ![Adicionar ou remover máquinas](./media/how-to-create-group-dependencies/add-remove.png)

Se pretende verificar as dependências de um computador específico que aparece no mapa de dependência do grupo, [configurar o mapeamento de dependência de máquina](how-to-create-group-machine-dependencies.md).


## <a name="next-steps"></a>Passos Seguintes

[Saiba mais](concepts-assessment-calculation.md) sobre como avaliações são calculadas.
