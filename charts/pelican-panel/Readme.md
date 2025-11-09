# Pelican Panel Helm Chart

A Helm chart for deploying the Pelican Panel.

## Steps to Use the Helm Chart

### 1. Add the Repository

To use this chart, first add the repository to Helm:

```bash
help repo add pelican https://dawidde.github.io/helm-charts
helm repo update
```

### 2. Customize `values.yaml`

Before installing the chart, you'll need to customize a few values.
It´s not much — let´s go through it together.

#### 1. Set your domain

Change the `general.domain` field to your domain.  
This will be the domain for the Panel.  
Note: Include `http://` or `https://` in the domain.

#### 2. Set trusted proxies

Add your proxy´s IP address to the `general.trusted_proxies` field.  
There are two ways to do this:

1. Use the IP of your Traefik pod.
2. Use the subnet that your cluster runs on ( your internal cluster IP/subnet, i.e., the local IP address of your server).

#### 3. Set your storage class

Add your storage class name to the `volumes.sharedStorageClassName` field.  
If you prefer to use separate storage classes, skip this field and uncomment the individual `overwriteStorageClassName` entries instead.

#### Example `values.yaml`:

```yaml
# Default values for Pelican Panel Helm Chart
# Leave settings that you dont know. Every change you need to make is documented.

general:
  domain: "domain" # Change to your domain.
  caddy:
    admin: "" # Turns of the admin api of caddy. Default is off.
    trusted_proxies: "proxy-ip" # Replace with the ip of your proxy.
    upload_max_filesize: "" # Overwrites the default max file upload size in caddy. Default is 2M.
    post_max_size: "" # Overwrites the default max size for post requests in Caddy. Default is 8M.

image:
  overwriteRepository: "" # Overwrites the repository for the container. Default is ghcr.io/pelican-dev/panel.
  overwriteTag: "" # Overwrites the container version. Default is v1.0.0-beta27.
  overwriteImagePullPolicy: "" # Overwrites the image pull policy. Default is IfNotPresent. IfNotPresent/Always/Never

service:
  type: "ClusterIP"
  port: 80

# These are only the pvc's. You still need to provides pv's.
volumes:
  sharedStorageClassName: "storage-class"
  data:
    accessMode: "ReadWriteOnce"
    size: "1Gi"
    #overwriteStorageClassName: "" # Overwrites the sharedStorageClassName for this pvc. Uncomment to overwrite.
  logs:
    accessMode: "ReadWriteOnce"
    size: "1Gi" 
    #overwriteStorageClassName: "" # Overwrites the sharedStorageClassName for this pvc. Uncomment to overwrite.
```

### 3. Install the Chart

Install the chart with the following command:

```bash
helm install pelican-panel pelican/pelican-panel -f values.yaml
```

### 4. Configure SSL

Finally, create a certificate and an IngressRoute that routes traffic to `<release-name>-service`.
Once that's done, you'll have a fully functional Pelican Panel.