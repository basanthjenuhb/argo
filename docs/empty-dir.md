# Empty Dir

While by default, the Docker and PNS [workflow executors](workflow-executors.md) can get output artifacts from the base layer (e.g. `/tmp`), neither the Kubelet or K8SAPI exectuors can. Nor are you likely to be able to get output artifacts from the base layer if you run your workflo pods a [security context](workflow-pod-security-context.md). 

You can work-around this constraint by mounting volumes onto your pod. The easiest way to do this is to use as `emptytDir` volume. 

!!! Note 
    This is only needed for output artifacts. Input artifacts are automatically mounted to an empty-dir if needed

This example shows how to mount an output volume: 

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Workflow
metadata:
  generateName: empty-dir-
spec:
  entrypoint: main
  templates:
    - name: main
      container:
        image: argoproj/argosay:v2
        volumeMounts:
          - name: out
            mountPath: /mnt/out
      volumes:
        - name: out
          emptyDir: { }
      outputs:
        artifacts:
          - name: message
            path: /mnt/out/message

```