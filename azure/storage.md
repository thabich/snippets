# Create Container in Storage account

```bash
az storage container create -n $CONTAINER_NAME --account-name $STORAGE_ACCOUNT_NAME
```

# Blob Operations
## Copy Blobs

With azcopy installed (Azure CLI will try to install it automatically)

```bash
az storage copy -s https://$SRC_ACCOUNT.blob.core.windows.net/$SRC_CONTAINER/$SRC_BLOB -d https://$DEST_ACCOUNT.blob.core.windows.net/$DEST_CONTAINER/[$DEST_BLOB]
```

## List Blobs
```bash
# default output json
az storage blob list --account-name $STORAGE_ACCOUNT  --container-name $CONTAINER_NAME --output table
```
