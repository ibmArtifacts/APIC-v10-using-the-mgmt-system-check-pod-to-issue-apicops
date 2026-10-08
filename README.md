# APIC-v10-using-the-mgmt-system-check-pod-to-issue-apicops


1. Log into your OCP cli.
2. Look for the last completed mgmt-system-check pod (within your mgmt namespace):
```
oc get po|grep system-check
```
3. Then issue the oc debug command to create a debug container with the -t to make the pod's shell behave like a terminal session:
```
 oc debug -t pod/{ENTER_THE_management-system-check_POD_HERE} -- /bin/sh
```

4. Inside the pod, to use apicops, you'll need to set the kubeconfig with the following:

```
export KUBECONFIG=/tmp/kubeconfig
export HOME=/tmp

kubectl config set-cluster c --server=https://${KUBERNETES_SERVICE_HOST}:${KUBERNETES_SERVICE_PORT} --certificate-authority=/var/run/secrets/kubernetes.io/serviceaccount/ca.crt --embed-certs=true
kubectl config set-credentials u --token=$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)
kubectl config set-context x --cluster=c --user=u --namespace=$(cat /var/run/secrets/kubernetes.io/serviceaccount/namespace)
kubectl config use-context x
```

5. Then after, you should be able to run apicops:
```
./usr/local/bin/apicops system:pre-upgrade-check management -n apic-mgmt
```

Then do the same for portal health-check.

