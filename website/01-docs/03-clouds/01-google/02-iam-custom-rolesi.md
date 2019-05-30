---
layout: page
title: IAM Custom Roles
permalink: /clouds/google/iam-custom-roles/
---

# IAM Custom Roles


Делаю!  
30.05.2019

<br/>

Взято отсюда:  
https://www.qwiklabs.com/focuses/1035?parent=catalog


<br/>

### List predefined roles

    $ gcloud iam roles list

<br/>

### Viewing the available permissions for a resource

    $ echo $DEVSHELL_PROJECT_ID
    qwiklabs-gcp-f9fcfa2b4bf50fb7

<br/>

    $ gcloud iam list-testable-permissions //cloudresourcemanager.googleapis.com/projects/$DEVSHELL_PROJECT_ID

<br/>

### Getting the role metadata

    $ gcloud iam roles describe roles/viewer
    $ gcloud iam roles describe roles/editor

<br/>

### Viewing the grantable roles on resources

    $ gcloud iam list-grantable-roles //cloudresourcemanager.googleapis.com/projects/$DEVSHELL_PROJECT_ID

<br/>

### Creating a custom role

    $ vi role-definition.yaml

```
title: "Role Editor"
description: "Edit access for App Versions"
stage: "ALPHA"
includedPermissions:
- appengine.versions.create
- appengine.versions.delete

```

    $ gcloud iam roles create editor --project $DEVSHELL_PROJECT_ID \
    --file role-definition.yaml

<br/>

### Create a custom role using flags


    $ gcloud iam roles create viewer --project $DEVSHELL_PROJECT_ID \
    --title "Role Viewer" --description "Custom role description." \
    --permissions compute.instances.get,compute.instances.list --stage ALPHA

<br/>

### Listing the custom roles


    $ gcloud iam roles list --project $DEVSHELL_PROJECT_ID
    ---
    description: Edit access for App Versions
    etag: BwWKG-vfYaM=
    name: projects/qwiklabs-gcp-f9fcfa2b4bf50fb7/roles/editor
    title: Role Editor
    ---
    description: Custom role description.
    etag: BwWKG-1zxk0=
    name: projects/qwiklabs-gcp-f9fcfa2b4bf50fb7/roles/viewer
    title: Role Viewer

<br/>

    $ gcloud iam roles list --project $DEVSHELL_PROJECT_ID --show-deleted

<br/>

### Editing an existing custom role


**To update a custom role using a YAML file**


    // $ gcloud iam roles describe [ROLE_ID] --project $DEVSHELL_PROJECT_ID
    $ gcloud iam roles describe editor --project $DEVSHELL_PROJECT_ID
    description: Edit access for App Versions
    etag: BwWKG-vfYaM=
    includedPermissions:
    - appengine.versions.create
    - appengine.versions.delete
    name: projects/qwiklabs-gcp-f9fcfa2b4bf50fb7/roles/editor
    stage: ALPHA
    title: Role Editor


<br/>

    $ vi new-role-definition.yaml

Вставляем содержимое предыдущего output.
Добавляем:

  - storage.buckets.get
  - storage.buckets.list

Получаем:

```
description: Edit access for App Versions
etag: BwWKG-vfYaM=
includedPermissions:
- appengine.versions.create
- appengine.versions.delete
- storage.buckets.get
- storage.buckets.list
name: projects/qwiklabs-gcp-f9fcfa2b4bf50fb7/roles/editor
stage: ALPHA
title: Role Editor
```


    // $ gcloud iam roles update [ROLE_ID] --project $DEVSHELL_PROJECT_ID
    --file new-role-definition.yaml

    $ gcloud iam roles update editor --project $DEVSHELL_PROJECT_ID \
    --file new-role-definition.yaml


<br/>

**To update a custom role using flags**

    $ gcloud iam roles update viewer --project $DEVSHELL_PROJECT_ID \
    --add-permissions storage.buckets.get,storage.buckets.list

<br/>

### Disabling a custom role

    $ gcloud iam roles update viewer --project $DEVSHELL_PROJECT_ID \
    --stage DISABLED

<br/>

### Deleting a custom role

    $ gcloud iam roles delete viewer --project $DEVSHELL_PROJECT_ID

<br/>

### Undeleting a custom role

    $ gcloud iam roles undelete viewer --project $DEVSHELL_PROJECT_ID







<br/>

### iam service-accounts list

    $ gcloud iam service-accounts list
    NAME                                    EMAIL                                                                                DISABLED
    Compute Engine default service account  220740529279-compute@developer.gserviceaccount.com                                   False
    marley                                  marley@qwiklabs-gcp-f9fcfa2b4bf50fb7.iam.gserviceaccount.com                         False
    ql-api                                  qwiklabs-gcp-f9fcfa2b4bf50fb7@qwiklabs-gcp-f9fcfa2b4bf50fb7.iam.gserviceaccount.com  False
    App Engine default service account      qwiklabs-gcp-f9fcfa2b4bf50fb7@appspot.gserviceaccount.com                            False


<br/>

### Create a service account

    $ export SERVICE_ACCOUNT=user-gcp-sa

    $ export SERVICE_ACCOUNT_EMAIL=${SERVICE_ACCOUNT}@${PROJECT_ID}.iam.gserviceaccount.com

    $ gcloud iam service-accounts create ${SERVICE_ACCOUNT} \
    --display-name "GCP Service Account for use with kubeflow examples"

<br/>

    $ gcloud projects add-iam-policy-binding ${PROJECT_ID} --member \
    serviceAccount:${SERVICE_ACCOUNT_EMAIL} \
    --role=roles/storage.admin

// Generate a credentials file for upload to the cluster:

    $ export KEY_FILE=${HOME}/secrets/${SERVICE_ACCOUNT_EMAIL}.json

    $ gcloud iam service-accounts keys create ${KEY_FILE} \
    --iam-account ${SERVICE_ACCOUNT_EMAIL}
