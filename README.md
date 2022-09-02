########## Houston API Queries ###########################

These file has collection of Houston API queries.

################ Create Workspace#########################

```
mutation createWorkspace{{
    createWorkspace(
        label: "<workspace_name>"
        description: "<description>"
    )
    {{
        id
        label
        description
    }}
}}

```

################### Update Workspace ######################

```
mutation updateWorkspace{{
    updateWorkspace(
        workspaceUuid: <workspace_uuid>
        payload: {{
            description: "{<description_value>"
        }}  
    )
    {{
        id
        label
        description
    }}
}}
```

################## Delete Workspaces ##########################

```
mutation deleteWorkspace{{
    deleteWorkspace(
        workspaceUuid: <workspace_uuid>
    )
    {{
        id
        label
        description
    }}
}}

```

################## List of All Workspaces #####################


```

query workspace_list{{
    sysWorkspaces 
    {{
        id
        label
    }}
}}
```

############ Get Description of workspace ###########

```

query workspace{{
    workspace(
        workspaceUuid: <workspace_uuid>
    )
    {{
        description
    }}
}}

```

############### Create Deployment ###################

```

mutation createDeployment {{
    createDeployment(
        workspaceUuid: "<workspace_uuid>",
        type: "airflow",
        label: "<deployment_name>",
        executor: <CeleryExecutor/KubernetesExecutor>,
        airflowVersion: "<airflow_version>",
        cloudRole: "<google_service_account>",

        scheduler: {{
            replicas:<scheduler_cnt>,
            resources:{{
                limits:{{
                    cpu:<scheduler_cpu>,
                    memory:<scheduler_mem>
                }}
                requests:{{
                    cpu:<scheduler_cpu>,
                    memory:<scheduler_mem>
                }}
            }}
        }}
            
    	#-- start for workers config if executor is Celery
        workers: {{
            replicas:<worker_cnt>,
            terminationGracePeriodSeconds: <worker_termination_grace_period>,
            resources: {{
                limits:{{
                    cpu:<worker_cpu>,
                    memory:<worker_mem>
                }},
                requests:{{
                    cpu:<worker_cpu>,
                    memory:<worker_mem>
                }}
            }}
        }}
    	#-- end for workers config if executor is Celery
        
        webserver: {{
            resources:{{
                limits:{{
                    cpu:<webserver_cpu>,
                    memory:<webserver_mem>
                }}
                requests:{{
                    cpu:<webserver_cpu>,
                    memory:<webserver_mem>
                }}
            }}
        }}
    )
    {{
        id
        urls{{url}}
        releaseName
    }}
}}


```

############# Update Deployment ########################


```

mutation updateDeployment {{
    updateDeployment(
        deploymentUuid:"<deployment_uuid>",
        executor: <executor_name>,
        cloudRole: "<google_service_account>",
        
        scheduler: {{
            replicas:<scheduler_cnt>,
            resources:{{
                limits:{{
                    cpu:<scheduler_cpu>,
                    memory:<scheduler_mem>
                }}
            }}
        }}
            
	    #-- start for workers config if executor is Celery
        workers: {{
            replicas:<worker_cnt>,
            terminationGracePeriodSeconds: <worker_termination_grace_period>,
            resources: {{
                limits:{{
                    cpu:<worker_cpu>,
                    memory:<worker_mem>
                }}
            }}
        }}
        #-- end for workers config if executor is Celery
                
        webserver: {{
            resources:{{
                limits:{{
                    cpu:<webserver_cpu>,
                    memory:<webserver_mem>
                }}
            }}
        }}
    )
    {{
        id
        urls{{url}}
        releaseName
    }}
}}

```

################## Update Airflow Version on Deployment ####################

```

mutation upgrade_airflow(<deployment_uuid>: Uuid!, <airflow_version>: String!){{
    updateDeploymentAirflow (
        deploymentUuid:<deployment_uuid>
        desiredAirflowVersion:<airflow_version>
    )
    {{
        version
        status
    }}
}}

```

############## All Deployments Configurations under Workspace ##########

```

query listdeployment_config{{
    workspaceDeployments (
        workspaceUuid:<workspace_uuid>
    )
    {{
        id,
        releaseName,
        label,
        description,
        airflowVersion,
        executor,
        config
    }}  
}}
```

