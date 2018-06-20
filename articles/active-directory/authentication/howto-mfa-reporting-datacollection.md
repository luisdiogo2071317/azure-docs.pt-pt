---
title: Recolha de dados de utilizador multi-factor Authentication do Azure
description: As informações que são utilizadas para ajudar a autenticar os utilizadores ao Azure multi-factor Authentication?
services: multi-factor-authentication
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.service: multi-factor-authentication
ms.workload: identity
ms.topic: article
ms.date: 05/01/2018
ms.openlocfilehash: 2281a35d1616aa88b0c646fb96cb9f95c3272536
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264373"
---
# <a name="azure-multi-factor-authentication-user-data-collection"></a>Recolha de dados de utilizador multi-factor Authentication do Azure

Este documento explica como localizar informações de utilizador recolhidas pelo servidor do Azure multi-factor Authentication (servidor MFA) e o MFA do Azure (baseado na nuvem) nos eventos que pretende removê-lo.

[!INCLUDE [gdpr-hybrid-note](../../../includes/gdpr-hybrid-note.md)]

## <a name="information-collected"></a>Informações recolhidas

Servidor MFA, a extensão de NPS e o Windows Server 2016 do Azure MFA adaptador AD FS recolhem e armazenam as seguintes informações para 90 dias.

Tentativas de autenticação (utilizadas para relatórios e resolução de problemas):

- Carimbo de data/hora
- Nome de utilizador
- Nome Próprio
- Apelido
- Endereço de E-mail
- Grupo de Utilizadores
- Método de autenticação (chamada telefónica, texto mensagem, aplicação móvel, OATH Token)
- Modo de chamada telefónica (padrão, PIN)
- Direção de mensagem de texto (unidirecional, bidirecional)
- Modo de mensagem de texto (OTP, OTP + PIN)
- Modo de aplicação móvel (padrão, PIN)
- Modo de Token OATH (padrão, PIN)
- Tipo de Autenticação
- Nome da Aplicação
- Indicativo de país de chamada primário
- Número de telefone principal chamada
- Extensão de chamada primário
- Chamada primária autenticada
- Resultado da chamada primário
- Indicativo de país de chamada de cópia de segurança
- Número de telefone de chamada de cópia de segurança
- Extensão de chamada de cópia de segurança
- Cópia de segurança chamada autenticada
- Resultado da chamada de cópia de segurança
- Em geral autenticado
- Resultado geral
- Resultados
- Autenticados
- Resultado
- Iniciar o endereço IP
- Dispositivos
- Token do dispositivo
- Tipo de Dispositivo
- Versão de aplicação móvel
- Versão do SO
- Resultado
- Verificação utilizada para notificação

Ativações (as tentativas para ativar uma conta na aplicação móvel da Microsoft Authenticator):
- Nome de utilizador
- Nome da Conta
- Carimbo de data/hora
- Obter Resultado de código de ativação
- Ativar êxito
- Ativar o erro
- Resultado do Estado de ativação
- Nome do dispositivo
- Tipo de Dispositivo
- Versão da Aplicação
- Token OATH ativada

Blocos de (utilizado para determinar o estado de bloqueado e para relatórios):

- Bloquear Timestamp
- Bloco de por nome de utilizador
- Nome de utilizador
- Indicativo do País
- Número de Telefone
- Número de telefone a formatados
- Extensão
- Extensão de raiz
- Bloqueado
- Motivo do Bloqueio
- Carimbo de conclusão
- Motivo de conclusão
- Bloqueio de Conta
- Alerta de Fraude
- Alerta de fraude não bloqueada
- Idioma

Omissões (utilizados para relatórios):

- Ignorar Timestamp
- Segundos de Omissão
- Ignorar por nome de utilizador
- Nome de utilizador
- Indicativo do País
- Número de Telefone
- Número de telefone a formatados
- Extensão
- Extensão de raiz
- Motivo da Omissão
- Carimbo de conclusão
- Motivo de conclusão
- Ignorar utilizado

Alterações (utilizadas para sincronizar as alterações do utilizador para o servidor MFA ou o AAD):

- Alterar Timestamp
- Nome de utilizador
- Novo Código de País
- Novo Número de Telefone
- Nova Extensão
- Novo código de país alternativo
- Novo número de telefone alternativo
- Nova extensão de cópia de segurança
- Novo PIN
- Exigir Alteração do PIN
- Token do dispositivo antigo
- Novo Token de Dispositivo

## <a name="gather-data-from-mfa-server"></a>Recolher dados do servidor MFA

Para o servidor MFA versão 8.0 ou superior, o seguinte processo permite aos administradores exportar todos os dados para os utilizadores:

