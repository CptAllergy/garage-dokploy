# Garage S3 on Dokploy

Docker Compose setup for deploying Garage S3 with Dokploy.

This configuration is intended for personal projects, lab environments, or small-scale setups. For multi-node cluster deployments, refer to the [official Garage documentation](https://garagehq.deuxfleurs.fr/documentation/).

---

## 1. Environment Variables

Configure the following environment variables in your Dokploy deployment:

```env
API_BASE_URL=http://garage:3903
S3_ENDPOINT_URL=http://garage:3900
```

---

## 2. Configuration (`garage.toml`)

In Dokploy, navigate to your deployment's **Advanced** tab -> **Volumes** section, create or edit the file with the mount path `garage.toml` (or your mapped directory path), and paste your `garage.toml` configuration values there.

---

## 3. Initialization & Setup

Set up an alias to run administrative commands against the container:

```bash
alias garage="sudo docker exec -ti <container_name> /garage"
```

### Layout Assignment

1. Retrieve the node ID:
   ```bash
   garage status
   ```
2. Assign the single-node layout (zone `zone-name`) and capacity (15G):
   ```bash
   garage layout assign -z zone-name -c 15G <node_id_prefix>
   ```
3. Apply the layout:
   ```bash
   garage layout apply --version 1
   ```

### Bucket & Key Creation

1. Create a bucket:
   ```bash
   garage bucket create application-bucket
   ```
2. Create an API key:
   ```bash
   garage key create application-app-key
   ```
3. Grant key access to the bucket:
   ```bash
   garage bucket allow --read --write --owner application-bucket --key application-app-key
   ```

---

## 4. Key Retrieval

To view the Access Key ID and Secret Key for an existing API key:

```bash
garage key info application-app-key --show-secret
```

---

For further details, refer to the [Official Garage Quick Start Guide](https://garagehq.deuxfleurs.fr/documentation/quick-start/).