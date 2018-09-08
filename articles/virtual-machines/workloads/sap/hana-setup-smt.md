---
title: Como servidor SMT de configuração para o SAP HANA no Azure (instâncias grandes) | Documentos da Microsoft
description: Como configurar o servidor SMT para SAP HANA no Azure (instância grande).
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5e729c2e3a802df15973fc6a43ee42265d1de654
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44164747"
---
# <a name="setting-up-smt-server-for-suse-linux"></a>Configurar o servidor SMT para SUSE Linux
Instâncias grandes do SAP HANA não tem conectividade direta à Internet. Por conseguinte, não é um processo simples para registar tal uma unidade com o fornecedor do sistema operacional e para transferir e aplicar patches. Se SUSE Linux, uma solução pode ser configurar um servidor SMT numa VM do Azure. Ao passo que a VM do Azure tem de ser hospedado numa VNet do Azure, que está ligado à instância grande do HANA. Com esse um servidor SMT, a unidade de instância grande do HANA poderia se registrar e baixar patches. 

SUSE fornece um guia maior sobre seus [ferramenta de gestão de subscrição para o SLES 12 SP2](https://www.suse.com/documentation/sles-12/pdfdoc/book_smt/book_smt.pdf). 

Como pré-condição para a instalação de um servidor SMT que atenda a tarefa para a instância grande do HANA, seria necessário:

- Uma VNet do Azure que está ligada para o circuito de ER instância grande do HANA.
- Uma conta SUSE que está associada uma organização. Ao passo que a organização precisaria ter alguns subscrição SUSE válida.

## <a name="installation-of-smt-server-on-azure-vm"></a>Instalação do server SMT numa VM do Azure

Neste passo, instala o servidor SMT numa VM do Azure. A primeira medida é iniciar sessão para o [atendimento ao consumidor do SUSE](https://scc.suse.com/)

Como está conectado, vá para a organização--> credenciais da organização. Nesta secção, deve encontrar as credenciais que são necessárias para configurar o servidor SMT.

O terceiro passo é instalar uma VM do Linux SUSE na VNet do Azure. Para implementar a VM, pegar uma imagem da Galeria de SLES 12 SP2 do Azure. No processo de implantação, não defina um nome DNS e não utilize endereços IP estáticos como visto na seguinte captura de ecrã

![implementação da VM para o servidor SMT](./media/hana-installation/image3_vm_deployment.png)

A VM implementada era uma VM mais pequena e tem o endereço IP na VNet do Azure de 10.34.1.4. Nome da VM foi smtserver. Após a instalação, a conectividade com a unidade de instância grande do HANA (s) foi verificada. Depende de como organizado resolução de nomes poderá ter de configurar a resolução das unidades de instância grande do HANA no etc/anfitriões da VM do Azure. Adicione um disco adicional para a VM que vai ser utilizado para conter os patches. O disco de arranque em si pode ser demasiado pequeno. No caso demonstrado, o disco foi montado para /srv/www/htdocs conforme mostrado na captura de ecrã seguinte. Um disco de 100 GB deve ser suficientes.

![implementação da VM para o servidor SMT](./media/hana-installation/image4_additional_disk_on_smtserver.PNG)

Entrar para a instância grande do HANA unidade (s), manter /etc/hosts e verifique se pode chegar a VM do Azure que deve ser executado o servidor SMT através da rede.

Depois que essa verificação é feita com êxito, terá de iniciar sessão na VM do Azure que deve executar o servidor SMT. Se estiver a utilizar o putty para iniciar sessão na VM, terá de executar esta sequência de comandos na janela do bash:

```
cd ~
echo "export NCURSES_NO_UTF8_ACS=1" >> .bashrc
```

Depois de executar estes comandos, reinicie o bash para ativar as definições. Em seguida, inicie YAST.

Na YAST, aceda a manutenção de Software e procurar smt. Selecione smt, que muda automaticamente para o yast2 smt, conforme mostrado abaixo

![SMT no yast](./media/hana-installation/image5_smt_in_yast.PNG)


Aceite a seleção para instalação no smtserver. Uma vez instalado, vá para a configuração do servidor SMT e introduza as credenciais organizacionais do Centro de SUSE obtido anteriormente. Também introduza o nome de anfitrião de VM do Azure como o URL do servidor SMT. Nesta demonstração, era https://smtserver tal como apresentado no gráfico seguinte.

![Configuração do servidor SMT](./media/hana-installation/image6_configuration_of_smtserver1.png)

Como passo seguinte, precisa testar se a ligação para o atendimento ao consumidor do SUSE funciona. Como pode ver nos gráficos seguintes, no caso de demonstração, ele funcionava.

![Teste ligar para o atendimento ao consumidor do SUSE](./media/hana-installation/image7_test_connect.png)

Uma vez iniciada a configuração SMT, tem de fornecer uma palavra-passe da base de dados. Uma vez que é uma nova instalação, terá de definir essa palavra-passe, como mostra o gráfico seguinte.

![Definir a palavra-passe para a base de dados](./media/hana-installation/image8_define_db_passwd.PNG)

A interação seguinte que tiver é quando é criado um certificado. Percorrer a caixa de diálogo, conforme mostrado a seguir e deve continuar a etapa.

![Criar certificado para o servidor SMT](./media/hana-installation/image9_certificate_creation.PNG)

Pode haver alguns minutos gastos no passo de "Verificação de sincronização de execução" no final da configuração. Após a instalação e configuração do servidor SMT, deve encontrar o repositório de diretório numa montagem ponto /srv/www/htdocs/além de alguns subdiretórios no repositório. 

Reinicie o servidor SMT e serviços relacionados com estes comandos.

```
rcsmt restart
systemctl restart smt.service
systemctl restart apache2
```

## <a name="download-of-packages-onto-smt-server"></a>Transferência de pacotes do SMT server

Depois de todos os serviços são reiniciados, selecione os pacotes apropriados no gerenciamento de SMT usando Yast. A seleção de pacote depende a imagem do SO do servidor instância grande do HANA e não a versão SLES ou a versão da VM a executar o servidor SMT. Um exemplo do ecrã de seleção é mostrado abaixo.

![Selecione pacotes](./media/hana-installation/image10_select_packages.PNG)

Quando tiver terminado com a seleção de pacote, precisa para começar a cópia inicial dos pacotes selecionadas para o servidor SMT que configurou. Esta cópia é acionada de shell usando o comando smt-espelhamento conforme mostrado abaixo


![Transfira pacotes para o servidor SMT](./media/hana-installation/image11_download_packages.PNG)

Como pode ver acima, os pacotes devem ser copiados em diretórios criados sob a montagem ponto /srv/www/htdocs. Este processo pode demorar algum tempo. Depende de quantos pacotes que selecionar, ele pode demorar até uma hora ou mais.
Como esse processo terminar, terá de mover para a configuração de cliente SMT. 

## <a name="set-up-the-smt-client-on-hana-large-instance-units"></a>Configurar o cliente SMT em unidades de instância grande do HANA

Neste caso, os clientes são as unidades de instância grande do HANA. A configuração de servidor SMT copiados clientSetup4SMT.sh o script na VM do Azure. Cópia do script sobre a unidade de instância grande do HANA pretende ligar ao seu servidor SMT. Iniciar o script com a opção -h e conceda-lhe como parâmetro o nome do seu servidor SMT. No smtserver neste exemplo.

![Configurar cliente SMT](./media/hana-installation/image12_configure_client.PNG)

Pode haver um cenário em que o carregamento do certificado do servidor pelo cliente foi concluída com êxito, mas o registo falha conforme mostrado abaixo.

![Registo de cliente falhar](./media/hana-installation/image13_registration_failed.PNG)

Se o registo falhou, leia isto [SUSE suporta documentos](https://www.suse.com/de-de/support/kb/doc/?id=7006024) e execute as etapas descritas aqui.

> [!IMPORTANT] 
> Como o nome do servidor tem de fornecer o nome da VM, neste smtserver maiúsculas, sem o nome de domínio completamente qualificado. Simplesmente, a funciona de nome VM. 

Depois destes passos tiverem sido executados, tem de executar o seguinte comando na unidade instância grande do HANA

```
SUSEConnect –cleanup
```

> [!Note] 
> No nossos testes tínhamos sempre de aguardar alguns minutos após esse passo. ClientSetup4SMT.sh execução imediata, depois das medidas corretivas descritas no artigo SUSE, terminou com mensagens que o certificado não seria válido ainda. A aguardar 5 a 10 minutos, normalmente e executar clientSetup4SMT.sh terminou numa configuração de cliente com êxito.

Se tiver executado para o problema que precisava corrigir com base nos passos do artigo SUSE, terá de reiniciar clientSetup4SMT.sh na unidade instância grande do HANA novamente. Agora ele deve ser concluído com êxito, conforme mostrado abaixo.

![Registo de cliente foi concluída com êxito](./media/hana-installation/image14_finish_client_config.PNG)

Neste passo, configurou o cliente SMT da unidade instância grande do HANA para ligar o servidor de SMT instalado na VM do Azure. Agora pode tirar 'zypper se' ou "zypper no" para instalar os patches de SO para instâncias grandes do HANA ou instalar pacotes adicionais. É compreendida que só pode obter patches que transferiu anteriormente no servidor SMT.

**Passos seguintes?**
- Consultar [instalação de HANA HLI](hana-example-installation.md).











