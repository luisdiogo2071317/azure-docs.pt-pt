---
title: Referência de configurações de roaming do Windows 10 | Documentos da Microsoft
description: Uma lista completa de todas as definições que serão movidas ou uma cópia de segurança no Windows 10.
services: active-directory
keywords: roaming de estado empresarial, cloud do windows
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: curtand
ms.component: devices
ms.assetid: 17cffc3e-2928-4235-91f7-a685bd6bdcbf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: markvi
ms.openlocfilehash: 45bf554e457e48704684fe8ac3b32c29f3f646b2
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2019
ms.locfileid: "54189289"
---
# <a name="windows-10-roaming-settings-reference"></a>Referência das definições de roaming do Windows 10
Segue-se uma lista completa de todas as definições que serão movidas ou uma cópia de segurança no Windows 10. 

## <a name="devices-and-endpoints"></a>Dispositivos e os pontos finais
Consulte a tabela seguinte para obter um resumo dos dispositivos e tipos de contas que são suportados pela cópia de segurança, a sincronização e restaurar o framework no Windows 10.

| Tipo de conta e operação | Ambiente de trabalho | Telemóvel |
| --- | --- | --- |
| O Azure Active Directory: sincronização |Sim |Não |
| O Azure Active Directory: cópia de segurança/restauro |Não |Não |
| Conta Microsoft: sincronização |Sim |Sim |
| Conta Microsoft: cópia de segurança/restauro |Não |Sim |

## <a name="what-is-backup"></a>O que é a cópia de segurança?
Definições do Windows em geral, sincronizar por predefinição, mas algumas definições são apenas uma cópia de segurança, como a lista de aplicações instaladas num dispositivo. Cópia de segurança é para dispositivos móveis apenas e não está atualmente disponíveis para utilizadores de Roaming de estado empresarial. Cópia de segurança utiliza uma conta Microsoft e armazena as definições e dados da aplicação no OneDrive. Se um utilizador desative a sincronização no dispositivo com a aplicação de definições, os dados de aplicativo que normalmente se sincroniza ficam apenas cópia de segurança. Dados de cópia de segurança só podem ser acedidos através da operação de restauro durante a primeira experiência de execução de um novo dispositivo. As cópias de segurança podem ser desativadas através das definições de dispositivo e podem ser geridas e eliminação através da conta do OneDrive do utilizador.

## <a name="windows-settings-overview"></a>Descrição geral das definições do Windows
Os seguintes grupos de definições estão disponíveis para os utilizadores finais Ativar/desativar a sincronização de definições em dispositivos Windows 10.

* Tema: fundo, o mosaico de utilizador, a posição de barra de tarefas, etc. 
* Configurações do Internet Explorer: histórico de navegação digitado URLs, Favoritos, etc. 
* Palavras-passe: Gestor de credenciais do Windows, incluindo perfis Wi-Fi 
* Preferências de idioma: dicionário de ortografia, definições de idioma do sistema 
* Facilidade de acesso: Narrador, teclado no ecrã, Lupa 
* Outras definições do Windows: veja os detalhes de definições do Windows
* Definição do browser Microsoft Edge: Favoritos do Microsoft Edge, lista de leitura e outras definições

![](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-syncyoursettings.png)

Sincronização de grupo (Favoritos, lista de leitura) de definição de browser Microsoft Edge pode ser ativado ou desativado pelos usuários finais através do browser Microsoft Edge opção de menu de definições.

![](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-edge.png)

Para Windows 10 versão 1803 ou posterior, Internet Explorer grupo de definições (Favoritos, digitado URLs) a sincronização pode ser ativada ou desabilitada pelos usuários finais por meio da opção de menu de configurações do Internet Explorer. 

![](./media/enterprise-state-roaming-windows-settings-reference/active-directory-enterprise-state-roaming-ie.png)

## <a name="windows-settings-details"></a>Detalhes das definições do Windows
Na tabela a seguir, outras entradas na coluna grupo de definições de refere-se às definições que podem ser desativadas acedendo às definições > contas > sincronizar as definições > definições Other Windows. 

Entradas internas na coluna grupo de definições de consultar as definições e aplicações que só podem ser desativadas de sincronização dentro da aplicação em si ou ao desativar a sincronização para todo o dispositivo com a gestão de dispositivos móveis (MDM) ou definições de política de grupo.
As definições que não são carregados ou sincronização não irão pertencer a um grupo.

