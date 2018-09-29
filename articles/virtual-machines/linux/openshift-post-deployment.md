---
title: OpenShift no tarefas de pós-implementação do Azure | Documentos da Microsoft
description: Tarefas adicionais para depois de um cluster do OpenShift foi implementada.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: najoshi
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: ''
ms.author: haroldw
ms.openlocfilehash: d400512c2e96e0e24bbf965b2e201adf92ccbb0f
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434896"
---
# <a name="post-deployment-tasks"></a>Tarefas de pós-implementação

Depois de implementar um cluster do OpenShift, pode configurar itens adicionais. Este artigo abrange o seguinte:

- Como configurar o início de sessão único com o Azure Active Directory (Azure AD)
- Como configurar o Log Analytics para monitorizar o OpenShift
- Como configurar métricas e registo

## <a name="configure-single-sign-on-by-using-azure-active-directory"></a>Configurar início de sessão único com o Azure Active Directory

Para utilizar o Azure Active Directory para autenticação, primeiro terá de criar um registo de aplicação do Azure AD. Este processo envolve dois passos: criar o registo de aplicações e a configuração de permissões.

### <a name="create-an-app-registration"></a>Criar um registo de aplicações

Estes passos utilizam a CLI do Azure para criar o registo de aplicações e a GUI (portal) para definir as permissões. Para criar o registo de aplicações, terá das seguintes cinco partes de informações:

