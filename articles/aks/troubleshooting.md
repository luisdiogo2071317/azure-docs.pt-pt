---
title: Resolver problemas comuns do serviço Kubernetes do Azure
description: Saiba como resolver problemas comuns ao utilizar o Azure Kubernetes Service (AKS)
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: troubleshooting
ms.date: 08/13/2018
ms.author: saudas
ms.openlocfilehash: fd3d1c464c6f2d4cbecd715db0689581ca141769
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/20/2018
ms.locfileid: "53654075"
---
# <a name="aks-troubleshooting"></a>Resolução de problemas do AKS

Quando criar ou gerir os clusters do Azure Kubernetes Service (AKS), ocasionalmente, poderá ter problemas. Este artigo fornece detalhes sobre alguns problemas comuns e passos de resolução de problemas.

## <a name="in-general-where-do-i-find-information-about-debugging-kubernetes-problems"></a>Em geral, onde posso encontrar informações sobre a depuração de problemas do Kubernetes?

Experimente o [guia oficial para resolução de problemas de clusters do Kubernetes](https://kubernetes.io/docs/tasks/debug-application-cluster/troubleshooting/).
Há também uma [guia de resolução de problemas](https://github.com/feiskyer/kubernetes-handbook/blob/master/en/troubleshooting/index.md), publicados por um engenheiro da Microsoft para resolução de problemas de pods, nós, clusters e outros recursos.

## <a name="im-getting-a-quota-exceeded-error-during-creation-or-upgrade-what-should-i-do"></a>Estou recebendo um erro de "quota excedida" durante a criação ou atualização. O que devo fazer? 

Precisa [pedir núcleos](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

## <a name="what-is-the-maximum-pods-per-node-setting-for-aks"></a>O que é a configuração máxima de pods por nó para o AKS?

A configuração de pods por nó máxima é 30 por predefinição, se implementar um cluster do AKS no portal do Azure.
A configuração de pods por nó máxima é 110 por predefinição, se implementar um cluster do AKS na CLI do Azure. (Certifique-se de que está a utilizar a versão mais recente da CLI do Azure). Essa configuração padrão pode ser alterada utilizando o `–-max-pods` sinalizar no `az aks create` comando.

## <a name="im-getting-an-insufficientsubnetsize-error-while-deploying-an-aks-cluster-with-advanced-networking-what-should-i-do"></a>Estou recebendo um erro de insufficientSubnetSize durante a implementação de um cluster do AKS com a rede avançada. O que devo fazer?

A opção de rede Virtual do Azure personalizada para funcionamento em rede durante a criação do AKS, a Interface de rede de contentor do Azure (CNI) é utilizado para gestão de endereços IP (IPAM). O número de nós num cluster do AKS pode ser qualquer número entre 1 e 100. Com base na secção anterior, o tamanho de sub-rede deve ser maior do que o produto do número de nós e os pods máximos por nó. A relação pode ser expresso desta forma: tamanho da sub-rede > número de nós no cluster * pods máximas por nó.

## <a name="my-pod-is-stuck-in-crashloopbackoff-mode-what-should-i-do"></a>Meu pod está bloqueada no modo de CrashLoopBackOff. O que devo fazer?

Pode haver vários motivos para o pod bloqueados nesse modo. Procurar em:

* O pod em si, ao utilizar `kubectl describe pod <pod-name>`.
* Os registos, utilizando `kubectl log <pod-name>`.

Para obter mais informações sobre como solucionar problemas de pod, consulte [depurar aplicativos](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application/#debugging-pods).

## <a name="im-trying-to-enable-rbac-on-an-existing-cluster-how-can-i-do-that"></a>Estou a tentar ativar o RBAC num cluster existente. Como posso fazer isso?

Infelizmente, ativar o controlo de acesso baseado em funções (RBAC) em clusters existentes não é suportada neste momento. Tem de criar explicitamente novos clusters. Se utilizar a CLI, RBAC está ativado por predefinição. Se utilizar o portal do AKS, um botão de alternância para ativar o RBAC está disponível no fluxo de trabalho de criação.

## <a name="i-created-a-cluster-with-rbac-enabled-by-using-either-the-azure-cli-with-defaults-or-the-azure-portal-and-now-i-see-many-warnings-on-the-kubernetes-dashboard-the-dashboard-used-to-work-without-any-warnings-what-should-i-do"></a>Criei um cluster com RBAC ativado ao utilizar a CLI do Azure com as predefinições ou o portal do Azure e, agora posso ver muitos avisos no dashboard do Kubernetes. O dashboard utilizado para funcionar sem quaisquer avisos. O que devo fazer?

O motivo para os avisos no dashboard é que o cluster está agora ativado com RBAC e acesso a ele foi desabilitado por predefinição. Em geral, essa abordagem é uma prática recomendada, uma vez que a exposição de predefinição do dashboard para todos os utilizadores do cluster pode levar a ameaças de segurança. Se pretender continuar a ativar o dashboard, siga os passos em [nesta mensagem de blogue](https://pascalnaber.wordpress.com/2018/06/17/access-dashboard-on-aks-with-rbac-enabled/).

## <a name="i-cant-connect-to-the-dashboard-what-should-i-do"></a>Não consigo ligar ao dashboard. O que devo fazer?

A maneira mais fácil acesso ao seu serviço fora do cluster é executar `kubectl proxy`, quais solicitações de proxies enviados para a localhost a porta 8001 para o servidor de API do Kubernetes. A partir daí, o servidor de API pode proxy ao seu serviço: `http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/#!/node?namespace=default`.

Se não vir o dashboard do Kubernetes, verifique se o `kube-proxy` pod está em execução `kube-system` espaço de nomes. Se não estiver num Estado de execução, elimine o pod e ele será reiniciado.

## <a name="i-cant-get-logs-by-using-kubectl-logs-or-i-cant-connect-to-the-api-server-im-getting-error-from-server-error-dialing-backend-dial-tcp-what-should-i-do"></a>Eu não é possível obter os registos ao utilizar o kubectl registos ou não consigo ligar ao servidor de API. Estou a receber "erro do servidor: back-end de discagem de erro: marque o tcp..." O que devo fazer?

Certifique-se de que o grupo de segurança de rede (NSG) padrão não é modificado e que a porta 22 está aberta para ligação ao servidor de API. Verifique se o `tunnelfront` pod está em execução `kube-system` espaço de nomes. Se não estiver, force a eliminação do pod e ele será reiniciado.

## <a name="im-trying-to-upgrade-or-scale-and-am-getting-a-message-changing-property-imagereference-is-not-allowed-error--how-do-i-fix-this-problem"></a>Eu estou tentando atualizar ou dimensionar e estou recebendo uma "mensagem: Erro não é permitido alterar a propriedade 'imageReference' ".  Como corrigir esse problema?

Poderá ser a obter este erro pois modificar as etiquetas em nós de agente dentro do cluster do AKS. Modificar e eliminar as etiquetas e outras propriedades de recursos no grupo de recursos MC_ * podem levar a resultados inesperados. Modificação de recursos sob o grupo MC_ * do AKS cluster divide o objetivo de nível de serviço (SLO).

## <a name="how-do-i-renew-the-service-principal-secret-on-my-aks-cluster"></a>Como posso renovar o segredo do principal de serviço no meu cluster AKS?

Por predefinição, os clusters do AKS são criadas com um principal de serviço com um tempo de expiração de um ano. Como perto da data de expiração, pode repor as credenciais para estender o principal de serviço para um período adicional de tempo.

O exemplo seguinte executa estas etapas:

1. Obtém o ID de principal de serviço do cluster utilizando o [show do az aks](/cli/azure/aks#az-aks-show) comando.
1. Lista o segredo de cliente do principal de serviço utilizando o [lista de credencial do az ad sp](/cli/azure/ad/sp/credential#az-ad-sp-credential-list).
1. Estende o principal de serviço durante um ano, outro, utilizando o [ad de az sp reposição de credencial](/cli/azure/ad/sp/credential#az-ad-sp-credential-reset) comando. O segredo de cliente do principal de serviço tem de permanecer o mesmo para o cluster do AKS para serem executados corretamente.

```azurecli
# Get the service principal ID of your AKS cluster.
sp_id=$(az aks show -g myResourceGroup -n myAKSCluster \
    --query servicePrincipalProfile.clientId -o tsv)

# Get the existing service principal client secret.
key_secret=$(az ad sp credential list --id $sp_id --query [].keyId -o tsv)

# Reset the credentials for your AKS service principal and extend for one year.
az ad sp credential reset \
    --name $sp_id \
    --password $key_secret \
    --years 1
```