## Delete Deployment
```
mutation deldeployment{{
    deleteDeployment (
        deploymentUuid:<deployment_uuid>
    )
    {{
        label
    }}
}}

```

################### Update Extra AU in Deployment ######################

```

mutation modifydeployment{{
    updateDeployment(
        deploymentUuid: <deployment_uuid>
        payload: {{
            properties: {{
                extra_au: <extra_au_value>
            }}
        }}
    )
    {{
        id 
    }}
}}

```

############## Teams - List of All Teams ###############

```

query teams{{
    paginatedTeams(
        take: 200
    )
    {{
        count
        teams{{
            id
            name
        }}
    }}
}}

```

############ Invite User in Workspace ##############

```
mutation addUser(<email_id>: String!){{
    workspaceAddUser(
        email: <email_id>
        workspaceUuid: <workspace_uuid>
        role: <worksapce_role>
        bypassInvite: false
    )
    {{
        id
        users{{
            username
        }}
    }}
}}

```

######### Add Teams in Workspace #############

```

mutation add_teams_to_WS{{
    workspaceAddTeam (
        workspaceUuid: <workspace_uuid>
        teamUuid: <teamsuid>
        role: <workspace_role>
    )
    {{
        id
    }}
}}

```

######### Update Teams in Workspace #########


```

mutation update_teams_to_WS{{
    workspaceUpdateTeamRole (
        workspaceUuid: <workspace_uuid>
        teamUuid: <teamsuid>
        role: <workspace_role>
    )
}}

```

############ Delete Teams in Workspace ###############

```

mutation remove_teams_to_WS{{
    workspaceRemoveTeam (
        workspaceUuid: <workspace_uuid>
        teamUuid: <teamsuid>
    )
    {{
        id
    }}
}}

```

################ Add Teams in Deployment #################


```

mutation add_teams_to_Deploy{{
    deploymentAddTeamRole (
        deploymentUuid: <deployment_uuid>
        teamUuid: <teamsuid>
        role: <deployment_role>
    )
    {{
        id
    }}
}}

```

######## Update Teams in Deployment ########

```

mutation update_teams_to_Deploy{{
    deploymentUpdateTeamRole (
        deploymentUuid: <deployment_uuid>
        teamUuid: <teamsuid>
        role: <deployment_role>
    )
    {{
        id
    }}
}}

```

########## Add email recipients in Deployment Email Alerts ###########

```

mutation deploymentAlertsUpdate{{
    deploymentAlertsUpdate (
        deploymentUuid: <deployment_uuid>
        alertEmails: <email_id>
    )
    {{
        alertEmails
    }}
}}

```

########## Remove Teams in Deployment ########

```


mutation remove_teams_to_Deploy{{
    deploymentRemoveTeamRole (
        deploymentUuid: <deployment_uuid>
        teamUuid: <teamsuid>
    )
    {{
        id
    }}
}}

```

######### List of All Teams in Workspace #########

```

query teams_list{{
    workspaceTeams (
        workspaceUuid: <workspace_uuid>
    )
    {{
        id
        name
    }}
}}

```

######## List of All Teams in Deployment ########


```

query deploymentTeams{{
    deploymentTeams (
        deploymentUuid: <deployment_uuid>
    )
    {{
        id
        name
    }}
}}


```

########### List All Teams with its role in Deployment #########

```

query Deployment {{
    deployment (
        where:
        {{
            id: "<deployment_uuid>"
        }}
    )
    {{
        id,roleBindings{{ 
            role team
            {{
                id
                name
            }}
        }}
    }}
}}

```

######### Create Workspace Service Account Key #############

```

mutation createWorkspaceServiceAccount{{
    createWorkspaceServiceAccount(
        label: "<name_of_workspace_viewer_key>"
        category: ""
        workspaceUuid: <workspace_uuid>
        role:<workspace_role>
    )
    {{
        id 
        label
        apiKey
    }}
}}

```

######### Get Workspace Service Account Key ##########


```

query workspaceServiceAccounts{{
    workspaceServiceAccounts(
        workspaceUuid: <workspace_uuid>
    )
    {{
        id 
        label
        apiKey
    }}
}}

```
