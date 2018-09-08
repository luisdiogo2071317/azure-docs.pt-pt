---
title: Alertas de segurança por tipo no Centro de Segurança do Azure | Microsoft Docs
description: Este artigo explica os vários tipos de alertas de segurança disponíveis no Centro de Segurança do Azure.
services: security-center
documentationcenter: na
author: terrylan
manager: mbaldwin
editor: ''
ms.assetid: b3e7b4bc-5ee0-4280-ad78-f49998675af1
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/21/2018
ms.author: yurid
ms.openlocfilehash: 6261f78b4943afdbf74236bc40bea6db5cc62329
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159389"
---
# <a name="understanding-security-alerts-in-azure-security-center"></a>Compreender os alertas de segurança no Centro de Segurança do Azure
Este artigo ajuda-o a compreender os vários tipos de alertas de segurança e informações relacionadas que estão disponíveis no Centro de Segurança do Azure. Para obter mais informações sobre como gerir alertas e incidentes, veja [Gerir e responder a alertas de segurança no Centro de Segurança do Azure](security-center-managing-and-responding-alerts.md).

Para configurar as deteções avançadas, atualize para o Centro de Segurança do Azure Standard. Está disponível uma avaliação gratuita de 60 dias. Para atualizar, selecione **Escalão de Preço** na [política de segurança](security-center-policies.md). Para saber mais, veja a [página de preços](https://azure.microsoft.com/pricing/details/security-center/).

> [!NOTE]
> O Centro de Segurança lançou uma pré-visualização limitada de um novo conjunto de deteções que tiram partido dos registos de auditoria, um framework de auditoria comum, para detetar comportamentos maliciosos em computadores Linux. Envie um e-mail para a [Microsoft](mailto:ASC_linuxdetections@microsoft.com) com os seus IDs de subscrição para aderir à pré-visualização.

## <a name="what-type-of-alerts-are-available"></a>Que tipo de alertas estão disponíveis?
O Centro de Segurança do Azure utiliza uma variedade de [capacidades de deteção](security-center-detection-capabilities.md) para alertar os clientes para possíveis ataques direcionados aos respetivos ambientes. Estes alertas contêm informações valiosas sobre o que acionou o alerta, os recursos afetados e a origem do ataque. As informações incluídas num alerta variam de acordo com o tipo de análise usado para detetar a ameaça. Os incidentes também podem conter informações contextuais adicionais que podem ser úteis ao investigar uma ameaça.  Este artigo fornece informações sobre os seguintes tipos de alerta:

* Análise Comportamental de Máquinas Virtuais (VMBA)
* Análise de Rede
* Base de Dados SQL e SQL Data Warehouse Analysis
* Informações Contextuais

## <a name="virtual-machine-behavioral-analysis"></a>Análise comportamental de máquinas virtuais
O Centro de Segurança do Azure pode utilizar a análise comportamental para identificar recursos comprometidos com base na análise de registos de eventos de máquinas virtuais. Por exemplo, Eventos de Criação de Processos e Eventos de Início de Sessão. Além disso, existe uma correlação com outros sinais para procurar provas de uma campanha ampla.

> [!NOTE]
> Para obter mais informações sobre como funcionam as capacidades de deteção do Centro de Segurança, veja [Capacidades de deteção do Centro de Segurança do Azure](security-center-detection-capabilities.md).


### <a name="event-analysis"></a>Análise de eventos
O Centro de Segurança utiliza análises avançadas para identificar recursos comprometidos com base na análise de registos de eventos de máquinas virtuais. Por exemplo, Eventos de Criação de Processos e Eventos de Início de Sessão. Além disso, existe uma correlação com outros sinais para procurar provas de uma campanha ampla.

* **Execução de processo suspeito detetada**: os atacantes tentam frequentemente executar código malicioso sem deteção por mascarando-o como processos benignos. Estes alertas indicam que uma execução de processo correspondeu a um dos padrões seguintes:
    * Um processo conhecido a ser utilizado para fins maliciosos foi executado. Enquanto os comandos individuais podem parecer benignos o alerta é classificado com base numa agregação destes comandos.
    * Um processo foi executado a partir de uma localização invulgar.
    * Um processo foi executado a partir de uma localização em comum com ficheiros suspeitos conhecidos.
    * Um processo foi executado a partir de um caminho suspeito.
    * Um processo foi executado num contexto anormal.
    * Um processo foi executado por uma conta invulgar
    * Um processo com uma extensão suspeita foi executado.
    * Um processo com uma extensão suspeita dupla foi executado.
    * Um processo com um caráter (RLO) da direita para a esquerda suspeito no respetivo nome de ficheiro foi executado.
    * Um processo cujo nome é muito semelhante, mas diferente a partir de um processo de execução muito comum foi executado
    * Um processo cujo nome corresponde a uma ferramenta de atacante conhecida foi executado.
    * Um processo com um nome aleatório foi executado.
    * Um processo com uma extensão suspeita foi executado.
    * Um ficheiro oculto foi executado.
    * Um processo foi executado como um subordinado de outro processo não relacionado.
    * Um processo invulgar foi criado por um processo de sistema.
    * Um processo anómalo foi iniciado pelo serviço do Windows Update.
    * Um processo foi executado com uma linha de comandos invulgar. Este foi associado com processos legítimos que está a ser hijacked para execução de conteúdo malicioso.
    * Uma tentativa de iniciar todos os executáveis (*.exe) num diretório foi executada a partir da linha de comandos.
    * Um processo foi executado pelo utilitário do PsExec, que pode ser utilizado para executar processos remotamente.
    * O executável principal do Apache Tomcat® (Tomcat#.exe) foi utilizado para iniciar processos subordinados suspeitos, que podem alojar ou iniciar comandos maliciosos.
    * O "Assistente do Programa de Compatibilidade" Microsoft Windows (pcalua.exe) foi utilizado para iniciar o código executável que pode ser malicioso.
    * Uma rajada de termino de um processo suspeito foi detetada.
    * Um processo de sistema SVCHOST foi executado num contexto anormal.
    * Um processo de sistema SVCHOST foi executado num grupo de serviço raro.
    * Uma linha de comandos suspeita foi executada.
    * Um script do PowerShell tem características em comum com scripts suspeitos conhecidos.
    * Um cmdlet do PowerShell Powersploit malicioso conhecido foi executado.
    * Um utilizador SQL incorporado executou um processo que normalmente não é executado
    * Foi detetado um executável codificado com Base 64, que pode indicar um atacante a tentar anular a deteção construindo um executável no momento através de uma sequência de comandos.

* **Atividade de recursos RDP suspeita**: frequentemente, os atacantes têm como alvo portas de gestão aberta como RDP com ataques de força bruta. Estes alertas indicam atividade de início de sessão de Ambiente de Trabalho Remoto suspeita:
    * foram tentados inícios de sessão de Ambiente de Trabalho Remoto.
    * foram tentados inícios de sessão de Ambiente de Trabalho Remoto utilizando contas inválidas.
    * foram tentados inícios de sessão de Ambiente de Trabalho Remoto, alguns dos quais conseguiram iniciar sessão com êxito na máquina.
* **Atividade de recursos SSH suspeita**: frequentemente, os atacantes têm como alvo portas de gestão aberta como SSH com ataques de força bruta. Estes alertas indicam atividade de início de sessão SSH suspeita:
    * foram tentados inícios de sessão SSH falhados.
    * foram tentados inícios de sessão SSH, alguns dos quais foram efetuados com êxito.
* **Valor de registo WindowPosition suspeito**: este alerta indica que foi tentada uma alteração de configuração de registo WindowPosition, que pode ser indicador de ocultar as janelas de aplicação em secções não visível do ambiente de trabalho.
* **Potencial tentativa para ignorar o AppLocker**: o AppLocker pode ser utilizado para limitar os processos que podem ser executados no Windows, limitando a exposição a software maligno. Este alerta indica uma possível tentativa de ignorar a restrições do AppLocker utilizando executáveis fidedignos (permitidos pela política do AppLocker) para executar código não fidedigno.
* **Comunicações pipe com nomes suspeitos**: este alerta indica que os dados foram escrita num local com o nome do pipe a partir de um comando de consola do Windows. Os pipes nomeados são conhecidos por ser utilizados pelos atacantes para tarefas e para comunicar com uma implantação maliciosa.
* **Descodificar um executável utilizando a ferramenta de certutil.exe incorporada**: este alerta indica que um utilitário de administrador incorporado, certutil.exe, foi utilizada para descodificar um executável. Os atacantes são conhecidos por abusar da funcionalidade de ferramentas de administração legítimas para efetuar ações maliciosas, por exemplo, utilizando uma ferramenta como o certutil.exe para descodificar um executável malicioso que, em seguida, será executado posteriormente.
* **Um registo de eventos foi limpo**: este alerta indica uma operação de limpeza de um registo de eventos suspeito, que é frequentemente utilizada pelos atacantes para tentar disfarçar os ataques.
* **Desativar e eliminar ficheiros de registo IIS**: este alerta indica que o ficheiro de registo IIS foi desativado e/ou eliminado, frequentemente utilizado pelos atacantes para tentar disfarçar os ataques.
* **Eliminação de ficheiros suspeita**: este alerta indica eliminações de ficheiros suspeitas, que podem ser utilizadas por um atacante para remover uma prova dos binários maliciosos.
* **Todas as cópias sombra do ficheiro foram eliminadas**: este alerta indica que as cópias sombra foram eliminadas.
* **Comandos de limpeza do ficheiro suspeito**: este alerta indica uma combinação de comandos systeminfo utilizada para executar a atividade de limpeza automática de pós-compromisso.  Enquanto o *systeminfo.exe* é uma ferramenta Windows legítima, executá-la duas vezes sucessivamente, seguida de um comando de eliminação da forma que ocorreu aqui é incomum.
* **Criação de conta suspeita**: este alerta indica que foi criada uma conta com um semelhança elevada de uma conta com privilégios administrativos existente. Esta técnica pode ser utilizada por atacantes para criar uma conta de adesão sem ser detetado.
* **Atividade de cópia sombra de volume suspeita**: este alerta indica a atividade de eliminação de cópia sombra de volumes no recurso. A Cópia Sombra de Volume (VSC) é um artefacto importante que armazena os instantâneos de dados. Esta atividade é normalmente associada a Ransowmare, mas também poderia ser legítima.
* **Método de persistência de registo do Windows**: este alerta indica uma tentativa de manter um executável no registo do Windows. O software maligno, muitas vezes, utiliza esta técnica para continuam a vigorar após um arranque.
* **Nova regra de firewall suspeita**: este alerta indica que foi adicionada uma nova regra de Firewall através de *netsh.exe* para permitir tráfego de um executável numa localização suspeita.
* **Execuções de XCOPY suspeitas**: este alerta indica uma série de execuções de XCOPY que podem assinalar que uma das suas máquinas foi comprometida e foi utilizada para propagar software maligno.
* **Supressão de avisos legais apresentado aos utilizadores no início de sessão**: este alerta indica uma alteração à chave de registo que controla se é apresentado um aviso legal aos utilizadores quando iniciam sessão. Esta é uma atividade comum realizada pelos atacantes após terem comprometido um anfitrião.
* **Combinação anómala de carateres maiúsculos e minúsculos detetados na linha de comandos**: este alerta indica que a utilização de uma combinação de carateres maiúsculos e minúsculos na linha de comandos, que é uma técnica utilizada pelos atacantes para ocultar maiúsculas e minúsculas ou regras de máquina com base em resumos de mensagens.
* **Linha de comandos oculta**: este alerta indica que foram detetados indicadores suspeitos de encobrimento na linha de comandos.
* **Várias contas de domínio a ser consultadas**: os atacantes consultam frequentemente as contas de domínio do AD ao executar o reconhecimento nos utilizadores, as contas de administrador de domínio, os controladores de domínio e as relações entre domínios de confiança. Este alerta indica que um número de contas de domínio distintos invulgar foi consultado durante um período de tempo curto.
* **Atividade de reconhecimento local possível**: este alerta indica que uma combinação de comandos systeminfo associados à atividade de reconhecimento foram executados.  Enquanto o *systeminfo.exe* é uma ferramenta Windows legítima, executá-la duas vezes sucessivamente é raro.
* **Execução possível de keygen executáveis**: este alerta indica que um processo cujo nome é indicativo da ferramenta de keygen foi executado. Essas ferramentas são normalmente utilizadas para erradicar mecanismos de licenciamento de software, mas as transferências, muitas vezes, são agrupada com outro software malicioso.
* **Execução suspeita através do rundll32.exe**: este alerta indica que o rundll32.exe foi utilizado para executar um processo com um nome invulgar, consistente com o esquema de nomenclatura de processo utilizado pelos atacantes para instalar um implante de primeira fase num anfitrião comprometido.
* **Combinação suspeita de HTA e PowerShell**: este alerta indica que o Anfitrião da Aplicação de HTML (HTA) da Microsoft está a iniciar comandos do PowerShell. Esta é uma técnica utilizada pelos atacantes para iniciar scripts maliciosos do PowerShell.
* **Alterar para uma chave de registo que pode ser abusada para ignorar o UAC**: este alerta indica que uma chave de registo que pode ser abusada para ignorar o UAC (Controlo de Conta de Utilizador) foi alterada, o que é utilizado frequentemente pelos atacantes para mover de utilizador de acesso sem privilégios (standard) para acesso com privilégios (por exemplo, administrador) num anfitrião comprometido.
* **Utilização suspeita do nome de domínio dentro da linha de comandos**: este alerta indica que foi utilizado um nome de domínio suspeito, que pode ser prova de que um atacante está a alojar ferramentas maliciosas e como pontos finais para comandos e controlo e para filtragem de dados.
* **Foi criada uma conta em vários anfitriões num período de tempo de 24 horas**: este alerta indica que foi efetuada uma tentativa de criar a mesma conta de utilizador em vários anfitriões, o que podem ser prova de um atacante a mover-se lateralmente em toda a rede depois de uma ou mais entidades de rede terem sido comprometidas.
* **Utilização suspeita de CACLS para reduzir o estado de segurança do sistema**: este alerta indica que a lista de controlo de acesso de alterações (CACLS) foi alterada. Esta técnica, muitas vezes, é utilizada pelos atacantes para dar acesso total aos binários de sistema como ftp.exe, net.exe, wscript.exe etc.
* **Parâmetros de ataque de Kerberos Golden Ticket suspeitos**: este alerta indica que os parâmetros da linha de comandos consistentes com um ataque de Kerberos Golden Ticket foram executados. Uma chave krbtgt comprometida pode ser utilizada por um atacante para represente qualquer utilizador que desejar.
* **Ativação da chave de registo WDigest UseLogonCredential**: este alerta indica que a chave de registo foi alterada para permitir que as credenciais de início de sessão fossem armazenada num texto não encriptado na memória do LSA, que, em seguida, podem ser recolhidas da memória.
* **Potencial utilização suspeita da ferramenta Telegram**: este alerta indica que a instalação do Telegram, uma serviço de mensagens instantâneas gratuito baseado na nuvem utilizado pelos atacantes para transferir os binários maliciosos para outro computador, telemóvel ou tablet.
* **Criação do novo ASEP**: este alerta indica a criação de um novo ASEP (Ponto de Extensibilidade de Início Automático), que faz com que o nome do processo identificado na linha de comandos seja iniciado automaticamente e que possa ser utilizado por um atacante para alcançar a persistência.
* **Alterações Set-ExecutionPolicy e WinRM suspeitas**: este alerta indica as alterações de configuração, que estão associadas à utilização de webshell de ChinaChopper maliciosa.
* **Desativação de serviços críticos**: este alerta indica que o comando "net.exe stop" foi utilizado para parar serviços críticos como SharedAccess ou Centro de Segurança do Windows.
* **Utilização suspeita de comutador de FTP-s**: este alerta indica a utilização do comutador "-s" de FTP, que pode ser utilizado por software maligno para ligar a um servidor remoto de FTP e transferir os binários maliciosos adicionais.
* **Execução suspeita do comando VBScript.Encode**: este alerta indica que o comando *VBScript.Encode* foi executado, o qual codifica scripts para texto ilegível, tornando mais difícil para os utilizadores examinar o código.
* **Alocação de objeto de HTTP de VBScript**: este alerta indica a criação de um ficheiro de VBScript utilizando a linha de comandos, que pode ser utilizado para transferir ficheiros maliciosos.
* **Ataque de chaves temporárias**: este alerta indica que um atacante pode estar a subverter uma acessibilidade binária (por exemplo chaves temporária, teclado no ecrã, narrador) para fornecer acesso através da backdoor.
* **Indicadores de ransomware Petya**: este alerta indica as técnicas associadas ao ransomware Petya que foram observadas.
* **Tentativa de desativar AMSI**: este alerta indica uma tentativa de desativar a interface de análise antimalware (AMSI), que desativa a deteção de antimalware.
* **Indicadores de ransomware**: este alerta indica atividades suspeitas tradicionalmente associadas a ransomware de bloqueio de ecrã e encriptação.
* **Ficheiro de saída de coleção de rastreio suspeito**: este alerta indica que um rastreio (por exemplo, de atividade de rede) foi recolhido e extraído para um tipo de ficheiro invulgar.
* **Software de alto risco**: este alerta indica a utilização de software que foi associado com a instalação de software maligno. Frequentemente, os atacantes empacotam software maligno com ferramentas benignas, tal como o mostrado neste alerta e instalam automaticamente o software maligno em segundo plano.
* **Criação de ficheiro suspeita**: este alerta indica que a criação ou execução de um processo utilizado pelos atacantes para transferir o software maligno adicional para um anfitrião comprometido depois de um anexo num documento de phishing foi aberto.
* **Credenciais suspeitas na linha de comandos**: este alerta indica uma palavra-passe suspeita utilizada para executar um ficheiro. Esta técnica foi utilizada pelos atacantes para executar o software maligno Pirpi.
* **Possível execução de dropper malware**: este alerta indica um nome de ficheiro que foi utilizado pelos atacantes para instalar o software maligno.
* **Execução suspeita através do rundll32.exe**: este alerta indica que o rundll32.exe está a ser utilizado para executar um notepad.exe ou reg.exe, consistente com a técnica de injeção de processo utilizada pelos atacantes.
* **Argumentos da linha de comandos suspeita**: este alerta indica argumentos de linha de comandos suspeitos que foram utilizados em conjunto com uma shell inversa utilizada pelo grupo de atividade HYDROGEN.
* **Credenciais do documento suspeitas**: este alerta indica um palavra-passe hash calculada suspeita comum utilizada por software maligno que está a ser utilizada para executar um ficheiro.
* **Construção de script PS dinâmico**: este alerta indica um script do PowerShell que está a ser construído dinamicamente. Os atacantes utilizam esta técnica para construir progressivamente um script para evadir os sistemas de IDS.
* **Indicadores Metaploit**: este alerta indica atividade associada com o framework de Metasploit, que fornece uma variedade de ferramentas e capacidades de atacante.
* **Atividade suspeita na conta**: este alerta indica uma tentativa de ligar a uma máquina através de uma conta que ficou comprometida recentemente.
* **Criação de contas**: este alerta indica uma criação de uma nova conta na máquina.


### <a name="crash-analysis"></a>Análise de falhas


A análise de memória de informação de falha de memória é um método utilizado para detetar software maligno sofisticado que é capaz de se esconder de soluções de segurança tradicionais. As várias formas de software maligno tentam reduzir a possibilidade de ser detetado por produtos de software antivírus, ao nunca escrever no disco ou ao encriptar componentes de software escritos em disco. Esta técnica faz com que o software maligno seja difícil de detetar com soluções contra software maligno tradicionais. No entanto, este tipo de software maligno pode ser detetado através da análise de memória, uma vez que o software maligno tem de deixar rastreios na memória para funcionar.

Quando o software falha, uma informação de falha de sistema captura uma parte da memória no momento da falha. A falha pode ser provocada por software maligno, aplicações gerais ou problemas de sistema. Ao analisar a memória na informação de falha de sistema, o Centro de Segurança pode detetar técnicas utilizadas para explorar vulnerabilidades no software, aceder a dados confidenciais e persistir, de maneira sub-reptícia, numa máquina afetada. Isto é conseguido com um impacto mínimo no desempenho nos anfitriões, porque a análise é realizada pelo back-end do Centro de Segurança.

* **Injeção de código detetada**: a injeção de código é a inserção de módulos executáveis em processos ou threads em execução. Esta técnica é utilizada por software maligno para aceder a dados, ocultar ou impedir a sua remoção (por exemplo, persistência). Este alerta indica que está presente nas informações de falha de sistema um módulo injetado. Os programadores de software legítimo efetuam ocasionalmente a injeção de código por motivos não maliciosos, por exemplo, para modificar ou expandir uma aplicação ou componente do sistema operativo existente. Para ajudar a diferenciar entre módulos maliciosos e não maliciosos injetados, o Centro de Segurança verifica se o módulo inserido está ou não em conformidade com um perfil de comportamento suspeito. O resultado desta verificação é indicado pelo campo "SIGNATURE" do alerta e é refletido na gravidade do alerta, na descrição do alerta e nos passos de remediação do alerta.
* **Segmento de código suspeito**: o alerta do segmento de código suspeito indica que foi alocado um segmento de código através de métodos não padrão, como injeção reflexiva e “hollowing” de processos. Além disso, as características adicionais do segmento de código são processadas para dar contexto relativamente às capacidades e comportamentos do segmento reportado.
* **Shellcode detetado**: o Shellcode é o payload executado depois de o software maligno explorar uma vulnerabilidade de software. Este alerta indica que a análise da informação de falha de sistema detetou código executável que apresenta um comportamento que é normalmente tido por payloads maliciosos. Apesar de o software não malicioso poder ter este comportamento, não é característico das práticas normais de desenvolvimento de software.
* **Módulo hijacking detetado**: o Windows utiliza DLLs (dynamic-link libraries) para permitir que o software utilize a funcionalidade do sistema Windows comum. O Hijacking de DLL ocorre quando software maligno altera a ordem de carregamento da DLL para carregar vários payloads maliciosos na memória, podendo ser executado código arbitrário. Este alerta indica que a análise de informação de falha de sistema detetou um módulo com o mesmo nome carregado a partir de dois caminhos diferentes. Um dos caminhos carregados é proveniente de uma localização binária comum do sistema do Windows. Por vezes, os programadores de software legítimo alteram a ordem de carregamento do DLL por motivos não maliciosos, por exemplo, para instrumentação, para expansão do SO Windows ou para expansão de uma aplicação do Windows. Para ajudar a fazer a distinção entre alterações maliciosas e potencialmente benignas na ordem de carregamento do DLL, o Centro de Segurança verifica se um módulo carregado está em conformidade com um perfil suspeito.
* **Módulo Masquerading Windows detetado**: o software maligno pode utilizar nomes comuns dos binários do sistema Windows (por exemplo, SVCHOST.EXE) ou módulos (por exemplo, NTDLL.DLL) para se infiltrar e ocultar dos administradores de sistema a natureza do software malicioso. Este alerta indica que o ficheiro de informação de falha de sistema contém módulos que utilizam nomes de módulos do sistema Windows, mas que não satisfazem outros critérios característicos dos módulos do Windows. Analisar a cópia em disco do módulo de disfarce pode fornecer mais informações sobre a natureza legítima ou maliciosa deste módulo.
* **Binário de sistema modificado detetado**: o software maligno pode modificar os binários do sistema principal para aceder a dados de maneira dissimulada ou persistir sub-reticiamente num sistema comprometido. Este alerta indica que a análise da informação de falha de sistema detetou que foram modificados binários principais do SO Windows na memória ou no disco. Os programadores de software legítimo modificam ocasionalmente módulos do sistema na memória por razões não maliciosas, por exemplo, para desvios ou para compatibilidade de aplicações. Para ajudar a fazer a distinção entre módulos maliciosos e potencialmente legítimos, o Centro de Segurança verifica se o módulo modificado está em conformidade com um perfil suspeito.

## <a name="network-analysis"></a>Análise de rede
A deteção de ameaças de rede do Centro de Segurança funciona através da recolha automática de informações de segurança a partir do tráfego do Azure IPFIX (Internet Protocol Flow Information Export). Analisa esta informação, muitas vezes correlacionando informações de várias origens, para identificar ameaças.

* **Possíveis tentativas de força brutal no SQL de entrada**: a análise de tráfego de rede detetou comunicação de entrada SQL suspeita. Esta atividade é consistente com as tentativas de força bruta contra servidores SQL.
* **Atividade de rede RDP de entrada suspeita em várias origens**: a análise de tráfego de rede detetou comunicações RDP (Remote Desktop Protocol) de entrada anómalas a partir de várias origens. Especificamente, os dados de rede de amostragem mostram a ligação de IPs exclusivos à máquina, que é considerada anormal para este ambiente. Esta atividade pode indicar uma tentativa de força bruta no ponto de final RDP a partir de vários anfitriões (Botnet).
* **Atividade de rede RDP de entrada suspeita**: a análise de tráfego de rede detetou comunicações RDP (Remote Desktop Protocol) de entrada anómalas. Especificamente, os dados de rede de amostragem mostram um elevado número de ligações à máquina, que é considerada anormal para este ambiente. Esta atividade pode indicar uma tentativa de força bruta no ponto de final RDP.
* **Atividade de rede RDP de saída suspeita para vários destinos**: a análise de tráfego de rede detetou comunicações RDP (Remote Desktop Protocol) de saída anómalas a para vários destinos. Esta atividade pode indicar que a máquina foi comprometida e que, agora, está a ser utilizada força bruta externa nos pontos finais RDP. Tenha em atenção que este tipo de atividade, possivelmente, pode fazer com que o IP seja sinalizados como malicioso por entidades externas.
* **Atividade de rede RDP de saída suspeita**: a análise de tráfego de rede detetou comunicações RDP (Remote Desktop Protocol) de saída anómalas. Especificamente, os dados de rede de amostragem mostram um elevado número de ligações de saída a partir da máquina, que é considerada anormal para este ambiente. Esta atividade pode indicar que a máquina foi comprometida e que, agora, está a ser utilizada força bruta externa nos pontos finais RDP. Tenha em atenção que este tipo de atividade, possivelmente, pode fazer com que o IP seja sinalizados como malicioso por entidades externas.
* **Atividade de rede SSH de entrada suspeita**: a análise de tráfego de rede detetou comunicações SSH de entrada anómalas. Especificamente, os dados de rede de amostragem mostram um elevado número de ligações à máquina, que é considerada anormal para este ambiente. Esta atividade pode indicar uma tentativa de força bruta no ponto de final SSH.
* **Atividade de rede SSH de entrada suspeita a partir de várias origens**: a análise de tráfego de rede detetou comunicações SSH de entrada anómalas. Especificamente, os dados de rede de amostragem mostram a ligação de IPs exclusivos à máquina, que é considerada anormal para este ambiente. Esta atividade pode indicar uma tentativa de força bruta no ponto de final SSH a partir de vários anfitriões (Botnet).
* **Atividade de rede SSH de saída suspeita**: a análise de tráfego de rede detetou comunicações SSH de saída anómalas. Especificamente, os dados de rede de amostragem mostram um elevado número de ligações de saída a partir da máquina, que é considerada anormal para este ambiente. Esta atividade pode indicar que a máquina foi comprometida e que, agora, está a ser utilizada força bruta externa nos pontos finais SSH. Tenha em atenção que este tipo de atividade, possivelmente, pode fazer com que o IP seja sinalizados como malicioso por entidades externas.
* **Atividade de rede SSH de saída suspeita para vários destinos**: a análise de tráfego de rede detetou comunicações SSH de saída anómalas a para vários destinos. Especificamente, os dados de rede de amostragem mostram a máquina a ligar-se a IPs exclusivos, que é considerada anormal para este ambiente. Esta atividade pode indicar que a máquina foi comprometida e que, agora, está a ser utilizada força bruta externa nos pontos finais SSH. Tenha em atenção que este tipo de atividade, possivelmente, pode fazer com que o IP seja sinalizados como malicioso por entidades externas.
* **Comunicação de rede com uma máquina maliciosa detetada**: a análise de tráfego de rede indica que a máquina comunicou com o que é possivelmente a um centro de Comando e de Controlo.
* **Possíveis máquinas comprometidas detetadas**: a análise de tráfego de rede detetou atividade de saída, que pode indicar que está a funcionar como parte de um botnet. A análise é baseada em IPs acedidos pelo recurso juntamente com registos DNS públicos.


## <a name="sql-database-and-sql-data-warehouse-analysis"></a>Base de Dados SQL e SQL Data Warehouse Analysis

A análise de recursos do Centro de Segurança concentra-se em serviços de PaaS (plataforma como serviço), como a integração com a funcionalidade [Deteção de Ameaças da Base de Dados SQL do Azure](../sql-database/sql-database-threat-detection.md) e o Azure SQL Data Warehouse. A Deteção de Ameaças do SQL deteta atividades anómalas que indiquem tentativas invulgares e potencialmente prejudiciais de acesso ou exploração de bases de dados e aciona os seguintes alertas:

* **Vulnerabilidade a Injeção de SQL**: este alerta é acionado quando uma aplicação gera uma instrução SQL defeituosa na base de dados. Tal pode indicar uma possível vulnerabilidade a ataques de injeção de SQL. Existem dois motivos possíveis para a geração de uma instrução defeituosa:
    * Um defeito no código da aplicação que constitui a instrução SQL defeituosa
    * O código de aplicação ou os procedimentos armazenados não saneiam a entrada de utilizador ao criar a instrução SQL defeituosa, o que pode ser explorado para Injeção SQL
* **Potencial injeção SQL**: este alerta é acionado quando uma exploração ativa ocorre contra uma vulnerabilidade de aplicação identificada para a injeção SQL. Significa que o atacante está a tentar injetar instruções SQL maliciosas através dos procedimentos armazenados ou código de aplicação com vulnerabilidade.
* **Acesso de uma localização invulgar**: este alerta é acionado quando ocorre uma alteração no padrão de acesso ao servidor SQL, no qual alguém iniciou sessão no servidor SQL a partir de uma localização geográfica invulgar. Em alguns casos, o alerta deteta uma ação legítima (uma nova manutenção de programador ou aplicação). Noutros casos, o alerta deteta uma ação maliciosa (ex-funcionário, atacante externo).
* **Acesso de um centro de dados do Azure invulgar**: este alerta é acionado quando ocorre uma alteração no padrão de acesso ao servidor SQL, no qual alguém iniciou sessão no servidor SQL a partir de um centro de dados do Azure invulgar que foi visto neste servidor durante o período recente. Em alguns casos, o alerta deteta uma ação legítima (a sua nova aplicação no Azure, Power BI, Editor de Consultas SQL do Azure). Noutros casos, o alerta deteta uma ação maliciosa de um recurso/serviço do Azure (ex-funcionário, atacante externo).
* **Acesso de um principal invulgar**: este alerta é acionado quando ocorre uma alteração no padrão de acesso ao servidor SQL, no qual alguém iniciou sessão no servidor SQL com recurso a um principal (utilizador SQL) invulgar. Em alguns casos, o alerta deteta uma ação legítima (nova aplicação, manutenção de programador). Noutros casos, o alerta deteta uma ação maliciosa (ex-funcionário, atacante externo).
* **Acesso de uma localização potencialmente prejudicial**: este alerta é acionado quando uma aplicação potencialmente prejudicial é utilizada para aceder à base de dados. Em alguns casos, o alerta deteta testes de penetração em ação. Noutros casos, o alerta deteta um ataque através de ferramentas de ataque comuns.
* **Credenciais SQL de força bruta**: este alerta é acionado quando existe um número anormalmente elevado de inícios de sessão falhados com diferentes credenciais. Em alguns casos, o alerta deteta testes de penetração em ação. Noutros casos, o alerta deteta um ataque de força bruta.

## <a name="contextual-information"></a>Informações contextuais
Durante uma investigação, os analistas precisam de contexto extra para chegar a um veredito sobre a natureza da ameaça e como resolvê-la.  Por exemplo, foi detetada uma anomalia na rede, mas sem compreender o que está a acontecer na rede ou ao recurso afetado, é muito difícil entender as ações que devem ser tomadas. Para ajudar nisso, um Incidente de Segurança pode incluir artefactos, eventos relacionados e informações que podem ajudar o investigador. A disponibilidade de informações adicionais varia com base no tipo de ameaça detetada e a configuração do seu ambiente, e não está disponível para todos os Incidentes de Segurança.

Se estiverem disponíveis informações adicionais, estas serão mostradas no Incidente de Segurança abaixo da lista de alertas. Podem estar contidas informações como:

- Eventos de limpeza de registos
- Dispositivo PNP ligado a partir de um dispositivo desconhecido
- Alertas não acionáveis
- Criação de nova conta
- Ficheiro descodificado com a ferramenta certutil 

![Alerta de acesso invulgar](./media/security-center-alerts-type/security-center-alerts-type-fig20.png)


## <a name="next-steps"></a>Passos Seguintes
Neste artigo, obteve informações sobre os vários tipos de alertas de segurança no Centro de Segurança. Para saber mais acerca do Centro de Segurança, consulte o seguinte:

* [Lidar com incidentes de segurança no Centro de Segurança do Azure](security-center-incident.md)
* [Capacidades de deteção do Centro de Segurança do Azure](security-center-detection-capabilities.md)
* [Guia de operações e planeamento do Centro de Segurança do Azure](security-center-planning-and-operations-guide.md)
* [Azure Security Center FAQ (FAQ do Centro de Segurança do Azure)](security-center-faq.md): encontre as perguntas mais frequentes acerca de como utilizar o serviço.
* [Blogue de segurança do Azure](http://blogs.msdn.com/b/azuresecurity/): encontre mensagens do blogue acerca da segurança e conformidade do Azure.
