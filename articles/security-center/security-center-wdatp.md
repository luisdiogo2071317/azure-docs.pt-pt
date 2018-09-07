---
title: Windows Defender avançadas de proteção contra ameaças (ATP) com o Centro de segurança do Azure (pré-visualização pública) | Documentos da Microsoft
description: Este documento apresenta a integração entre o Centro de segurança do Azure e o Windows Defender ATP.
services: security-center
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: ''
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/06/2018
ms.author: barclayn
ms.openlocfilehash: f59525d6ff006ebd4560f96b2ca12bea3626a841
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44057229"
---
# <a name="windows-defender-advanced-threat-protection-atp-with-azure-security-center-public-preview"></a>Windows Defender avançadas de proteção contra ameaças (ATP) com o Centro de segurança do Azure (pré-visualização pública)

Centro de segurança do Azure é expandir a sua oferta de plataformas de proteção de carga de trabalho de nuvem (CWPP) através da integração com o [do Windows Defender ATP](https://www.microsoft.com/WindowsForBusiness/windows-atp).
Esta alteração oferece capacidades abrangentes de ponto final de deteção e resposta (EDR). Permite-lhe detetar anomalias, detetar e responder a ataques avançados em pontos finais de servidor monitorizados pelo ASC.

Os clientes do Centro de segurança do Azure podem agora utilizar funcionalidades do Windows Defender ATP:

- **Sensores de deteção de violação de publicação de próxima geração:** sensor do Windows Defender ATP para servidores do Windows que recolhe uma grande diversidade de sinais comportamentais para ativar a deteção de ataque avançada e investigação.

- **Deteção de violação de publicação com base na análise, com tecnologia de cloud:** do Windows Defender ATP adapta-se rapidamente aos ameaças em constante mudança. Emprega análises avançadas e de macrodados. Amplificado pelo poder do gráfico de segurança inteligente com sinais no Windows, o Azure e o Office, para detetar ameaças desconhecidas. Ele fornece alertas acionáveis e que lhe permite responder rapidamente.

- **Informações sobre ameaças**: identifica as ferramentas do invasor, técnicas e procedimentos do Windows Defender ATP e gera alertas quando estes estão a ser observados. Ele usa os dados gerados por hunters da Microsoft, as equipes de segurança e aumentaram as informações sobre ameaças fornecidas pelos parceiros.

Estas capacidades estão agora disponíveis no Centro de segurança do Azure:

- Inclusão automática - sensor do Windows Defender ATP automaticamente está ativado nos servidores de Windows que são carregadas ao ASC

- Painel único de vidro no ASC - alertas do Windows Defender ATP estão disponíveis na consola do ASC

- Detalhadas de investigação da máquina - clientes ASC podem aceder a consola do Windows Defender ATP para efetuar uma investigação detalhada para descobrir o âmbito da violação

![* Figura 1 a ver a imagem inteira ao investigar incluindo alertas gerados pelo ASC *](media/security-center-wdatp/image1.png)

Pode investigar o alerta no Centro de segurança do Azure:

![Figura 2 investigação - Centro de segurança do Azure](media/security-center-wdatp/image2.png)

Pode continuar a investigar o alerta por dinamização para o Windows Defender ATP. Aqui pode ver informações adicionais, como a árvore de processos de alerta, de gráficos de incidentes e de uma linha do tempo detalhados de machine expondo todos os comportamentos durante um período de histórico de até seis meses.

![Figura 3 investigação – o Windows Defender ATP](media/security-center-wdatp/image3.png)

## <a name="platform-support"></a>Suporte da plataforma

Esta funcionalidade suporta a deteção no Windows Server 2012 R2 e Windows Server 2016.

Apenas os servidores em subscrições no escalão Standard

## <a name="onboarding-instructions"></a>Instruções de inclusão

- Se já carregada os servidores para o escalão standard do ASC - nenhuma ação necessária, ASC irá carregar automaticamente servidores para WDATP.

- Se nunca integrado a servidores do padrão de ASC camada – integrar ao ASC como de costume.

- Se efetuou os servidores por meio de WDATP:
  - Consulte a documentação para obter orientações sobre [como máquinas de servidor de descarregar](https://go.microsoft.com/fwlink/p/?linkid=852906).
  - Integrar ao ASC

## <a name="access-to-wdatp-portal"></a>Acesso ao portal WDATP

Siga as instruções no [atribuir acesso de utilizador para o portal](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/assign-portal-access-windows-defender-advanced-threat-protection)

## <a name="set-firewall-configuration"></a>Configuração de firewall de conjunto

Se tiver um proxy ou firewall que está a bloquear tráfego anônimo, tal como o sensor do Windows Defender ATP é ligar a partir do contexto do sistema, certificar-se de que o tráfego de anônimo é permitido; Siga as instruções [ativar o acesso aos URLs do serviço do Windows Defender ATP no servidor proxy](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/configure-proxy-internet-windows-defender-advanced-threat-protection#enable-access-to-windows-defender-atp-service-urls-in-the-proxy-server)

## <a name="how-can-i-test-the-feature"></a>Como posso testar a funcionalidade?

 Os seguintes passos irão gerar um alerta de teste WDATP benigno.

1. RDP para uma das VMs do Windows Server (2012R2 ou 2016) na subscrição e abra uma janela da linha de comandos

2. Na linha de comandos, copie e execute o comando abaixo. Tenha em atenção que a janela da linha de comando será fechado automaticamente.

    **PowerShell.exe - NoExit - ExecutionPolicy omissão - NONE oculta (novo objeto WebClient). DownloadFile ('http://127.0.0.1/1.exe', ' c:\\WDATP-teste-teste\\invoice.exe'); Iniciar o processo "c:\\WDATP-teste-teste\\invoice.exe' * *

  ![imagem do comando acima numa janela de linha de comandos](media/security-center-wdatp/image4.jpeg)

3. Se tiver êxito, será apresentado um novo alerta dentro de alguns minutos no portal do ASC e WD ATP.

4. Consulte o alerta no Centro de segurança do Azure, aceda a **alertas de segurança -\> suspeita linha de comandos do Powershell**

5. Da investigação janela, clique na ligação para redirecionar para o portal WDATP

## <a name="next-steps"></a>Passos Seguintes

* [Definir políticas de segurança no Centro de Segurança do Azure](security-center-policies.md) – Saiba como configurar políticas de segurança para as suas subscrições e grupos de recursos do Azure.
* [Gerir recomendações de segurança no Centro de segurança do Azure](security-center-recommendations.md) – Saiba como o recomendações o ajudam a proteger os seus recursos do Azure.
* [Monitorização de estado de funcionamento de segurança no Centro de segurança do Azure](security-center-monitoring.md) – Saiba como monitorizar o estado de funcionamento dos seus recursos do Azure.