| Definições | Ambiente de trabalho | Telemóvel | Grupo |
| --- | --- | --- | --- |
| **Contas**: imagem da conta |Sincronização |X |Tema |
| **Contas**: outras definições de conta |X |X | |
| **Advanced banda larga móvel**: Nome de rede (permite a deteção automática de hotspots de Wi-Fi móveis através de Bluetooth) de compartilhamento de conexão de Internet |X |X |Palavras-passe |
| **Dados da aplicação**: aplicações individuais, podem sincronizar dados |cópia de segurança de sincronização |cópia de segurança de sincronização |Interno |
| **Lista de aplicações**: lista de aplicações instaladas |X |cópia de segurança |Outros |
| **Bluetooth**: todas as definições de Bluetooth |X |X | |
| **Linha de comandos**: Definições de "Predefinições" prompts de comando |Sincronização |X |Interno |
| **credenciais**: Cacifo de credenciais |Sincronização |Sincronização |palavra-passe |
| **Data, hora e região**: tempo automática (sincronização de hora da Internet) |Sincronização |Sincronização |language |
| **Data, hora e região**: relógio de 24 horas |Sincronização |X |language |
| **Data, hora e região**: data e hora |Sincronização |X |language |
| **Data, hora e região**: fuso horário | |X |language |
| **Data, hora e região**: horário de Verão |Sincronização |X |language |
| **Data, hora e região**: país/região |Sincronização |X |language |
| **Data, hora e região**: primeiro dia da semana |Sincronização |X |language |
| **Data, hora e região**: formato de região (local) |Sincronização |X |language |
| **Data, hora e região**: data de curta |Sincronização |X |language |
| **Data, hora e região**: data |Sincronização |X |language |
| **Data, hora e região**: curto período de tempo |Sincronização |X |language |
| **Data, hora e região**: longo tempo |Sincronização |X |language |
| **Personalização do ambiente de trabalho**: tema de área de trabalho (em segundo plano, cor de sistema, sons de sistema padrão, proteção de tela) |Sincronização |X |Tema |
| **Personalização do ambiente de trabalho**: imagem de fundo de apresentação de slides |Sincronização |X |Tema |
| **Personalização do ambiente de trabalho**: definições de barra de tarefas (posição, de ocultação automática, etc.) |Sincronização |X |Tema |
| **Personalização do ambiente de trabalho**: esquema do ecrã Iniciar |X |cópia de segurança | |
| **Dispositivos**: estabeleceu ligação de impressoras compartilhadas |X |X |outro |
| **Browser Microsoft Edge**: lista de leitura |Sincronização |Sincronização |Interno |
| **Browser Microsoft Edge**: Favoritos |Sincronização |Sincronização |Interno |
| **Browser Microsoft Edge**: principais sites <sup> [[1]](#footnote-1)</sup> |Sincronização |Sincronização |Interno |
| **Browser Microsoft Edge**: digitado URLs <sup> [[1]](#footnote-1)</sup> |Sincronização |Sincronização |Interno |
| **Browser Microsoft Edge**: as definições da barra de Favoritos <sup> [[1]](#footnote-1)</sup> |Sincronização |Sincronização |Interno |
| **Browser Microsoft Edge**: Mostrar botão de início <sup> [[1]](#footnote-1)</sup> |Sincronização |Sincronização |Interno |
| **Browser Microsoft Edge**: bloquear pop-ups <sup> [[1]](#footnote-1)</sup> |Sincronização |Sincronização |Interno |
| **Browser Microsoft Edge**: me perguntam o que fazer com cada download <sup> [[1]](#footnote-1)</sup> |Sincronização |Sincronização |Interno |
| **Browser Microsoft Edge**: oferta para guardar palavras-passe <sup> [[1]](#footnote-1)</sup> |Sincronização |Sincronização |Interno |
| **Browser Microsoft Edge**: envio não monitorizar os pedidos <sup> [[1]](#footnote-1)</sup> |Sincronização |Sincronização |Interno |
| **Browser Microsoft Edge**: guardar entradas do formulário <sup> [[1]](#footnote-1)</sup> |Sincronização |Sincronização |Interno |
| **Browser Microsoft Edge**: Mostrar sugestões de pesquisa e o site enquanto digito <sup> [[1]](#footnote-1)</sup> |Sincronização |Sincronização |Interno |
| **Browser Microsoft Edge**: a preferência de cookies <sup> [[1]](#footnote-1)</sup> |Sincronização |Sincronização |Interno |
| **Browser Microsoft Edge**: permitir que os sites guardar licenças de suporte de dados protegidos no meu dispositivo <sup> [[1]](#footnote-1)</sup> |Sincronização |Sincronização |Interno |
| **Browser Microsoft Edge**: a definição de leitor de tela <sup> [[1]](#footnote-1)</sup> |Sincronização |Sincronização |Interno |
| **Alto contraste**: Ativada ou desativada |Sincronização |X |Facilidade de acesso |
| **Alto contraste**: Configurações de tema |Sincronização |X |Facilidade de acesso |
| **Internet Explorer**: Abra separadores (URL e título) |Sincronização |Sincronização |Internet Explorer |
| **Internet Explorer**: lista de leitura |Sincronização |Sincronização |Internet Explorer |
| **Internet Explorer**: URLs de tipos |Sincronização |Sincronização |Internet Explorer |
| **Internet Explorer**: histórico de navegação |Sincronização |Sincronização |Internet Explorer |
| **Internet Explorer**: Favoritos |Sincronização |Sincronização |Internet Explorer |
| **Internet Explorer**: excluídos URLs |Sincronização |Sincronização |Internet Explorer |
| **Internet Explorer**: páginas iniciais |Sincronização |Sincronização |Internet Explorer |
| **Internet Explorer**: sugestões de domínio |Sincronização |Sincronização |Internet Explorer |
| **Teclado**: os utilizadores podem Ativar/desativar teclado virtual |Sincronização |X |Facilidade de acesso |
| **Teclado**: Ativar o adesivo Sim (desativado por predefinição) |Sincronização |X |Facilidade de acesso |
| **Teclado**: Ativar as chaves de filtro (desativado por predefinição) |Sincronização |X |Facilidade de acesso |
| **Teclado**: Ativar as chaves de alternância (desativado por predefinição) |Sincronização |X |Facilidade de acesso |
| **Internet Explorer**: domínio idioma: Chinês (CHS) QWERTY - ativar a aprendizagem automática |Sincronização |X |Idioma |
| **Idioma**: CHS QWERTY - ativar Release candidate dinâmica classificação |Sincronização |X |Idioma |
| **Idioma**: CHS QWERTY - char-set chinês simplificado |Sincronização |X |Idioma |
| **Idioma**: CHS QWERTY - char-set, chinês tradicional |Sincronização |X |Idioma |
| **Idioma**: CHS QWERTY - pinyin difusa |Sincronização |cópia de segurança |Idioma |
| **Idioma**: CHS QWERTY - pares difusas |Sincronização |cópia de segurança |Idioma |
| **Idioma**: CHS QWERTY - pinyin completo |Sincronização |X |Idioma |
| **Idioma**: CHS QWERTY - pinyin duplo |Sincronização |X |Idioma |
| **Idioma**: CHS QWERTY - correção automática de leitura |Sincronização |X |Idioma |
| **Idioma**: CHS QWERTY - chave de C/E comutador, shift |Sincronização |X |Idioma |
| **Idioma**: CHS QWERTY - C/E mude a chave, Ctrl |Sincronização |X |Idioma |
| **Idioma**: CHS WUBI - modo de entrada de caractere único |Sincronização |X |Idioma |
| **Idioma**: CHS WUBI - mostrar os restantes codificação da Release candidate |Sincronização |X |Idioma |
| **Idioma**: CHS WUBI - emitir aviso sonoro quando a codificação de 4 é inválido |Sincronização |X |Idioma |
| **Idioma**: CHT Bopomofo - incluem CJK Ext-A |Sincronização |X |Idioma |
| **Idioma**: IME do japonês - preditiva palavras de digitação e personalizadas |Sincronização |Sincronização |Idioma |
| **Idioma**: Coreano (KOR) IME |X |X |Idioma |
| **Idioma**: reconhecimento de manuscrito |X |X |Idioma |
| **Idioma**: perfil de idioma |Sincronização |cópia de segurança |Idioma |
| **Idioma**: verificação ortográfica - erros de ortografia autocorreção e realçar |Sincronização |cópia de segurança |Idioma |
| **Idioma**: lista de teclados |Sincronização |cópia de segurança |Idioma |
| **Ecrã de bloqueio**: todas as definições de ecrã de bloqueio |X |X | |
| **Lupa**: ou desativar (Ativar/desativar principal) |X |X |Facilidade de acesso |
| **Lupa**: Ativar ou desativar a cor de inversão (desativado por predefinição) |Sincronização |X |Facilidade de acesso |
| **Lupa**: controlo - siga o foco do teclado |Sincronização |X |Facilidade de acesso |
| **Lupa**: controlo - siga o cursor do rato |Sincronização |X |Facilidade de acesso |
| **Lupa**: iniciar quando os utilizadores iniciam sessão (desativado por predefinição) |Sincronização |X |Facilidade de acesso |
| **Mouse**: alterar o tamanho do cursor do rato |Sincronização |X |outro |
| **Mouse**: alterar a cor do cursor do rato |Sincronização |X |outro |
| **Mouse**: todas as outras definições |X |X | |
| **O Narrador**: início rápido |Sincronização |X |Facilidade de acesso |
| **O Narrador**: os utilizadores podem alterar o Narrador falando argumento de venda |Sincronização |X |Facilidade de acesso |
| **O Narrador**: os utilizadores podem ativar ou desativar o Narrador sugestões de itens comuns de leitura (por predefinição) |Sincronização |X |Facilidade de acesso |
| **O Narrador**: os utilizadores podem ativar ou desativar se eles podem ouvir caracteres digitados (por predefinição) |Sincronização |X |Facilidade de acesso |
| **O Narrador**: os utilizadores podem ativar ou desativar se eles podem ouvir com tipos de palavras (por predefinição) |Sincronização |X |Facilidade de acesso |
| **O Narrador**: ter cursor de inserir o Narrador a seguir (por predefinição) |Sincronização |X |Facilidade de acesso |
| **O Narrador**: Ativar o realce visual do cursor Narrador (por predefinição) |Sincronização |X |Facilidade de acesso |
| **O Narrador**: reproduzir áudio (por predefinição) |Sincronização |X |Facilidade de acesso |
| **O Narrador**: Ativar as chaves do teclado de toque, quando levanta o dedo (desativado por predefinição) |Sincronização |X |Facilidade de acesso |
| **Facilidade de acesso**: defina a espessura do cursor piscando |Sincronização |X |Facilidade de acesso |
| **Facilidade de acesso**: remover as imagens em segundo plano (desativado por predefinição) |Sincronização |X |Facilidade de acesso |
| **Energia e suspensão**: todas as definições |X |X | |
| **Começar a personalização de tela**: acen cor (apenas telemóvel) |X |Sincronização |Tema |
| **Escrever**: dicionário de ortografia |Sincronização |cópia de segurança |Idioma |
| **Escrever**: autocorreção com erros ortográficos word |Sincronização |cópia de segurança |Idioma |
| **Escrever**: realçar palavras com erros ortográficos |Sincronização |cópia de segurança |Idioma |
| **Escrever**: Mostrar sugestões de texto enquanto digito |Sincronização |cópia de segurança |Idioma |
| **Escrever**: adicionar um espaço depois escolho uma sugestão de texto |Sincronização |cópia de segurança |Idioma |
| **Escrever**: adicionar um período após eu duplo toque a barra de espaço |Sincronização |cópia de segurança |Idioma |
| **Escrever**: em maiúsculas a primeira letra de cada sentença |Sincronização |cópia de segurança |Idioma |
| **Escrever**: utilizar todas as letras maiúsculas quando eu duplo toque tecla shift |Sincronização |cópia de segurança |Idioma |
| **Escrever**: reproduzir sons de chave, à medida que escreve |Sincronização |cópia de segurança |Idioma |
| **Escrever**: data individuálního nastavení Pro teclado de toque |Sincronização |cópia de segurança |Idioma |
| **Wi-Fi**: Perfis de Wi-Fi (apenas WPA) |Sincronização |Sincronização |Palavras-passe |

###### <a name="footnote-1"></a>Nota de rodapé 1
Versão de SO de atualização para criativos do Windows (criar 15063) mínima suportada. 

## <a name="next-steps"></a>Passos Seguintes

Para uma descrição geral, consulte [descrição geral de roaming de estado empresarial](enterprise-state-roaming-overview.md).

