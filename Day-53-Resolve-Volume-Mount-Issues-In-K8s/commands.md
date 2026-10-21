# Day 53 - Commands

This file contains the commands used during **Day 53 of my 100 Days of DevOps Challenge**.

The objective was to resolve the volume mount issue in the `nginx-phpfpm` Pod and restore the Nginx and PHP-FPM application.

---

## Step 1: Check the Pods

```bash
kubectl get pods
```

---

## Step 2: Inspect the Pod

```bash
kubectl describe pod nginx-phpfpm
```

Check the container names, volume mounts, volume definitions, and Pod events.

---

## Step 3: Inspect the ConfigMap

```bash
kubectl get configmap nginx-config -o yaml
```

Check the Nginx document root:

```nginx
root /var/www/html;
```

---

## Step 4: Export the Pod Configuration

```bash
kubectl get pod nginx-phpfpm -o yaml > pod.yaml
```

Open the YAML file:

```bash
nano pod.yaml
```

Change the shared-volume mount path for `nginx-container` to:

```yaml
mountPath: /var/www/html
```

Ensure that `php-fpm-container` uses the same shared-volume mount path.

Before recreating the Pod, remove generated metadata fields such as `uid`, `resourceVersion`, `managedFields`, and `creationTimestamp`, and remove the `status` section.

---

## Step 5: Delete the Existing Pod

```bash
kubectl delete pod nginx-phpfpm
```

---

## Step 6: Create the Pod with the Corrected Configuration

```bash
kubectl apply -f pod.yaml
```

---

## Step 7: Verify the Pod Status

```bash
kubectl get pods
```

Inspect the Pod:

```bash
kubectl describe pod nginx-phpfpm
```

Wait until both containers are ready.

---

## Step 8: Copy the PHP File into the Nginx Container

```bash
kubectl cp /home/thor/index.php nginx-phpfpm:/var/www/html/index.php -c nginx-container
```

This copies the PHP application file from the jump host into the shared document root.

---

## Step 9: Verify the File in the Nginx Container

```bash
kubectl exec nginx-phpfpm -c nginx-container -- ls -l /var/www/html
```

---

## Step 10: Verify the File in the PHP-FPM Container

```bash
kubectl exec nginx-phpfpm -c php-fpm-container -- ls -l /var/www/html
```

The `index.php` file should be visible in both containers.

---

## Step 11: Check Nginx Logs

```bash
kubectl logs nginx-phpfpm -c nginx-container
```

Check PHP-FPM logs if needed:

```bash
kubectl logs nginx-phpfpm -c php-fpm-container
```

---

# Quick Command Summary

```bash
# List Pods
kubectl get pods

# Inspect the Pod
kubectl describe pod nginx-phpfpm

# Inspect the ConfigMap
kubectl get configmap nginx-config -o yaml

# Export the Pod configuration
kubectl get pod nginx-phpfpm -o yaml > pod.yaml

# Edit the configuration
nano pod.yaml

# Delete the old Pod
kubectl delete pod nginx-phpfpm

# Recreate the Pod
kubectl apply -f pod.yaml

# Verify Pod status
kubectl get pods

# Copy PHP file into the Nginx container
kubectl cp /home/thor/index.php nginx-phpfpm:/var/www/html/index.php -c nginx-container

# Verify files in Nginx
kubectl exec nginx-phpfpm -c nginx-container -- ls -l /var/www/html

# Verify files in PHP-FPM
kubectl exec nginx-phpfpm -c php-fpm-container -- ls -l /var/www/html

# Check Nginx logs
kubectl logs nginx-phpfpm -c nginx-container

# Check PHP-FPM logs
kubectl logs nginx-phpfpm -c php-fpm-container
```

---

# Important Lesson

When troubleshooting Kubernetes volume mount issues, always check:

1. The volume name defined in the Pod.
2. The mount path used by each container.
3. The document root configured in Nginx.
4. Whether the required application files exist in the shared volume.
5. The Pod status and container logs.

For this challenge, the key was ensuring that both containers and the Nginx document root used the same application path:

```text
/var/www/html
```

**Day 53 completed successfully.** 🚀
