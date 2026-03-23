# create VM

[Storage-SKU](https://learn.microsoft.com/en-us/rest/api/storagerp/srp_sku_types)

```bash
# generate-ssh-keys will put a new key pair in your .ssh folder
# list sizes az vm list-sizes --location <location>

az vm create --resource-group "kml_rg_main-ad00e74fbb83481a" \
             --name my-vm \
             --image Ubuntu2404 \
             --public-ip-sku Standard \
             --custom-data cloud-init.yaml \
             --generate-ssh-keys  \
             --size Standard_B1s \
             --storage-sku Standard_LRS \
             --os-disk-size-gb 30
```

## Custom-Data

Cloud-init
**`cloud-init.yaml`**
```yaml
#cloud-config
package_update: true
packages:
  - nginx
runcmd:
  - systemctl start nginx
  - systemctl enable nginx
```
