```
#!/usr/bin/env bash
PROJECT="<project-id>"

### Sinks ###

#Base sink for all logs to BigQuery
gcloud logging sinks create --quiet --project $project ${project}_bigquery_sink bigquery.googleapis.com/projects/billing-exports-184901/datasets/CLOUD_LOGGING_EXPORTS --log-filter "resource.type:*"

#### Metrics ####

#IAM role added
gcloud logging metrics create --quiet --project $project ${project}_iam_role_added_metric --description="IAM role added" --log-filter="resource.type=project AND protoPayload.methodName=SetIamPolicy AND protoPayload.serviceData.policyDelta.bindingDeltas.action=ADD"

#IAM role removed
gcloud logging metrics create --quiet --project $project ${project}_iam_role_removed_metric --description="IAM role removed" --log-filter="resource.type=project AND protoPayload.methodName=SetIamPolicy AND protoPayload.serviceData.policyDelta.bindingDeltas.action=REMOVE"

#Detect any buckets created outside Australia
gcloud logging metrics create --quiet --project $project ${project}_bucket_outside_region_metric --description="GCS bucket created outside of Australia region" --log-filter="resource.type=gcs_bucket AND NOT resource.labels.location:australia AND protoPayload.methodName=storage.buckets.create"

#Cloud Function failed to start a Dataflow template
gcloud logging metrics create --quiet --project $project ${project}_cfn_failed_to_start_dataflow_template_job_metric --description="Cloud Function failed to start a Dataflow template" --log-filter="resource.type=cloud_function AND severity>=ERROR AND Problem running dataflow template"

#BigQuery dataset ACL changed
gcloud logging metrics create --quiet --project $project ${project}_bigquery_dataset_acl_change_metric --description="BigQuery dataset ACL changed" --log-filter="resource.type=bigquery_resource AND protoPayload.serviceData.datasetUpdateRequest.resource.acl:*"

#BigQuery table deleted
gcloud logging metrics create --quiet --project $project ${project}_bigquery_table_deleted_metric --description="BigQuery table deleted" --log-filter="resource.type=bigquery_resource AND protoPayload.methodName=tableservice.delete"

#BigQuery dataset deleted
gcloud logging metrics create --quiet --project $project ${project}_bigquery_dataset_deleted_metric --description="BigQuery dataset deleted" --log-filter="resource.type=bigquery_resource AND protoPayload.methodName=datasetservice.delete"
```
