---
{"dg-publish":true,"permalink":"/03-published-notes/google-cloud-migrating-compute-instances-across-google-cloud-projects/","noteIcon":""}
---

Migrating virtual machines (VMs) across Google Cloud projects can streamline your infrastructure management and improve resource allocation. In this guide, we'll walk you through the process of creating, snapshotting, and migrating VMs using Google Cloud's command-line tool, `gcloud`. This will help you move VMs between projects and ensure a smooth transition.

### **1. Creating a New VM Instance**

To start, you'll create a new VM instance in your destination project. Here’s how to do it:

```bash
gcloud compute instances create rama_test_vm_move \
    --image-project rama_lab \
    --image machine-images-sf-sa \
    --subnet default
```

In this command:
- `rama_test_vm_move` is the name of the new VM instance.
- `--image-project` specifies the project where the image is located.
- `--image` indicates the name of the image to use.
- `--subnet` assigns the instance to a subnet (in this case, the default subnet).

For consistency, if you need to create another instance with similar parameters, you can use:

```bash
gcloud compute instances create ramatestvmmove \
    --image-project rama-lab \
    --image machine-images-sf-sa \
    --subnet default
```

### **2. Creating and Managing Snapshots**

**Source Project:**

1. **Create a Snapshot:**
   To snapshot a disk from a VM in your source project, use:

   ```bash
   gcloud compute snapshots create sasnapshoot \
       --source-disk instance-2 \
       --snapshot-type STANDARD \
       --source-disk-zone asia-southeast2-a
   ```

   - `sasnapshoot` is the name of the snapshot.
   - `--source-disk` specifies the disk you want to snapshot.
   - `--snapshot-type` defines the type of snapshot (STANDARD).
   - `--source-disk-zone` is the zone where the source disk is located.

2. **Create an Image from the Snapshot:**

   After creating the snapshot, convert it into an image:

   ```bash
   gcloud compute images create serviceassuranceimages \
       --source-snapshot=sasnapshoot
   ```

   - `serviceassuranceimages` is the name of the new image created from the snapshot.

3. **Grant Access to the Image:**

   Ensure that the new user or project has the necessary permissions to access the image. Assign the `user.image` role via IAM.

### **3. Creating VM Instances in the Destination Project**

**Destination Project:**

To create a VM instance in a new project using the image from the source project:

```bash
gcloud compute instances create ramatest \
    --image-project rama-lab \
    --image serviceassuranceimages
```

In this command:
- `ramatest` is the name of the new VM instance.
- `--image-project` specifies the project where the image is stored.
- `--image` refers to the image created from the snapshot.

### **4. Example Implementation: A Full Migration**

Here’s a complete example of migrating a VM from one project to another:

**Source Project:**

1. **Create Snapshot:**

   ```bash
   gcloud compute snapshots create exportedsnapshoot \
       --source-disk newvmsa-instance \
       --snapshot-type STANDARD \
       --source-disk-zone asia-southeast2-a
   ```

2. **Create Image from Snapshot:**

   ```bash
   gcloud compute images create exportedserviceassuranceimages \
       --source-snapshot=exportedsnapshoot
   ```

**Destination Project:**

1. **Create VM Instance:**

   ```bash
   gcloud compute instances create serviceassurancevm \
       --image-project zeta-crossbar-371709 \
       --image exportedserviceassuranceimages
   ```

### **Conclusion**

Migrating VMs across Google Cloud projects involves creating snapshots, converting them to images, and deploying new VM instances using these images. By following these steps, you can efficiently manage and transfer your VMs while ensuring they retain their configurations and data. This process is essential for maintaining a flexible and scalable cloud infrastructure, allowing you to optimize resource utilization and project management.