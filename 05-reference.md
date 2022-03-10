## Reference

### [Generating CRDs](https://book.kubebuilder.io/reference/generating-crd.html)

- [controller-gen](https://sigs.k8s.io/controller-tools) generates utility code and Kubernetes object YAML, like CustomResourceDefinitions. (`make manifests`)
- CRDs support [declarative validation](https://kubernetes.io/docs/tasks/access-kubernetes-api/custom-resources/custom-resource-definitions/#validation) using an [OpenAPI v3 schema](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.0.md#schemaObject) in the validation section.
    ```go
    // +kubebuilder:validation:MaxLength=15
    ```
- Additional Printer Columns
    ```go
    // +kubebuilder:printcolumn:name="Alias",type=string,JSONPath=`.spec.alias`
    ```
- Subresources
    - `status`: `// +kubebuilder:subresource:scale:selectorpath=<string>,specpath=<string>,statuspath=<string>`
    - `scale`: `// +kubebuilder:subresource:status`
- More details: `controller-gen -h`

### [Finalizers](https://book.kubebuilder.io/reference/using-finalizers.html)

- Presence of deletion timestamp on the object indicates that it is being deleted.
- [Finalizers](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/#finalizers)

1. If the object is not being deleted and does not have the finalizer registered, then add the finalizer and update the object in Kubernetes.
    - `cronJob.ObjectMeta.DeletionTimestamp.IsZero()`
    - `controllerutil.ContainsFinalizer(cronJob, myFinalizerName)`
    - `controllerutil.AddFinalizer(cronJob, myFinalizerName)`
    - `r.Update(ctx, cronJob)`
1. If object is being deleted and the finalizer is still present in finalizers list, then execute the pre-delete logic and remove the finalizer and update the object.
    - `controllerutil.RemoveFinalizer(cronJob, myFinalizerName)`
1. Ensure that the pre-delete logic is idempotent.
    - `err := r.deleteExternalResources(cronJob)`

### [Watching Resources](https://book.kubebuilder.io/reference/watching-resources.html)

Manager needs to know when reconciler is triggered.

```go
func (r *SimpleDeploymentReconciler) SetupWithManager(mgr ctrl.Manager) error {
    return ctrl.NewControllerManagedBy(mgr).
        For(&appsv1.SimpleDeployment{}).
        Owns(&kapps.Deployment{}).
        Complete(r)
}
```

```go
return ctrl.NewControllerManagedBy(mgr).
    For(&appsv1.ConfigDeployment{}).
    Owns(&kapps.Deployment{}).
    Watches(
        &source.Kind{Type: &corev1.ConfigMap{}},
        handler.EnqueueRequestsFromMapFunc(r.findObjectsForConfigMap),
        builder.WithPredicates(predicate.ResourceVersionChangedPredicate{}),
    ).
    Complete(r)
}
```

### [kind cluster](https://book.kubebuilder.io/reference/kind.html)

https://kind.sigs.k8s.io/

### [Webhook](https://book.kubebuilder.io/reference/webhook-overview.html)

1. [admission webhook](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/#admission-webhooks) Supported by controller-runtime libraries
    1. Mutating Admission Webhook
    1. Validating Admission Webhook
1. [authorization webhook](https://kubernetes.io/docs/reference/access-authn-authz/webhook/)
1. [CRD conversion webhook](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definition-versioning/#webhook-conversion) Supported by controller-runtime libraries

Admission webhook [Example in controller-runtime](https://github.com/kubernetes-sigs/controller-runtime/tree/master/examples/builtins)
- [admission.Handler](https://pkg.go.dev/sigs.k8s.io/controller-runtime/pkg/webhook/admission#Handler) interface
- `// +kubebuilder:webhook:path=/mutate-v1-pod,mutating=true,failurePolicy=fail,groups="",resources=pods,verbs=create;update,versions=v1,name=mpod.kb.io` marker to have controller-gen to generate the webhook configuration
- Register your handler in the webhook server:
    ```go
    mgr.GetWebhookServer().Register("/mutate-v1-pod", &webhook.Admission{Handler: &podAnnotator{Client: mgr.GetClient()}})
    ```

### [Markers](https://book.kubebuilder.io/reference/markers.html)

- `make manifests` generates Kubernetes object YAML, like CustomResourceDefinitions, WebhookConfigurations, and RBAC roles.
- `make generate` generates code, like runtime.Object/DeepCopy implementations.

Syntax: [controller-tools](https://pkg.go.dev/sigs.k8s.io/controller-tools/pkg/markers)

1. CRD generation
1. CRD validation
1. CRD processing
1. Webhook
1. Object/DeepCopy
1. RBAC

### [controller-gen CLI](https://book.kubebuilder.io/reference/controller-gen.html)

Example:

```
controller-gen paths=./... crd:trivialVersions=true rbac:roleName=controller-perms output:crd:artifacts:config=config/crd/bases
```

### [kubebuilder autocompletion](https://book.kubebuilder.io/reference/completion.html)

### [Artifacts](https://book.kubebuilder.io/reference/artifacts.html)

### [Configuring envtest for integration tests](https://book.kubebuilder.io/reference/envtest.html)

```
export K8S_VERSION=1.21.2
curl -sSLo envtest-bins.tar.gz "https://go.kubebuilder.io/test-tools/${K8S_VERSION}/$(go env GOOS)/$(go env GOARCH)"
```

```
mkdir /usr/local/kubebuilder
tar -C /usr/local/kubebuilder --strip-components=1 -zvxf envtest-bins.tar.gz
```

```
make test SKIP_FETCH_TOOLS=1 KUBEBUILDER_ASSETS=/usr/local/kubebuilder
```

```go
import sigs.k8s.io/controller-runtime/pkg/envtest

//specify testEnv configuration
testEnv = &envtest.Environment{
    CRDDirectoryPaths: []string{filepath.Join("..", "config", "crd", "bases")},
}

//start testEnv
cfg, err = testEnv.Start()

//write test logic

//stop testEnv
err = testEnv.Stop()
```

- Controller-runtime’s envtest framework requires kubectl, kube-apiserver, and etcd binaries
- You can use environment variables and/or flags to specify the kubectl,api-server and etcd
- no built-in controllers are running in the test context. e.g. objects do not get deleted, even if an OwnerReference is set up

### [Metrics](https://book.kubebuilder.io/reference/metrics.html)

### [Makefile Helpers](https://book.kubebuilder.io/reference/makefile-helpers.html)

One of the options would be use go-delve for it:

```
# Run with Delve for development purposes against the configured Kubernetes cluster in ~/.kube/config
# Delve is a debugger for the Go programming language. More info: https://github.com/go-delve/delve
run-delve: generate fmt vet manifests
    go build -gcflags "all=-trimpath=$(shell go env GOPATH)" -o bin/manager main.go
    dlv --listen=:2345 --headless=true --api-version=2 --accept-multiclient exec ./bin/manager
```

### [Project Config](https://book.kubebuilder.io/reference/project-config.html)
