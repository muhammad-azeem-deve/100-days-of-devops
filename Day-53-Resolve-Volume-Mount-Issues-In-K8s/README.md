# Day 53 - Resolve Volume Mounts Issue in Kubernetes

## Challenge Overview

This is Day 53 of my **100 Days of DevOps Challenge by KodeKloud**.

The challenge was related to **Kubernetes Volume Mount Troubleshooting**. The task required fixing a volume mount issue in a Pod running Nginx and PHP-FPM.

The application was not working correctly because the shared volume was mounted at different paths in the two containers. I investigated the Pod configuration, checked the Nginx ConfigMap, corrected the mount path, recreated the Pod, and copied the required PHP application file into the shared volume.

### Challenge Requirement

Troubleshoot and fix the Kubernetes Pod named `nginx-phpfpm` and its ConfigMap named `nginx-config`.

The task required:

- Identifying the volume mount issue.
- Ensuring both containers use the same shared-volume mount path.
- Checking the Nginx document root in the ConfigMap.
- Recreating the Pod with the corrected configuration.
- Copying `/home/thor/index.php` into the Nginx document root.
- Verifying the application through the KodeKloud Website button.

---

## Objectives

The objectives of this challenge are:

- Inspect a Kubernetes Pod using `kubectl`.
- Understand shared volumes in multi-container Pods.
- Identify inconsistent volume mount paths.
- Inspect a ConfigMap containing Nginx configuration.
- Correct the volume mount configuration.
- Recreate a Pod after changing immutable settings.
- Copy a file from the jump host into a container.
- Verify that shared files are accessible from both containers.
- Test the application after troubleshooting.

---

## Environment

| Item | Details |
|------|---------|
| Challenge | 100 Days of DevOps |
| Platform | KodeKloud |
| Day | 53 |
| Technology | Kubernetes |
| Pod | `nginx-phpfpm` |
| Nginx Container | `nginx-container` |
| PHP-FPM Container | `php-fpm-container` |
| ConfigMap | `nginx-config` |
| Shared Volume | `shared-files` |
| Document Root | `/var/www/html` |
| Application File | `/home/thor/index.php` |
| Status | Completed |

---

# Solution

## Step 1: Inspect the Pod

First, I checked the Pods running in the Kubernetes cluster.

```bash
kubectl get pods
```

Then I inspected the Pod configuration:

```bash
kubectl describe pod nginx-phpfpm
```

I checked the container names, volume definitions, mount paths, and Pod events.

The Pod contained two containers:

- `nginx-container`
- `php-fpm-container`

Both containers used a shared volume, but the volume was mounted at different paths.

---

## Step 2: Inspect the Nginx ConfigMap

Next, I checked the ConfigMap:

```bash
kubectl get configmap nginx-config -o yaml
```

The Nginx configuration used the following document root:

```nginx
root /var/www/html;
```

This indicated that Nginx expected the application files to be available under `/var/www/html`.

---

## Step 3: Save the Existing Pod Configuration

I exported the existing Pod configuration to a YAML file:

```bash
kubectl get pod nginx-phpfpm -o yaml > pod.yaml
```

Then I opened the file for editing:

```bash
nano pod.yaml
```

I located the `nginx-container` volume mount and changed its shared-volume mount path to:

```yaml
mountPath: /var/www/html
```

I also checked that the `php-fpm-container` used the same mount path.

This ensured that both containers could access the application files from the same location.

---

## Step 4: Recreate the Pod

Because the volume mount configuration cannot generally be changed on an existing Pod, I removed the old Pod and created it again using the corrected YAML.

```bash
kubectl delete pod nginx-phpfpm
```

Then I applied the updated configuration:

```bash
kubectl apply -f pod.yaml
```

I checked the Pod status:

```bash
kubectl get pods
```

I also verified the configuration:

```bash
kubectl describe pod nginx-phpfpm
```

The Pod needed to become ready with both containers running.

---

## Step 5: Copy the PHP Application File

After the Pod was running, I copied the PHP application file from the jump host into the Nginx container.

```bash
kubectl cp /home/thor/index.php nginx-phpfpm:/var/www/html/index.php -c nginx-container
```

The file was placed in the shared volume at `/var/www/html`.

Since both containers used the same shared volume and mount path, PHP-FPM could also access the file.

---

## Step 6: Verify the Shared Volume

I checked the files inside the Nginx container:

```bash
kubectl exec nginx-phpfpm -c nginx-container -- ls -l /var/www/html
```

I also checked the PHP-FPM container:

```bash
kubectl exec nginx-phpfpm -c php-fpm-container -- ls -l /var/www/html
```

The `index.php` file should be visible in both containers.

---

## Step 7: Test the Application

Finally, I checked the Nginx logs:

```bash
kubectl logs nginx-phpfpm -c nginx-container
```

I then used the **Website** button in the KodeKloud interface to verify that the application was working.

---

# Understanding the Problem

The main issue was an inconsistent mount path for the shared volume.

### Before the Fix

| Container | Mount Path |
|-----------|------------|
| `nginx-container` | `/usr/share/nginx/html` |
| `php-fpm-container` | `/var/www/html` |

### After the Fix

| Container | Mount Path |
|-----------|------------|
| `nginx-container` | `/var/www/html` |
| `php-fpm-container` | `/var/www/html` |

The Nginx document root in the ConfigMap was also `/var/www/html`, so the corrected mount path aligned the shared volume with the application configuration.

---

# What I Learned

From this challenge, I learned and practiced:

- How to inspect Kubernetes Pods using `kubectl describe`.
- How to troubleshoot multi-container Pods.
- How shared volumes allow containers to access the same files.
- How inconsistent mount paths can break an application.
- How to inspect a ConfigMap using `kubectl get configmap`.
- How to export and edit a Pod manifest.
- Why a Pod may need to be recreated after a volume mount change.
- How to copy files into containers using `kubectl cp`.
- How to verify shared files using `kubectl exec`.
- How to check container logs using `kubectl logs`.
- How to verify an application after correcting a Kubernetes configuration.

# Challenge Status

Day 53 — Completed Successfully ✅

**Fixed the Kubernetes volume mount issue by aligning the shared-volume paths, recreating the Pod, and copying the PHP application file into the document root.**

100 Days. 100 Challenges. One DevOps Journey. 🚀

I will continue documenting each challenge.
