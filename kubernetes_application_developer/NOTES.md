Notes regarding `Linux: Certified Kubernetes Application Developer` exam.

1. Labeling a node:
    ```bash
    # First of all, label a node
    kubectl label nodes <node-name> disk=ssd

    # If we were to get the description for the node, we see
    metadata:
    labels:
        disk: ssd
        ...
    ```

2. **Node Selector**: a field that you can use to ensure that a particular pod gets scheduled on a specific node or a set of nodes that meet the defined criteria.
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
    name: my-pod
    spec:
    containers:
    - name: my-container
        image: my-image
    nodeSelector:
        disktype: ssd
    ```

3. **Node Affinity**: is like the `nodeSelector` but more expressive. It allows you to specify rules that are more expressive than just key-value pairs. It can be used to guide pod scheduling with conditions based on node attributes.
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
    name: my-pod
    spec:
    containers:
    - name: my-container
        image: my-image
    affinity:
        nodeAffinity:
        requiredDuringSchedulingIgnoredDuringExecution:
    ```

4. **Taints**: are used to repel pods from being scheduled on certain nodes. Unlike labels and node affinity, which attract pods, taints actively prevent certain pods from being scheduled on a node unless the pod has a corresponding toleration.
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
    name: my-pod
    spec:
    containers:
    - name: my-container
        image: my-image
    tolerations:
    - key: "disktype"
        operator: "Equal"
        value: "ssd"
        effect: "NoSchedule"
    ```

5. **Liveness Probe**: This checks whether the container in a Pod is still running. If the liveness probe fails, Kubernetes will restart the container. It ensures that the application is alive and running but doesn't consider whether it's ready to accept requests.

6. **Readiness Probe**: This checks whether the container is ready to service requests. If the readiness probe fails, Kubernetes won't send traffic to that Pod until it passes. It's focused on whether the application is ready to handle traffic, not just if it's running.

7. The `kubectl --restart` policy is best described through the table:
    |  | Description | Exit Status | Use Case |
    | --- | --- | --- | --- |
    | Always | The container will be restarted continuously, regardless of whether it exited with success or failure. | The container will be restarted continuously, regardless of whether it exited with success or failure. | Useful for long-running services that need to be constantly available. |
    | OnFailure | The container will be restarted only if it exits with a failure status (non-zero). | Non-zero exit status (e.g., 1, 2, 3, etc.). | Good for batch processing tasks where a failure means the task should be retried. |
    | Never | The container will not be restarted, regardless of its exit status. Once it's terminated, it stays terminated. | Any exit status, whether 0 (success) or non-zero (failure). | Useful for one-time tasks or temporary computations that don't require continuous availability. |

8. 
    Create a new `PersistentVolume` named `earth-project-earthflower-pv`. It should have a capacity of `2Gi`, `accessMode` `ReadWriteOnce`, `hostPath` `/Volumes/Data` and no `storageClassName` defined.

    Next create a new `PersistentVolumeClaim` in Namespace `earth` named `earth-project-earthflower-pvc` . It should request `2Gi` storage, `accessMode` `ReadWriteOnce` and should not define a `storageClassName`. The PVC should bound to the PV correctly.

    Finally create a new Deployment `project-earthflower` in Namespace `earth` which mounts that volume at `/tmp/project-data`. The Pods of that Deployment should be of image `httpd:2.4.41-alpine`.

    **Creating persistent volume**:
    ```yaml
    apiVersion: v1
    kind: PersistentVolume
    metadata:
        name: earth-project-earthflower-pv
    spec:
    capacity:
        storage: 2Gi
    accessModes:
        - ReadWriteOnce
    hostPath:
        path: /volumes/Data
    ```

    **Creating persistent volume claim**:
    ```yaml
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
        name: earth-project-earthflower-pvc
        namespace: earth
    spec:
        accessModes:
            - ReadWriteOnce
        resources:
            requests:
            storage: 2Gi
    ```
    **Creating deployment**:

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    creationTimestamp: null
    labels:
        app: project-earthflower
    name: project-earthflower
    namespace: earth
    spec:
    replicas: 1
    selector:
        matchLabels:
        app: project-earthflower
    strategy: {}
    template:
        metadata:
        creationTimestamp: null
        labels:
            app: project-earthflower
        spec:
        containers:
        - image: httpd:2.4.41-alpine
            name: httpd
            resources: {}
            volumeMounts:
            - name: claim-log
            mountPath: /tmp/project-data
        volumes:
        - name: claim-log
            persistentVolumeClaim:
            claimName: earth-project-earthflower-pvc
    status: {}
    ```

9. 
    Team Moonpie, which has the Namespace `moon`, needs more storage. Create a new `PersistentVolumeClaim` named `moon-pvc-126` in that namespace. This claim should use a new `StorageClass` `moon-retain` with the provisioner set to `moon-retainer` and the `reclaimPolicy` set to `Retain`. The claim should request storage of `3Gi`, an `accessMode` of `ReadWriteOnce` and should use the new `StorageClass`.

    The provisioner moon-retainer will be created by another team, so it's expected that the PVC will not boot yet. Confirm this by writing the log message from the PVC into file `/opt/course/13/pvc-126-reason`.

    **Creation of storage class**:

    ```yaml
    apiVersion: storage.k8s.io/v1
    kind: StorageClass
    metadata:
    name: moon-retain
    provisioner: moon-retainer
    reclaimPolicy: Retain
    ```

    **Creation of persistent volume claim**:
    ```yaml
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
    name: moon-pvc-126
    namespace: moon
    spec:
    accessModes:
        - ReadWriteOnce
    volumeMode: Filesystem
    resources:
        requests:
        storage: 3Gi
    storageClassName: moon-retain
    ```

    **Getting the message and copying it**:
    ```yaml
    Type    Reason                Age                  From                         Message
    ----    ------                ----                 ----                         -------
    Normal  ExternalProvisioning  3s (x20 over 4m31s)  persistentvolume-controller  waiting for a volume to be created, either by external provisioner "moon-retainer" or manually created by system administrator
    ```

10. 
    Team Moonpie has a nginx server `Deployment` called `web-moon` in Namespace `moon`. Someone started configuring it but it was never completed. To complete please create a `ConfigMap` called `configmap-web-moon-html` containing the content of file `/opt/course/15/web-moon.html` under the data key-name `index.html`.

    The `Deployment` `web-moon` is already configured to work with this `ConfigMap` and serve its content. Test the nginx configuration for example using curl from a temporary `nginx:alpine` Pod.

    **Config map creation**:
    ```yaml
    apiVersion: v1
    kind: ConfigMap
    metadata:
    name: configmap-web-moon-html
    namespace: moon
    data:
    index.html: |
        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <title>Web Moon Webpage</title>
        </head>
        <body>
        This is some great content.
        </body>
        </html>
    ```

    **Deployment creation**:
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    creationTimestamp: null
    labels:
        app: web-moon
    name: web-moon
    namespace: moon
    spec:
    replicas: 1
    selector:
        matchLabels:
        app: web-moon
    strategy: {}
    template:
        metadata:
        creationTimestamp: null
        labels:
            app: web-moon
        spec:
        containers:
        - image: nginx
            name: nginx
            resources: {}
            envFrom:
            - configMapRef:
                name: configmap-web-moon-html
    status: {}
    ```