# volume-sync-gcp-k8s
## Create the namespace
```kubectl apply -f local-namespace.yaml```

## Setup GCP Service Account
1. Add your GCP Service Account key .json file to the `gcp/` directory.
2. Create a symbolic link for the key file in the same directory and name it sync-service-account.json

     ```sudo ln -s real-key-file-1234567.json sync-service-account.json```

    Resources in this project will reference the key by the file name `sync-service-account.json`.
    Creating the symbolic only helps to self-document which key file is active, so you can match it back to IAM quickly.

4. From the project root, apply to create the secret by name `gcp-sync-account`

    ```kubectl apply -k gcp/```

    NOTE: The `generatorOptions.disableNameSuffixHash` is set to `true` in kustomization.yaml; when you need to switch service account keys, repeat steps 2 - 4 to update the secret within Kubernetes.

## Setup the environment
Edit sync-config.env
```
INCLUDE_FILE_PATTERN=**/*
EXCLUDE_FILE_PATTERN=.DSStore
GCP_PROJECT_ID=your-project-id-here
GCP_BUCKET_NAME=your-target-bucket-here
```

## Setup the source Volume strategy
Edit volumes.yaml

The example in this project was for use from a single-node microK8s cluster, so it uses hostPath.
Other ideas are NFS, etc; at which point you would want to change the accessModes appropriately so you could take advantage of Horizontal Pod Autoscaling, for example.

volumes.yaml is not included in the kustomize plan, so it must be applied beforehand.
`kubectl apply -f volumes.yaml`

## Deploy
1. Deploy redis 

    `kubectl apply -k redis/`

    The solution includes redis to back a queue, with minimal overhead. 

2. Deploy the remaining components 

    `kubectl apply -k ./`