- Nome a apresentar: o nome de registo de aplicação (por exemplo, OCPAzureAD)
- Home page: OpenShift URL da consola (por exemplo, https://masterdns343khhde.westus.cloudapp.azure.com:8443/console)
- URI identificador: OpenShift o URL da consola (por exemplo, https://masterdns343khhde.westus.cloudapp.azure.com:8443/console)
- URL de resposta: Principal do URL público e o nome do registo de aplicação (por exemplo, https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/OCPAzureAD)
- Palavra-passe: Palavra-passe segura (utilize uma palavra-passe segura)

O exemplo seguinte cria um registo de aplicações ao utilizar as informações anteriores:

```azurecli
az ad app create --display-name OCPAzureAD --homepage https://masterdns343khhde.westus.cloudapp.azure.com:8443/console --reply-urls https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/OCPAzureAD --identifier-uris https://masterdns343khhde.westus.cloudapp.azure.com:8443/console --password {Strong Password}
```

Se o comando for bem-sucedida, obtém uma saída JSON semelhante a:

```json
{
  "appId": "12345678-ca3c-427b-9a04-ab12345cd678",
  "appPermissions": null,
  "availableToOtherTenants": false,
  "displayName": "OCPAzureAD",
  "homepage": "https://masterdns343khhde.westus.cloudapp.azure.com:8443/console",
  "identifierUris": [
    "https://masterdns343khhde.westus.cloudapp.azure.com:8443/console"
  ],
  "objectId": "62cd74c9-42bb-4b9f-b2b5-b6ee88991c80",
  "objectType": "Application",
  "replyUrls": [
    "https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/OCPAzureAD"
  ]
}
```

Tome nota da propriedade appId devolvida do comando de um passo posterior.

No portal do Azure:

1.  Selecione **do Azure Active Directory** > **registo da aplicação**.
2.  Procurar o registo de aplicação (por exemplo, OCPAzureAD).
3.  Nos resultados, clique o registo de aplicações.
4.  Sob **configurações**, selecione **permissões obrigatórias**.
5.  Sob **permissões obrigatórias**, selecione **Add**.

  ![Registo de aplicações](media/openshift-post-deployment/app-registration.png)

6.  Clique no passo 1: Selecione a API e, em seguida, clique em **Windows Azure Active Directory (Microsoft.Azure.ActiveDirectory)**. Clique em **selecione** na parte inferior.

  ![API de selecione registo de aplicações](media/openshift-post-deployment/app-registration-select-api.png)

7.  No passo 2: Selecione as permissões, selecione **iniciar sessão e ler o perfil de utilizador** sob **permissões delegadas**e, em seguida, clique em **selecionar**.

  ![Acesso de registo de aplicações](media/openshift-post-deployment/app-registration-access.png)

8.  Selecione **Done** (Concluído).

### <a name="configure-openshift-for-azure-ad-authentication"></a>Configurar o OpenShift para autenticação do Azure AD

Para configurar o OpenShift para utilizar o Azure AD como fornecedor de autenticação, o ficheiro de /etc/origin/master/master-config.yaml tem de ser editado em todos os nós principais.

Encontre o ID do inquilino com o seguinte comando da CLI:

```azurecli
az account show
```

No ficheiro yaml, encontre as seguintes linhas:

```yaml
oauthConfig:
  assetPublicURL: https://masterdns343khhde.westus.cloudapp.azure.com:8443/console/
  grantConfig:
    method: auto
  identityProviders:
  - challenge: true
    login: true
    mappingMethod: claim
    name: htpasswd_auth
    provider:
      apiVersion: v1
      file: /etc/origin/master/htpasswd
      kind: HTPasswdPasswordIdentityProvider
```

Insira as seguintes linhas imediatamente após as linhas anteriores:

```yaml
  - name: <App Registration Name>
    challenge: false
    login: true
    mappingMethod: claim
    provider:
      apiVersion: v1
      kind: OpenIDIdentityProvider
      clientID: <appId>
      clientSecret: <Strong Password>
      claims:
        id:
        - sub
        preferredUsername:
        - unique_name
        name:
        - name
        email:
        - email
      urls:
        authorize: https://login.microsoftonline.com/<tenant Id>/oauth2/authorize
        token: https://login.microsoftonline.com/<tenant Id>/oauth2/token
```

Encontre o ID do inquilino com o seguinte comando da CLI: ```az account show```

Reinicie os serviços de mestres de OpenShift em todos os nós principais:

**OpenShift Origin**

```bash
sudo systemctl restart origin-master-api
sudo systemctl restart origin-master-controllers
```

**OpenShift Container Platform (OCP) com vários modelos de estrutura mestres**

```bash
sudo systemctl restart atomic-openshift-master-api
sudo systemctl restart atomic-openshift-master-controllers
```

**OpenShift Container Platform com uma única mestra**

```bash
sudo systemctl restart atomic-openshift-master
```

Na consola do OpenShift, verá agora duas opções para a autenticação: htpasswd_auth e [registo da aplicação].

## <a name="monitor-openshift-with-log-analytics"></a>OpenShift de monitor com o Log Analytics

Para monitorizar o OpenShift com o Log Analytics, pode utilizar uma das duas opções: instalação de agente do OMS no anfitrião de VM ou do contentor OMS. Este artigo fornece instruções para implementar o contentor de OMS.

## <a name="create-an-openshift-project-for-log-analytics-and-set-user-access"></a>Criar um projeto do OpenShift para o Log Analytics e definir o acesso de utilizador

```bash
oadm new-project omslogging --node-selector='zone=default'
oc project omslogging
oc create serviceaccount omsagent
oadm policy add-cluster-role-to-user cluster-reader system:serviceaccount:omslogging:omsagent
oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent
```

## <a name="create-a-daemon-set-yaml-file"></a>Criar um ficheiro de yaml de conjunto de daemon

Crie um ficheiro denominado ocp-omsagent.yml:

```yaml
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  name: oms
spec:
  selector:
    matchLabels:
      name: omsagent
  template:
    metadata:
      labels:
        name: omsagent
        agentVersion: 1.4.0-45
        dockerProviderVersion: 10.0.0-25
    spec:
      nodeSelector:
        zone: default
      serviceAccount: omsagent
      containers:
      - image: "microsoft/oms"
        imagePullPolicy: Always
        name: omsagent
        securityContext:
          privileged: true
        ports:
        - containerPort: 25225
          protocol: TCP
        - containerPort: 25224
          protocol: UDP
        volumeMounts:
        - mountPath: /var/run/docker.sock
          name: docker-sock
        - mountPath: /etc/omsagent-secret
          name: omsagent-secret
          readOnly: true
        livenessProbe:
          exec:
            command:
              - /bin/bash
              - -c
              - ps -ef | grep omsagent | grep -v "grep"
          initialDelaySeconds: 60
          periodSeconds: 60
      volumes:
      - name: docker-sock
        hostPath:
          path: /var/run/docker.sock
      - name: omsagent-secret
        secret:
         secretName: omsagent-secret
````

## <a name="create-a-secret-yaml-file"></a>Criar um ficheiro yaml secreta

Para criar o ficheiro yaml secreta, precisa de duas informações: ID de área de trabalho do Log Analytics e a chave de partilhada do Log Analytics área de trabalho. 

Segue-se um ficheiro de ocp-secret.yml de exemplo: 

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: omsagent-secret
data:
  WSID: wsid_data
  KEY: key_data
```

Replace wsid_data com o Base64 codificado por ID de área de trabalho do Log Analytics. Em seguida, substitua key_data o codificada em Base64 Log Analytics área de trabalho chave partilhada.

```bash
wsid_data='11111111-abcd-1111-abcd-111111111111'
key_data='My Strong Password'
echo $wsid_data | base64 | tr -d '\n'
echo $key_data | base64 | tr -d '\n'
```

## <a name="create-the-secret-and-daemon-set"></a>Criar um conjunto de segredo e o daemon

Implemente o ficheiro do segredo:

```bash
oc create -f ocp-secret.yml
```

Implemente o conjunto de daemon de agente do OMS:

```bash
oc create -f ocp-omsagent.yml
```

## <a name="configure-metrics-and-logging"></a>Configurar métricas e registo

O modelo Azure Resource Manager para o OpenShift Container Platform fornece parâmetros de entrada para ativar as métricas e registo. A oferta do Marketplace do OpenShift Container Platform e o modelo do Resource Manager do OpenShift Origin não.

Se utilizou o modelo de Gestor de recursos de OCP e métricas e registo não estavam habilitado no momento da instalação ou se tiver utilizado a oferta do OCP Marketplace, pode ser facilmente ative estes após o fato. Se estiver a utilizar o modelo OpenShift Origin Resource Manager, algum pré-trabalho de é necessário.

### <a name="openshift-origin-template-pre-work"></a>Trabalho de pré-lançamento de modelo do OpenShift Origin

1. SSH para o primeiro nó mestre através da porta 2200.

   Exemplo:

   ```bash
   ssh -p 2200 clusteradmin@masterdnsixpdkehd3h.eastus.cloudapp.azure.com 
   ```

2. Edite o ficheiro de /etc/ansible/hosts e adicione as seguintes linhas depois da secção de fornecedor de identidade (# ativar HTPasswdPasswordIdentityProvider):

   ```yaml
   # Setup metrics
   openshift_hosted_metrics_deploy=false
   openshift_metrics_cassandra_storage_type=dynamic
   openshift_metrics_start_cluster=true
   openshift_metrics_hawkular_nodeselector={"type":"infra"}
   openshift_metrics_cassandra_nodeselector={"type":"infra"}
   openshift_metrics_heapster_nodeselector={"type":"infra"}
   openshift_hosted_metrics_public_url=https://metrics.$ROUTING/hawkular/metrics

   # Setup logging
   openshift_hosted_logging_deploy=false
   openshift_hosted_logging_storage_kind=dynamic
   openshift_logging_fluentd_nodeselector={"logging":"true"}
   openshift_logging_es_nodeselector={"type":"infra"}
   openshift_logging_kibana_nodeselector={"type":"infra"}
   openshift_logging_curator_nodeselector={"type":"infra"}
   openshift_master_logging_public_url=https://kibana.$ROUTING
   ```

3. Substitua a cadeia utilizada para a opção de openshift_master_default_subdomain no mesmo ficheiro /etc/ansible/hosts $ROUTING.

### <a name="azure-cloud-provider-in-use"></a>Fornecedor de Cloud do Azure em utilização

No primeiro nó mestre (origem) ou o nó de bastion (OCP), SSH com as credenciais fornecidas durante a implementação. Emita o seguinte comando:

```bash
ansible-playbook $HOME/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook $HOME/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True \
-e openshift_hosted_logging_storage_kind=dynamic
```

### <a name="azure-cloud-provider-not-in-use"></a>Fornecedor de Cloud do Azure não está em utilização

No primeiro nó mestre (origem) ou o nó de bastion (OCP), SSH com as credenciais fornecidas durante a implementação. Emita o seguinte comando:

```bash
ansible-playbook $HOME/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True 

ansible-playbook $HOME/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True 
```

## <a name="next-steps"></a>Passos Seguintes

- [Introdução ao OpenShift Container Platform](https://docs.openshift.com/container-platform/3.6/getting_started/index.html)
- [Introdução ao OpenShift Origin](https://docs.openshift.org/latest/getting_started/index.html)
