---
title: "Grupo de máquinas com o Azure migrar de dependências de máquina | Microsoft Docs"
description: "Descreve como criar uma avaliação utilizando dependências de máquina com o serviço Azure migrar."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/25/2017
ms.author: raynew
ms.openlocfilehash: 720380fd14d9eaf4856ad75269a80f2b63a4725f
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2018
---
# <a name="group-machines-using-machine-dependency-mapping"></a>Máquinas de grupo utilizando o mapeamento de dependência de máquina

Este artigo descreve como criar um grupo de computadores para [Azure migrar](migrate-overview.md) avaliação por visualizar as dependências de máquinas. Normalmente, utilize este método quando pretender avaliar a grupos de VMs com níveis superiores de confiança, a verificação entre máquina as dependências, antes de executar uma avaliação. Visualização de dependência pode ajudar a planear a migração para o Azure para eficazmente. Ajuda a garantir que nada for deixado e surprise não ocorrerem quando estiver a migrar para o Azure. Pode detetar todos os sistemas interdependentes que necessitam para migrar em conjunto e identificar se ainda está a servir os utilizadores de um sistema em execução ou é uma candidata para desativar em vez de migração. 


## <a name="prepare-machines-for-dependency-mapping"></a>Preparar máquinas para o mapeamento de dependência
Para ver as dependências de máquinas, terá de transferir e instalar agentes em cada máquina no local que pretende avaliar. Além disso, se tiver máquinas sem conectividade internet, terá de transferir e instalar [OMS gateway](../log-analytics/log-analytics-oms-gateway.md) nos mesmos.

### <a name="download-and-install-the-vm-agents"></a>Transferir e instalar os agentes VM
1. No **descrição geral**, clique em **gerir** > **máquinas**e selecione a máquina necessária.
2. No **dependências** coluna, clique em **instalar agentes**. 
3. No **dependências** página, transfira e instale o Microsoft Monitoring Agent (MMA) e o agente de dependência em cada VM que pretende avaliar.
4. Copie o ID da área de trabalho e a chave. Terá de estes quando instalar o MMA na máquina no local.

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

## <a name="create-a-group"></a>Criar um grupo

1. Depois de instalar os agentes, vá para o portal e clique em **gerir** > **máquinas**.
2. Pesquisa para a máquina onde instalou os agentes.
3. O **dependências** coluna para a máquina deve agora mostrar como **dependências de vista**. Clique na coluna para visualizar as dependências da máquina.
4. O mapa de dependência para a máquina apresenta os detalhes seguintes:
    - Entrada (clientes) e de ligações de saída (servidores) TCP/da máquina
        - As máquinas dependentes que não tenham MMA e dependência instalado o agente estão agrupadas por números de porta
        - As máquinas de dependenct que tenham o MMA e instalado o agente de dependência são apresentadas como caixas separadas 
    - Processos em execução dentro da máquina, pode expandir cada caixa de máquina para ver os processos
    - Propriedades de nome de domínio completamente qualificado, o sistema operativo, etc. de endereços MAC de cada máquina, pode clicar em cada caixa de máquina para ver estes detalhes

 ![Dependências de máquina de vista](./media/how-to-create-group-machine-dependencies/machine-dependencies.png)

4. Pode observar as dependências para durações de hora diferente, clicando na duração de tempo na etiqueta de intervalo de tempo. Por predefinição, o intervalo é uma hora. Pode modificar o intervalo de tempo, ou especificar o início e fim datas e duração.
5. Depois de ter identificado dependentes máquinas que pretende agrupar, utilize Ctrl + clique para selecionar várias máquinas no mapa e clique em **grupo máquinas**.
6. Especifique um nome de grupo. Certifique-se de que as máquinas dependentes são detetadas pelo Azure migrar. 

    > [!NOTE]
    > Se uma máquina dependente não é detetada pelo Azure migrar, não é possível adicioná-lo ao grupo. Ao adicionar esses computadores ao grupo, terá de executar o processo de deteção novamente com o âmbito direita no vCenter Server e certifique-se de que a máquina é detetada pelo Azure migrar.  

7. Se pretender criar uma avaliação para este grupo, selecione a caixa de verificação para criar uma nova avaliação para o grupo.
8. Clique em **OK** para guardar o grupo.

Quando o grupo for criado, recomenda-se para instalar agentes em todas as máquinas do grupo e refine o grupo por visualizar a dependência do grupo inteiro.

## <a name="next-steps"></a>Passos Seguintes

- [Saiba como](how-to-create-group-dependencies.md) para otimizar o grupo por visualizar dependências de grupo
- [Saiba mais](concepts-assessment-calculation.md) sobre como avaliações são calculadas.
