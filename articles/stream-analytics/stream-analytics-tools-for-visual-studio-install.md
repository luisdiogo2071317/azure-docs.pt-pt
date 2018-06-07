---
title: Configurar as ferramentas do Azure Stream Analytics para o Visual Studio
description: Este artigo descreve os requisitos de instalação e como configurar as ferramentas do Azure Stream Analytics para Visual Studio.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/22/2018
ms.openlocfilehash: 94ed603990859d12f709e4a6121e3736221cf10a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34651183"
---
# <a name="install-azure-stream-analytics-tools-for-visual-studio"></a>Instalar as ferramentas do Azure Stream Analytics para o Visual Studio
Ferramentas do Azure Stream Analytics suportam Visual de Studio de 2017, 2015 ou 2013. Este artigo descreve como instalar e desinstalar as ferramentas.

Para obter mais informações sobre como utilizar as ferramentas, consulte [ferramentas do Stream Analytics para o Visual Studio](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio).

## <a name="install"></a>Instalar
### <a name="visual-studio-2017"></a>Visual Studio 2017
* Transferir [Visual Studio 2017, (15.3 ou superior)](https://www.visualstudio.com/). Enterprise (Ultimate/Premium), Professional e Community são suportadas as edições. Não é suportada a edição Express. 
* As ferramentas de análise de fluxo fazem parte do **programação do Azure** e **armazenamento de dados e processamento** cargas de trabalho no Visual Studio 2017. Ative uma destas duas cargas de trabalho como parte da sua instalação do Visual Studio.

Ativar o **armazenamento de dados e processamento** carga de trabalho conforme mostrado:

![Carga de trabalho de dados, processamento e de armazenamento é seleccionada](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2017-install-01.png)

Ativar o **programação do Azure** carga de trabalho conforme mostrado:

![Carga de trabalho de desenvolvimento do Azure é seleccionada](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2017-install-02.png)


### <a name="visual-studio-2013-2015"></a>Visual Studio 2013, 2015
* Instale Visual Studio 2015 ou Visual Studio 2013 atualização 4. Enterprise (Ultimate/Premium), Professional e Community são suportadas as edições. Não é suportada a edição Express. 
* Instalar o Microsoft Azure SDK para .NET versão 2.7.1 ou superior, utilizando o [instalador de plataforma Web](http://www.microsoft.com/web/downloads/platform.aspx).
* Instalar [ferramentas do Azure Stream Analytics para o Visual Studio](http://aka.ms/asatoolsvs).

## <a name="update"></a>Atualizar

### <a name="visual-studio-2017"></a>Visual Studio 2017
O lembrete de versão nova aparece na notificação do Visual Studio. 

### <a name="visual-studio-2013-and-visual-studio-2015"></a>Visual Studio 2013 e o Visual Studio 2015
As ferramentas de Stream Analytics instaladas para o Visual Studio verifica a existência de novas versões automaticamente. Siga as instruções na janela de pop-up para instalar a versão mais recente. 


## <a name="uninstall"></a>Desinstalar

### <a name="visual-studio-2017"></a>Visual Studio 2017
Faça duplo clique o instalador do Visual Studio e selecione **modificar**. Limpar o **do Azure Data Lake e as ferramentas de análise de fluxo** caixa de verificação a partir de **processamento e armazenamento de dados** carga de trabalho ou o **programação do Azure** carga de trabalho.

### <a name="visual-studio-2013-and-visual-studio-2015"></a>Visual Studio 2013 e o Visual Studio 2015
Aceda ao painel de controlo e desinstalar **Microsoft Azure Data Lake e Stream Analytics tools para Visual Studio**.