- Inicie sessão no seu servidor MFA, navegue para o **utilizadores** , selecione o utilizador em questão e, no **editar** botão. Tirar capturas de ecrã (Alt-PrtScn) de cada separador para fornecer ao utilizador as respetivas definições atuais do MFA.
- Na linha de comandos do servidor MFA, execute o seguinte comando a alteração do caminho, de acordo com a instalação `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe export <username>` produzir um JSON formatado ficheiro.
- Os administradores também podem utilizar a operação do Web Service SDK GetUserGdpr como uma opção para exportar todas as MFA nuvem serviço as informações recolhidas para um determinado utilizador ou incorporar uma solução de relatórios maior.
- Pesquisa `C:\Program Files\Multi-Factor Authentication Server\Logs\MultiFactorAuthSvc.log` e quaisquer cópias de segurança para "<username>" (incluir as aspas na pesquisa) para localizar todas as instâncias do registo de utilizador que está a ser adicionadas ou alteradas.
   - Podem ser limitados (mas não eliminados) estes registos desmarcando **"Alterações do utilizador de início de sessão"** no UX de servidor MFA, a secção de registo, separador de ficheiros de registo.
   - Se estiver configurado syslog, e **"Alterações do utilizador de início de sessão"** estiver marcada na UX de servidor MFA, a secção de registo, o separador Syslog, em seguida, as entradas de registo podem ser reunidas a partir do syslog em vez disso.
- Ficheiros relativas à autenticação tentativas são consideradas operacional e duplicadas para as informações fornecidas utilizando a exportação de MultiFactorAuthGdpr.exe ou o SDK do serviço Web de registo de outras ocorrências do nome de utilizador no MultiFactorAuthSvc.log e outro servidor MFA GetUserGdpr.

## <a name="delete-data-from-mfa-server"></a>Eliminar dados do servidor MFA

Na linha de comandos do servidor MFA, execute o seguinte comando a alteração do caminho, de acordo com a instalação `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe delete <username>` eliminar todos os MFA nuvem serviço informações recolhidas para este utilizador.

- Forem eliminados dados incluídos na exportação em tempo real, mas poderá demorar até 30 dias de dados operacionais ou duplicados para completamente removida.
- Os administradores também podem utilizar a operação do Web Service SDK DeleteUserGdpr como uma opção para eliminar todas as MFA nuvem serviço as informações recolhidas para um determinado utilizador ou incorporar uma maior solução de relatórios.

## <a name="gather-data-from-nps-extension"></a>Recolher dados de extensão de NPS

Utilize o [Portal de privacidade do Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) para efetuar um pedido de exportação.

- Informações de MFA estão incluídas na exportação, o que pode demorar horas ou dias para concluir.
- As ocorrências do nome de utilizador nos AuthN/AzureMfa/AuthNOptCh, AuthZ/AzureMfa/AuthZAdminCh e registos de eventos de AuthZ/AzureMfa/AuthZOptCh são consideradas operacional e duplicadas para as informações fornecidas na exportação.

## <a name="delete-data-from-nps-extension"></a>Eliminar dados de extensão de NPS

Utilize o [Portal de privacidade do Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) para efetuar um pedido para a conta fechar para eliminar todas as MFA nuvem serviço informações recolhidas para este utilizador.

- Pode demorar até 30 dias de dados ser completamente removido.

## <a name="gather-data-from-windows-server-2016-azure-mfa-ad-fs-adapter"></a>Recolher dados a partir do Windows Server 2016 do Azure MFA adaptador AD FS

Utilize o [Portal de privacidade do Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) para efetuar um pedido de exportação. 

- Informações de MFA estão incluídas na exportação, o que pode demorar horas ou dias para concluir.
- As ocorrências do nome de utilizador nos registos de eventos de rastreio/depuração do AD FS (se estiver ativada) são consideradas operacional e duplicadas para as informações fornecidas na exportação.

## <a name="delete-data-from-windows-server-2016-azure-mfa-ad-fs-adapter"></a>Eliminar dados do Windows Server 2016 do Azure MFA adaptador AD FS

Utilize o [Portal de privacidade do Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) para efetuar um pedido para a conta fechar para eliminar todas as MFA nuvem serviço informações recolhidas para este utilizador.

- Pode demorar até 30 dias de dados ser completamente removido.

## <a name="gather-data-for-azure-mfa"></a>Recolher dados para o Azure MFA

Utilize o [Portal de privacidade do Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) para efetuar um pedido de exportação.

- Informações de MFA estão incluídas na exportação, o que pode demorar horas ou dias para concluir.

## <a name="delete-data-for-azure-mfa"></a>Eliminar dados de MFA do Azure

Utilize o [Portal de privacidade do Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) para efetuar um pedido para a conta fechar para eliminar todas as MFA nuvem serviço informações recolhidas para este utilizador.

- Pode demorar até 30 dias de dados ser completamente removido.

## <a name="next-steps"></a>Passos Seguintes

[Relatórios do servidor MFA](howto-mfa-reporting.md)
