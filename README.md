# volume-sync-gcp-k8s
## Create the namespace
```kubectl apply -f local-namespace.yaml```

## Setup GCP Service Account
1. Copy the `kustomization.yaml` to a directory where key file will be stored (`target-dir`).
2. Add your key file (.json) to the `target-dir`.
3. Create a symbolic link for the key file in the same directory named sync-service-account.json

     ```sudo ln -s real-key-file-1234567.json sync-service-account.json```

    Resources in this project will reference the key by the file name `sync-service-account.json`.
    Creating the symbolic only helps to self-document which key file is active, so you can match it back to IAM quickly.

4. Apply to create the secret by name `gcp-sync-account`

    ```kubectl apply -k target-dir```

5. The `generatorOptions.disableNameSuffixHash` is set to `true` in kustomization.yaml; when you need to switch service account keys, repeat steps 2 - 4 to update the secret within Kubernetes.
