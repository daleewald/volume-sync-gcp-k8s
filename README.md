# volume-sync-gcp-k8s

## Setup GCP Service Account
1. Copy the `kustomization.yaml` to a directory where key file will be stored.
2. Add your key file (.json) to the directory.
3. Create a symbolic link for the key file in the same directory named sync-service-account.json

     ```sudo ln -s real-key-file-1234567.json sync-service-account.json```

    The deployment will reference the key by the file name `sync-service-account.json`.
    Creating the symbolic only helps to self-document which key file is active, so you can match it back to IAM.

4. Apply to create the secret by name `gcp-sync-account`

    ```kubectl apply -k ./```

