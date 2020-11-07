- `az appservice plan create` [Ref for Deployment](https://docs.microsoft.com/en-us/azure/app-service/scripts/cli-deploy-github#sample-script)

1. `az webapp log config` - [Reference](https://docs.microsoft.com/en-us/cli/azure/webapp/log?view=azure-cli-latest#az-webapp-log-config)

- `--docker-container-logging`

2. `az webapp log tail` - to get a live trail of the logs [Ref](https://docs.microsoft.com/en-us/cli/azure/webapp/log?view=azure-cli-latest#az-webapp-log-tail)

3. `New-AzVM` - Azure Powershell command [Ref](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/tutorial-manage-vm)

4. `az webapp cors add` - [Ref](https://docs.microsoft.com/en-us/cli/azure/webapp/cors?view=azure-cli-latest)

5. `az cosmosdb create -n <name> -g <rg>` [Ref](https://docs.microsoft.com/en-us/azure/cosmos-db/scripts/cli/table/create?toc=/cli/azure/toc.json)

6. `az cosmosdb table create `

7. `az acr create --name myregistry --resource-group mygroup --sku standard --admin-enabled true`

8. `az acr credential show --name myregistry`

9. `az acr repository list --name myregistry`

10.

```
  az container create
    --resource-group mygroup
    --name myinstance
    --image myregistry.azurecr.io/myapp:latest
    --dns-name-label mydnsname
    --registry-username <username>
    --registry-password <password>

```

11. `az container show --resource-group mygroup --name myinstance --query ipAddress.fqdn`

12. Build container image from a Docker File: `az acr build --registry $ACR_NAME --image helloacrtasks:v1 .`

13. Run the following command in the Cloud Shell to enable the admin account on your registry. `az acr update -n $ACR_NAME --admin-enabled true`

14. `az acr credential show --name $ACR_NAME`

15. Get the IP address of the Azure container instance using the following command:

```

az container show
  --resource-group  learn-deploy-acr-rg
  --name acr-tasks
  --query ipAddress.ip
  --output table

```

16. Run the following command to replicate your registry to another region: `az acr replication create --registry $ACR_NAME --location japaneast`

17. View the container's logs to examine the output. To do so, run az container logs like this.

```
az container logs \
  --resource-group learn-deploy-aci-rg \
  --name mycontainer-restart-demo
```

18. UnSecure vs Secure Environment variables for Azure Containers

```

az container create \
  --resource-group learn-deploy-aci-rg \
  --name aci-demo \
  --image microsoft/azure-vote-front:cosmosdb \
  --ip-address Public \
  --location eastus \
  --environment-variables \
    COSMOS_DB_ENDPOINT=$COSMOS_DB_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_DB_MASTERKEY

az container create \
  --resource-group learn-deploy-aci-rg \
  --name aci-demo-secure \
  --image microsoft/azure-vote-front:cosmosdb \
  --ip-address Public \
  --location eastus \
  --secure-environment-variables \
    COSMOS_DB_ENDPOINT=$COSMOS_DB_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_DB_MASTERKEY
```

19. The az container attach command provides diagnostic information during container startup. Once the container has started, it also writes standard output and standard error streams to your local terminal.

```
az container attach \
  --resource-group learn-deploy-aci-rg \
  --name mycontainer

```

20. run the following az container exec command to start an interactive session on your container.

```
az container exec \
  --resource-group learn-deploy-aci-rg \
  --name mycontainer \
  --exec-command /bin/sh
```

21. Run the az monitor metrics list command to retrieve CPU usage information.

```
az monitor metrics list \
  --resource $CONTAINER_ID \
  --metric CPUUsage \
  --output table
```

22.

```

az acr task create
  --registry <container_registry_name>
  --name buildwebapp
  --image webimage
  --context https://github.com/MicrosoftDocs/mslearn-deploy-run-container-app-service.git
  --branch master
  --file Dockerfile
  --git-access-token <access_token>

```

23.

```
az webapp deployment source config --repo-url
                                   [--app-working-dir]
                                   [--branch]
                                   [--cd-account-create]
                                   [--cd-app-type {AspNet, AspNetCore, NodeJS, PHP, Python}]
                                   [--cd-project-url]
                                   [--git-token]
                                   [--ids]
                                   [--manual-integration]
                                   [--name]
                                   [--nodejs-task-runner {Grunt, Gulp, None}]
                                   [--private-repo-password]
                                   [--private-repo-username]
                                   [--python-framework {Bottle, Django, Flask}]
                                   [--python-version {Python 2.7.12 x64, Python 2.7.12 x86, Python 2.7.13 x64, Python 2.7.13 x86, Python 3.5.3 x64, Python 3.5.3 x86, Python 3.6.0 x64, Python 3.6.0 x86, Python 3.6.1 x86, Python 3.6.2 x64}]
                                   [--repository-type {externalgit, git, github, localgit, mercurial, vsts}]
                                   [--resource-group]
                                   [--slot]
                                   [--slot-swap]
                                   [--subscription]
                                   [--test]
```

24. Create AZ Service Bus using Powershell

```
New-AzServiceBusQueue
   [-ResourceGroupName] <String>
   [-Namespace] <String>
   [-Name] <String>
   [-EnablePartitioning <Boolean>]
   [-LockDuration <String>]
   [-AutoDeleteOnIdle <String>]
   [-DefaultMessageTimeToLive <String>]
   [-DuplicateDetectionHistoryTimeWindow <String>]
   [-DeadLetteringOnMessageExpiration <Boolean>]
   [-EnableBatchedOperations]
   [-EnableExpress <Boolean>]
   [-MaxDeliveryCount <Int32>]
   [-MaxSizeInMegabytes <Int64>]
   [-MessageCount <Int64>]
   [-RequiresDuplicateDetection <Boolean>]
   [-RequiresSession <Boolean>]
   [-SizeInBytes <Int64>]
   [-ForwardTo <String>]
   [-ForwardDeadLetteredMessagesTo <String>]
   [-DefaultProfile <IAzureContextContainer>]
   [-WhatIf]
   [-Confirm]
   [<CommonParameters>]
```

25. To create a VM, run the following set of commands:

```
az group create --name myResourceGroup --location eastus

az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image win2016datacenter \
    --admin-username azureuser

az vm open-port --port 80 --resource-group myResourceGroup --name myVM

```

26. Azure Blob copy using Azure CLI

```
az storage blob copy start --destination-blob
                           --destination-container
                           [--account-key]
                           [--account-name]
                           [--auth-mode {key, login}]
                           [--connection-string]
```

[Ref](https://docs.microsoft.com/en-us/cli/azure/storage/blob/copy?view=azure-cli-latest)

Same for Powershell

[Ref](https://docs.microsoft.com/en-us/powershell/module/az.storage/start-azstorageblobcopy?view=azps-5.0.0&viewFallbackFrom=azps-4.6.0)

```
Start-AzStorageBlobCopy
     [-SrcBlob] <String>
     -SrcContainer <String>
     -DestContainer <String>
     [-DestBlob <String>]
     [-PremiumPageBlobTier <PremiumPageBlobTier>]
     [-StandardBlobTier <String>]
```

27. Get Connection String for Azure Event Hub Namespace

`az eventhubs namespace authorization-rule keys list --resource-group dummyresourcegroup --namespace-name dummynamespace --name RootManageSharedAccessKey`

Get Connection String for Azure EventHub eventhub entity

`az eventhubs eventhub authorization-rule keys list --resource-group dummyresourcegroup --namespace-name dummynamespace --eventhub-name dummyeventhub --name RootManageSharedAccessKey`

28. Pull Container Logs

`az container logs --resource-group myResourceGroup --name mycontainer`

29. execute the az container attach command to attach your local console to the container's output streams:

`az container attach --resource-group myResourceGroup --name mycontainer`

30. An event grid topic provides a user-defined endpoint that you post your events to. The following example creates the custom topic in your resource group. Replace <your-topic-name> with a unique name for your custom topic. The custom topic name must be unique because it's represented by a DNS entry.

```
topicname=<your-topic-name>
az eventgrid topic create --name $topicname -l westus2 -g gridResourceGroup`
```

31. Before subscribing to the custom topic, let's create the endpoint for the event message. You create an event hub for collecting the events.

```
namespace=<unique-namespace-name>
hubname=demohub

az eventhubs namespace create --name $namespace --resource-group gridResourceGroup
az eventhubs eventhub create --name $hubname --namespace-name $namespace --resource-group gridResourceGroup
```

32. You subscribe to an event grid topic to tell Event Grid which events you want to track. The following example subscribes to the custom topic you created, and passes the resource ID of the event hub for the endpoint.

```
hubid=$(az eventhubs eventhub show --name $hubname --namespace-name $namespace --resource-group gridResourceGroup --query id --output tsv)
topicid=$(az eventgrid topic show --name $topicname -g gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --source-resource-id $topicid \
  --name subtoeventhub \
  --endpoint-type eventhub \
  --endpoint $hubid
```
