---
title: Como instalar o HANA no SAP HANA no Azure (instâncias grandes) | Documentos da Microsoft
description: Como instalar o HANA no SAP HANA no Azure (instância grande).
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
ms.openlocfilehash: 76a7ce99799b85d81aa6e127ebe1e57e2df3e59a
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44164743"
---
# <a name="example-of-an-sap-hana-installation-on-hana-large-instances"></a>Exemplo de uma instalação de SAP HANA nas instâncias grandes do HANA

Esta secção ilustra como instalar o SAP HANA numa unidade de instância grande do HANA. O estado de início temos ter o seguinte aspeto:

- Proporcionou à Microsoft todos os dados para implementar uma instância grande do SAP HANA.
- A instância grande do SAP HANA que recebeu da Microsoft.
- Criou uma VNet do Azure que está ligada à sua rede no local.
- Ligado o circuito de ExpressRotue para instâncias grandes do HANA para a mesma VNet do Azure.
- Instalou uma VM do Azure a utilizar como uma jumpbox para instâncias grandes do HANA.
- Certificou-se de que pode ligar a partir da jumpbox em sua unidade de instância grande do HANA e vice-versa.
- Tiver selecionado se todos os pacotes necessários e os patches estão instalados.
- Leia as notas de SAP e documentação atualizados sobre a instalação do HANA no sistema operacional estiver a utilizar e certifique-se de que a versão do HANA à escolha é suportada no sistema operacional de versão.

O que é mostrado nas sequências seguintes é o download dos pacotes de instalação HANA à jumpbox VM, nesse caso, executada num sistema operacional Windows a cópia dos pacotes para a unidade de instância grande do HANA e a sequência da configuração.

## <a name="download-of-the-sap-hana-installation-bits"></a>Transferência de bits de instalação SAP HANA
Uma vez que as unidades de instância grande do HANA não tem conectividade direta à internet, não é possível baixar diretamente os pacotes de instalação do SAP para a VM de instância grande do HANA. Para superar a conectividade internet direta em falta, terá da jumpbox. Transfira os pacotes para a jumpbox VM.

Para transferir os pacotes de instalação do HANA, é necessário um utilizador de S SAP ou outro usuário, que permite-lhe aceder ao Marketplace de SAP. Execute esta sequência de ecrãs após iniciar sessão:

Aceda a [SAP Service Marketplace](https://support.sap.com/en/index.html) > clique em transferência de Software > instalações e atualização > pelo índice alfabética > em H – SAP HANA edição da plataforma > edição da plataforma SAP HANA 2.0 > instalação > Baixe o ficheiros seguintes

![Transferir a instalação do HANA](./media/hana-installation/image16_download_hana.PNG)

No caso de demonstração, iremos transferir pacotes de instalação do SAP HANA 2.0. Na caixa de rápida do Azure à VM, expanda os extração de arquivos no diretório conforme mostrado abaixo.

![Extrair a instalação do HANA](./media/hana-installation/image17_extract_hana.PNG)

Como os arquivos são extraídos, copie o diretório criado pela extração, no caso acima 51052030, para a unidade de instância grande do HANA para o volume de /hana/shared num diretório que criou.

> [!Important]
> Não copie os pacotes de instalação para a raiz ou o LUN de arranque, uma vez que o espaço é limitado e tem de ser utilizada por outros processos também.


## <a name="install-sap-hana-on-the-hana-large-instance-unit"></a>Instalar o SAP HANA na unidade instância grande do HANA
Para instalar o SAP HANA, terá de iniciar sessão como raiz do utilizador. Raiz apenas tem permissões suficientes para instalar o SAP HANA.
A primeira coisa que precisa fazer é definir as permissões no diretório copiado através de no/hana/partilhado. As permissões têm de definir como

```
chmod –R 744 <Installation bits folder>
```

Se pretender instalar o SAP HANA com a configuração gráfica, o pacote de gtk2 tem de ser instalado nas instâncias grandes do HANA. Verifique se está instalada com o comando

```
rpm –qa | grep gtk2
```

Ainda mais passos, vamos são demonstrar a configuração de SAP HANA com a interface gráfica do usuário. Como passo seguinte, vá até o diretório de instalação e navegue para o diretório de sub-rotina HDB_LCM_LINUX_X86_64. Iniciar

```
./hdblcmgui 
```
fora do diretório. Agora introdução guiado através de uma sequência de telas em que precisa fornecer os dados para a instalação. No caso demonstrado, iremos estiver a instalar o servidor de base de dados do SAP HANA e os componentes de cliente do SAP HANA. Portanto nossa seleção é "Base de dados do SAP HANA", conforme mostrado abaixo

![Selecione o HANA na instalação](./media/hana-installation/image18_hana_selection.PNG)

No ecrã seguinte, escolha a opção "Instalar o novo sistema"

![Selecione a nova instalação do HANA](./media/hana-installation/image19_select_new.PNG)

Após este passo, tem de selecionar entre os vários componentes adicionais que podem ser instalados além para o servidor de base de dados do SAP HANA.

![Selecione os componentes adicionais do HANA](./media/hana-installation/image20_select_components.PNG)

Para efeitos desta documentação, escolhemos o cliente do SAP HANA e o SAP HANA Studio. Também está instalada a uma instância de aumentar verticalmente. Por conseguinte, no ecrã seguinte, tem de escolher "Sistema anfitrião único" 

![Selecione a instalação de aumentar verticalmente](./media/hana-installation/image21_single_host.PNG)

No ecrã seguinte, tem de fornecer alguns dados

![Fornecer o SID do SAP HANA](./media/hana-installation/image22_provide_sid.PNG)

> [!Important]
> Como ID de sistema HANA (SID), terá de fornecer o mesmo SID, à medida que proporcionou à Microsoft quando pediu a implementação de instância grande do HANA. Escolher um SID diferente faz com que a instalação falhar devido a problemas de permissão de acesso nos volumes diferentes

Como instalação o diretório que utiliza o/hana/partilhado diretório. No próximo passo, tem de fornecer as localizações para os ficheiros de dados HANA e os ficheiros de registo do HANA


![Fornecer a localização do registo do HANA](./media/hana-installation/image23_provide_log.PNG)

> [!Note]
> Deve definir como dados de ficheiros de registo e os volumes que já vinham com os pontos de montagem que contêm o SID que escolheu na seleção de ecrã antes deste ecrã. Se o SID falta de correspondência com aquela que escreveu no, no ecrã de antes, volte atrás e ajustar o SID para o valor que nos pontos de montagem.

No próximo passo, reveja o nome de anfitrião e, eventualmente, corrigi-lo. 

![Nome de anfitrião de revisão](./media/hana-installation/image24_review_host_name.PNG)

No próximo passo, terá também de obter dados que deu à Microsoft quando pediu a implementação de instância grande do HANA. 

![Fornecer ao utilizador do sistema UID e GID](./media/hana-installation/image25_provide_guid.PNG)

> [!Important]
> Tem de fornecer o mesmo ID de utilizador do sistema e ID de grupo de utilizadores à medida que proporcionou à Microsoft como a ordem a implementação de unidade. Se não fornecer os IDs de muito mesmos, a instalação do SAP HANA na unidade instância grande do HANA falha.

Os dois ecrãs, não podemos nesta documentação, tem de fornecer a palavra-passe para o usuário do sistema de base de dados do SAP HANA e a palavra-passe para o utilizador sapadm, que é utilizado para o agente de anfitrião do SAP que é instalado como parte da datab SAP HANA instância do ase.

Depois de definir a palavra-passe, um ecrã de confirmação está a aparecer. Verifique todos os dados apresentados e prosseguir a instalação. Atingir uma tela de progresso que documenta o progresso da instalação, como a mostrada abaixo

![Verificar o progresso da instalação](./media/hana-installation/image27_show_progress.PNG)

Como a instalação estiver concluída, deve uma imagem semelhante ao seguinte

![Conclusão da instalação](./media/hana-installation/image28_install_finished.PNG)

Neste momento, a instância do SAP HANA deve ser até e em execução e está pronto para utilização. Deverá conseguir ligar à mesma a partir do SAP HANA Studio. Além disso, certifique-se de que verifique os patches mais recentes do SAP HANA e aplica esses patches.


**Passos seguintes?**

- Consultar [instâncias grandes do SAP HANA elevada disponibilidade e recuperação após desastre no Azure](hana-overview-high-availability-disaster-recovery.md).

