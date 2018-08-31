---
title: Resolver problemas comuns do serviço Kubernetes do Azure
description: Saiba como resolver problemas comuns ao utilizar o Azure Kubernetes Service (AKS)
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: troubleshooting
ms.date: 08/13/2018
ms.author: saudas
ms.openlocfilehash: 9f082c5f198ebd7123058bd250d3fef55494d553
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43287544"
---
# <a name="aks-troubleshooting"></a>Resolução de problemas do AKS
Quando criar ou AKS Gestor de clusters, ocasionalmente, poderá encontrar problemas. Este artigo fornece detalhes sobre alguns problemas comuns e passos de resolução de problemas.

### <a name="in-general-where-do-i-find-information-about-debugging-kubernetes-issues"></a>Em geral, onde posso encontrar informações sobre a depuração de problemas do Kubernetes?

[Aqui] (https://kubernetes.io/docs/tasks/debug-application-cluster/troubleshooting/) é uma ligação oficial para resolução de problemas de clusters do kubernetes.
[Aqui](https://github.com/feiskyer/kubernetes-handbook/blob/master/en/troubleshooting/index.md) é uma hiperligação para um guia de resolução de problemas publicado por um engenheiro da Microsoft em torno de resolução de problemas de pods, nós, clusters, etc.

### <a name="i-am-getting-a-quota-exceeded-error-during-create-or-upgrade-what-should-i-do"></a>Estou recebendo um erro de quota foi excedida durante a criação ou atualização. O que devo fazer? 

Precisará solicitar núcleos [aqui](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

### <a name="what-is-the-max-pods-per-node-setting-for-aks"></a>O que é os máximos pods por definição de nó para o AKS?

Os pods máximos por nó são definidos como 30 por predefinição, se implementar um cluster do AKS no portal do Azure.
Os pods máximos por nó são definidos para 110 por predefinição, se implementar um cluster do AKS na CLI do Azure. (Certifique-se de que está a utilizar a versão mais recente da CLI do Azure). Essa configuração padrão pode ser alterada usando o – max-nós-por-comando de criação do sinalizador de pod no az aks.

### <a name="i-am-getting-insufficientsubnetsize-error-while-deploying-an-aks-cluster-with-advanced-networking-what-should-i-do"></a>Estou recebendo o erro de "insufficientSubnetSize" durante a implementação de um cluster do AKS com a rede avançada. O que devo fazer?

Na opção de VNET personalizado selecionada para a rede durante o AKS cria, CNI o Azure é utilizado para o IPAM. O número de nós num cluster do AKS pode ser qualquer número entre 1 e 100. Com base em 2) acima da sub-rede tamanho deve ser maior do que o produto do número de nós e o pod máximo por tamanho da sub-rede do nó > não de nós no cluster * máx. de pods por nó.

### <a name="my-pod-is-stuck-in-crashloopbackoff-mode-what-should-i-do"></a>Meu pod está bloqueada no modo de 'CrashLoopBackOff'. O que devo fazer?

Pode haver vários motivos para o pod bloqueados nesse modo. Pode desejar examinar a 
* Usando o pod em si `kubectl describe pod <pod-name>`
* Os registos de utilização  `kubectl log <pod-name>`

### <a name="i-am-trying-to-enable-rbac-on-an-existing-cluster-can-you-tell-me-how-i-can-do-that"></a>Estou a tentar ativar o RBAC num cluster existente. Pode me dizer como posso fazer isso?

Infelizmente, ativar RBAC em clusters existentes não é suportado neste momento. Terá de criar novos clusters explicitamente. Se utilizar a CLI, o RBAC está ativado por predefinição, ao passo que um botão de alternância para ativá-la está disponível no portal do AKS criar fluxo de trabalho.

### <a name="i-created-a-cluster-using-the-azure-cli-with-defaults-or-the-azure-portal-with-rbac-enabled-and-numerous-warnings-in-the-kubernetes-dashboard-the-dashboard-used-to-work-before-without-any-warnings-what-should-i-do"></a>Criei um cluster com a CLI do Azure com as predefinições ou o portal do Azure com o RBAC habilitado e vários avisos no dashboard do kubernetes. O dashboard utilizado a funcionar antes de sem quaisquer avisos. O que devo fazer?

O motivo para receber avisos no dashboard é que agora é habilitado com RBAC'ed e acesso a ele foi desabilitado por predefinição. Em geral, essa abordagem é considerada uma prática recomendada, uma vez que a exposição de predefinição do dashboard para todos os utilizadores do cluster pode levar a ameaças de segurança. Se pretender continuar a ativar o dashboard, siga este [blogue](https://pascalnaber.wordpress.com/2018/06/17/access-dashboard-on-aks-with-rbac-enabled/) para ativá-la.

### <a name="i-cant-seem-to-connect-to-the-dashboard-what-should-i-do"></a>Não consigo ligar ao dashboard. O que devo fazer?

A maneira mais fácil acesso ao seu serviço fora do cluster é executar o proxy de kubectl, que será proxy nos pedidos para o localhost a porta 8001 para o servidor de API do Kubernetes. A partir daí, o apiserver pode proxy ao seu serviço: http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/#! / nó? espaço de nomes = predefinido

Se não vir o dashboard do kubernetes, em seguida, verifique se o pod kube proxy está em execução no namespace kube system. Se não estiver no estado de execução, elimine o pod e ele será reiniciado.

### <a name="i-could-not-get-logs-using-kubectl-logs-or-cannot-connect-to-the-api-server-getting-the-error-from-server-error-dialing-backend-dial-tcp-what-should-i-do"></a>Eu não foi possível obter os registos com os registos de Kubectl ou não é possível ligar para o servidor de api ao obter o "erro do servidor: back-end de discagem de erro: marque o tcp...". O que devo fazer?

Certifique-se de que o padrão NSG não for modificado e a porta 22 está aberta para ligação ao servidor de API. Verifique se o pod tunnelfront está em execução no namespace kube system. Se não for, forçar a eliminação e ele será reiniciado.

### <a name="i-am-trying-to-upgrade-or-scale-and-am-getting-message-changing-property-imagereference-is-not-allowed-error--how-do-i-fix-this-issue"></a>Eu estou tentando atualizar ou dimensionar e estou a receber "message": "Alterar a propriedade 'imageReference' não é permitida." Erro.  Como corrigir este problema?

É possível que está a obter este erro porque modificou as etiquetas em nós de agente dentro do cluster do AKS. Modificar e eliminar as etiquetas e outras propriedades de recursos no grupo de recursos MC_ * podem levar a resultados inesperados. Modificação de recursos em MC_ * do cluster do AKS divide o SLO.


