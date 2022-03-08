# kubebuilder cronjob-controller

https://book.kubebuilder.io/cronjob-tutorial/cronjob-tutorial.html


## Prerequisite

https://book.kubebuilder.io/quick-start.html

```
kubebuilder version
Version: main.version{KubeBuilderVersion:"3.3.0", KubernetesVendor:"1.23.1", GitCommit:"47859bf2ebf96a64db69a2f7074ffdec7f15c1ec", BuildDate:"2022-01-18T17:03:29Z", GoOs:"darwin", GoArch:"amd64"}
```

```
go version
go version go1.17.3 darwin/amd64
```

## Getting Started

### 1. [Initialize a controller](https://book.kubebuilder.io/cronjob-tutorial/basic-project.html)

1. Initialize a project

    ```
    go mod init tutorial.kubebuilder.io
    kubebuilder init --domain tutorial.kubebuilder.io
    ```

    <details><summary>result</summary>

    ```
    kubebuilder init --domain tutorial.kubebuilder.io
    Writing kustomize manifests for you to edit...
    Writing scaffold for you to edit...
    Get controller runtime:
    $ go get sigs.k8s.io/controller-runtime@v0.11.0
    Update dependencies:
    $ go mod tidy
    Next: define a resource with:
    $ kubebuilder create api
    ```

    </details>

1. Check the generated files.

    Build Infrastructure:

    - `go.mod`: A new Go module matching our project, with basic dependencies
    - `Makefile`: Make targets for building and deploying your controller
    - `PROJECT`: Kubebuilder metadata for scaffolding new components

    Launch configuration under [config](config):

    - `config/default`: launching the controller in a standard configuration.
    - `config/manager`: launch your controllers as pods in the cluster
    - `config/rbac`: permissions required to run your controllers under their own service account

1. Commit the change.

### 2. [Main](https://book.kubebuilder.io/cronjob-tutorial/empty-main.html)
- Instantiate a [manager](https://pkg.go.dev/sigs.k8s.io/controller-runtime/pkg/manager#Manager), which keeps track of running all of our controllers, as well as setting up shared caches and clients to the API server.
- Run our manager, which in turn runs all of our controllers and webhooks.

### 3. [Groups, Versions and Kinds](https://book.kubebuilder.io/cronjob-tutorial/gvks.html)

4 terms: groups, versions, kinds, and resources.

- An **API Group** in Kubernetes is simply a collection of related functionality
- Each group has one or more **versions**, which, as the name suggests, allow us to change how an API works over time.
- Each API group-version contains one or more API types, which we call **Kinds**.
- A **resource** is simply a use of a Kind in the API.
- The **Scheme** we saw before is simply a way to keep track of what Go type corresponds to a given GVK.

### 4. [Adding a new API](https://book.kubebuilder.io/cronjob-tutorial/new-api.html#adding-a-new-api)

1. Scaffold out a new Kind and corresponding controller:

    ```
    kubebuilder create api --group batch --version v1 --kind CronJob --resource --controller
    ```

    <details><summary>result</summary>

    ```
    Writing kustomize manifests for you to edit...
    Writing scaffold for you to edit...
    api/v1/cronjob_types.go
    controllers/cronjob_controller.go
    Update dependencies:
    $ go mod tidy
    Running make:
    $ make generate
    go: creating new go.mod: module tmp
    Downloading sigs.k8s.io/controller-tools/cmd/controller-gen@v0.8.0
    go get: installing executables with 'go get' in module mode is deprecated.
            To adjust and download dependencies of the current module, use 'go get -d'.
            To install using requirements of the current module, use 'go install'.
            To install ignoring the current module, use 'go install' with a version,
            like 'go install example.com/cmd@latest'.
            For more information, see https://golang.org/doc/go-get-install-deprecation
            or run 'go help get' or 'go help install'.
    go get: added github.com/fatih/color v1.12.0
    go get: added github.com/go-logr/logr v1.2.0
    go get: added github.com/gobuffalo/flect v0.2.3
    go get: added github.com/gogo/protobuf v1.3.2
    go get: added github.com/google/go-cmp v0.5.6
    go get: added github.com/google/gofuzz v1.1.0
    go get: added github.com/inconshreveable/mousetrap v1.0.0
    go get: added github.com/json-iterator/go v1.1.12
    go get: added github.com/mattn/go-colorable v0.1.8
    go get: added github.com/mattn/go-isatty v0.0.12
    go get: added github.com/modern-go/concurrent v0.0.0-20180306012644-bacd9c7ef1dd
    go get: added github.com/modern-go/reflect2 v1.0.2
    go get: added github.com/spf13/cobra v1.2.1
    go get: added github.com/spf13/pflag v1.0.5
    go get: added golang.org/x/mod v0.4.2
    go get: added golang.org/x/net v0.0.0-20210825183410-e898025ed96a
    go get: added golang.org/x/sys v0.0.0-20210831042530-f4d43177bf5e
    go get: added golang.org/x/text v0.3.7
    go get: added golang.org/x/tools v0.1.6-0.20210820212750-d4cc65f0b2ff
    go get: added golang.org/x/xerrors v0.0.0-20200804184101-5ec99f83aff1
    go get: added gopkg.in/inf.v0 v0.9.1
    go get: added gopkg.in/yaml.v2 v2.4.0
    go get: added gopkg.in/yaml.v3 v3.0.0-20210107192922-496545a6307b
    go get: added k8s.io/api v0.23.0
    go get: added k8s.io/apiextensions-apiserver v0.23.0
    go get: added k8s.io/apimachinery v0.23.0
    go get: added k8s.io/klog/v2 v2.30.0
    go get: added k8s.io/utils v0.0.0-20210930125809-cb0fa318a74b
    go get: added sigs.k8s.io/controller-tools v0.8.0
    go get: added sigs.k8s.io/json v0.0.0-20211020170558-c049b76a60c6
    go get: added sigs.k8s.io/structured-merge-diff/v4 v4.1.2
    go get: added sigs.k8s.io/yaml v1.3.0
    /Users/masato-naka/repos/nakamasato/kubebuilder-cronjob-controller/bin/controller-gen object:headerFile="hack/boilerplate.go.txt" paths="./..."
    Next: implement your new API and generate the manifests (e.g. CRDs,CRs) with:
    $ make manifests
    ```

    </details>

1. Check what's generated.

    ```
    tree -L 2
    .
    ├── Dockerfile
    ├── Makefile
    ├── PROJECT
    ├── README.md
    ├── api
    │   └── v1
    ├── bin
    │   └── controller-gen
    ├── config
    │   ├── crd
    │   ├── default
    │   ├── manager
    │   ├── prometheus
    │   ├── rbac
    │   └── samples
    ├── controllers
    │   ├── cronjob_controller.go
    │   └── suite_test.go
    ├── go.mod
    ├── go.sum
    ├── hack
    │   └── boilerplate.go.txt
    └── main.go

    12 directories, 11 files
    ```

1. Which files will be modified and which files will not be modified.

    Neither of the following files ever needs to be edited: [A Brief Aside: What’s the rest of this stuff?](https://book.kubebuilder.io/cronjob-tutorial/other-api-files.html)
    - `api/v1/groupversion_info.go`:
    - `api/v1/zz_generated.deepcopy.go`:

1. Commit the change.

### 5. [Designing an API](https://book.kubebuilder.io/cronjob-tutorial/api-design.html)

1. Define types for the Spec and Status of our Kind. Every functional object includes spec and status. (Exception: `ConfigMap` doesn't follow this pattern.) `api/v1/cronjob_types.go`

    - Spec: Desired state e.g. `CronJobSpec`
        - `Schedule`
        - `StartingDeadlineSeconds`
        - `ConcurrencyPolicy`
        - `Suspend`
        - `JobTemplate`
        - `SuccessfulJobsHistoryLimit`
        - `FailedJobsHistoryLimit`
    - Status: Actual state e.g. `CronJobStatus`
    - `CronJob`: contains `TypeMeta` (which describes API version and Kind) and `ObjectMeta` (which holds name, namespace and labels) <- Usually we don't modify
    - `CronJobList` <- Usually we don't modifiy
    - `+kubebuilder:object:root`: called a ***marker*** (telling [controller-tools](https://github.com/kubernetes-sigs/controller-tools) extra information)
    - `omitempty` struct tag to mark that a field should be omitted from serialization when empty.
    - `ConcurrencyPolicy` is just a string, but with a marker `+kubebuilder:validation:Enum=Allow;Forbid;Replace`, we can make it enum.

    <details><summary>For lazy people</summary>

    Just copy and paste the following codes to `api/v1/cronjob_types.go` (Update only `CronJobSpec` and `CronJobStatus`).

    ```go
    import (
        metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
        corev1 "k8s.io/api/core/v1"
        batchv1beta1 "k8s.io/api/batch/v1beta1"
    )
    ```

    ```go

    // ConcurrencyPolicy describes how the job will be handled.
    // Only one of the following concurrent policies may be specified.
    // If none of the following policies is specified, the default one
    // is AllowConcurrent.
    // +kubebuilder:validation:Enum=Allow;Forbid;Replace
    type ConcurrencyPolicy string

    const (
    	// AllowConcurrent allows CronJobs to run concurrently.
    	AllowConcurrent ConcurrencyPolicy = "Allow"

    	// ForbidConcurrent forbids concurrent runs, skipping next run if previous
    	// hasn't finished yet.
    	ForbidConcurrent ConcurrencyPolicy = "Forbid"

    	// ReplaceConcurrent cancels currently running job and replaces it with a new one.
    	ReplaceConcurrent ConcurrencyPolicy = "Replace"
    )

    // CronJobSpec defines the desired state of CronJob
    type CronJobSpec struct {
    	//+kubebuilder:validation:MinLength=0

    	// The schedule in Cron format, see https://en.wikipedia.org/wiki/Cron.
    	Schedule string `json:"schedule"`

    	//+kubebuilder:validation:Minimum=0

    	// Optional deadline in seconds for starting the job if it misses scheduled
    	// time for any reason.  Missed jobs executions will be counted as failed ones.
    	// +optional
    	StartingDeadlineSeconds *int64 `json:"startingDeadlineSeconds,omitempty"`
    	// Specifies how to treat concurrent executions of a Job.
    	// Valid values are:
    	// - "Allow" (default): allows CronJobs to run concurrently;
    	// - "Forbid": forbids concurrent runs, skipping next run if previous run hasn't finished yet;
    	// - "Replace": cancels currently running job and replaces it with a new one
    	// +optional
    	ConcurrencyPolicy ConcurrencyPolicy `json:"concurrencyPolicy,omitempty"`

    	// This flag tells the controller to suspend subsequent executions, it does
    	// not apply to already started executions.  Defaults to false.
    	// +optional
    	Suspend *bool `json:"suspend,omitempty"`
    	// Specifies the job that will be created when executing a CronJob.
    	JobTemplate batchv1beta1.JobTemplateSpec `json:"jobTemplate"`

    	//+kubebuilder:validation:Minimum=0

    	// The number of successful finished jobs to retain.
    	// This is a pointer to distinguish between explicit zero and not specified.
    	// +optional
    	SuccessfulJobsHistoryLimit *int32 `json:"successfulJobsHistoryLimit,omitempty"`

    	//+kubebuilder:validation:Minimum=0

    	// The number of failed finished jobs to retain.
    	// This is a pointer to distinguish between explicit zero and not specified.
    	// +optional
    	FailedJobsHistoryLimit *int32 `json:"failedJobsHistoryLimit,omitempty"`
    }

    // CronJobStatus defines the observed state of CronJob
    type CronJobStatus struct {
    	// A list of pointers to currently running jobs.
    	// +optional
    	Active []corev1.ObjectReference `json:"active,omitempty"`

    	// Information when was the last time the job was successfully scheduled.
    	// +optional
    	LastScheduleTime *metav1.Time `json:"lastScheduleTime,omitempty"`
    }
    ```

    </details>

1. Generate codes with `controller-gen`

    ```
    make generate
    /Users/masato-naka/repos/nakamasato/kubebuilder-cronjob-controller/bin/controller-gen object:headerFile="hack/boilerplate.go.txt" paths="./..."
    ```

1. Try running the operator in your local.

    ```
    make install run
    ```

    <details><summary>Result</summary>

    ```
    /Users/masato-naka/repos/nakamasato/kubebuilder-cronjob-controller/bin/controller-gen rbac:roleName=manager-role crd webhook paths="./..." output:crd:artifacts:config=config/crd/bases
    go: creating new go.mod: module tmp
    Downloading sigs.k8s.io/kustomize/kustomize/v3@v3.8.7
    go get: installing executables with 'go get' in module mode is deprecated.
            To adjust and download dependencies of the current module, use 'go get -d'.
            To install using requirements of the current module, use 'go install'.
            To install ignoring the current module, use 'go install' with a version,
            like 'go install example.com/cmd@latest'.
            For more information, see https://golang.org/doc/go-get-install-deprecation
            or run 'go help get' or 'go help install'.
    go get: added cloud.google.com/go v0.38.0
    go get: added github.com/Azure/go-autorest/autorest v0.9.0
    go get: added github.com/Azure/go-autorest/autorest/adal v0.5.0
    go get: added github.com/Azure/go-autorest/autorest/date v0.1.0
    go get: added github.com/Azure/go-autorest/logger v0.1.0
    go get: added github.com/Azure/go-autorest/tracing v0.5.0
    go get: added github.com/PuerkitoBio/purell v1.1.1
    go get: added github.com/PuerkitoBio/urlesc v0.0.0-20170810143723-de5bf2ad4578
    go get: added github.com/asaskevich/govalidator v0.0.0-20190424111038-f61b66f89f4a
    go get: added github.com/bgentry/go-netrc v0.0.0-20140422174119-9fd32a8b3d3d
    go get: added github.com/davecgh/go-spew v1.1.1
    go get: added github.com/dgrijalva/jwt-go v3.2.0+incompatible
    go get: added github.com/emicklei/go-restful v0.0.0-20170410110728-ff4f55a20633
    go get: added github.com/evanphx/json-patch v4.9.0+incompatible
    go get: added github.com/go-errors/errors v1.0.1
    go get: added github.com/go-openapi/analysis v0.19.5
    go get: added github.com/go-openapi/errors v0.19.2
    go get: added github.com/go-openapi/jsonpointer v0.19.3
    go get: added github.com/go-openapi/jsonreference v0.19.3
    go get: added github.com/go-openapi/loads v0.19.4
    go get: added github.com/go-openapi/runtime v0.19.4
    go get: added github.com/go-openapi/spec v0.19.5
    go get: added github.com/go-openapi/strfmt v0.19.5
    go get: added github.com/go-openapi/swag v0.19.5
    go get: added github.com/go-openapi/validate v0.19.8
    go get: added github.com/go-stack/stack v1.8.0
    go get: added github.com/gogo/protobuf v1.3.1
    go get: added github.com/golang/protobuf v1.3.2
    go get: added github.com/google/gofuzz v1.1.0
    go get: added github.com/google/shlex v0.0.0-20191202100458-e7afc7fbc510
    go get: added github.com/googleapis/gnostic v0.1.0
    go get: added github.com/gophercloud/gophercloud v0.1.0
    go get: added github.com/hashicorp/errwrap v1.0.0
    go get: added github.com/hashicorp/go-cleanhttp v0.5.0
    go get: added github.com/hashicorp/go-multierror v1.1.0
    go get: added github.com/hashicorp/go-safetemp v1.0.0
    go get: added github.com/hashicorp/go-version v1.1.0
    go get: added github.com/inconshreveable/mousetrap v1.0.0
    go get: added github.com/json-iterator/go v1.1.8
    go get: added github.com/mailru/easyjson v0.7.0
    go get: added github.com/mattn/go-runewidth v0.0.7
    go get: added github.com/mitchellh/go-homedir v1.1.0
    go get: added github.com/mitchellh/go-testing-interface v1.0.0
    go get: added github.com/mitchellh/mapstructure v1.1.2
    go get: added github.com/modern-go/concurrent v0.0.0-20180306012644-bacd9c7ef1dd
    go get: added github.com/modern-go/reflect2 v1.0.1
    go get: added github.com/monochromegane/go-gitignore v0.0.0-20200626010858-205db1a8cc00
    go get: added github.com/olekukonko/tablewriter v0.0.4
    go get: added github.com/pkg/errors v0.9.1
    go get: added github.com/pmezard/go-difflib v1.0.0
    go get: added github.com/qri-io/starlib v0.4.2-0.20200213133954-ff2e8cd5ef8d
    go get: added github.com/spf13/cobra v1.0.0
    go get: added github.com/spf13/pflag v1.0.5
    go get: added github.com/stretchr/testify v1.6.1
    go get: added github.com/ulikunitz/xz v0.5.5
    go get: added github.com/xlab/treeprint v0.0.0-20181112141820-a009c3971eca
    go get: added github.com/yujunz/go-getter v1.4.1-lite
    go get: added go.mongodb.org/mongo-driver v1.1.2
    go get: added go.starlark.net v0.0.0-20200306205701-8dd3e2ee1dd5
    go get: added golang.org/x/crypto v0.0.0-20200622213623-75b288015ac9
    go get: added golang.org/x/net v0.0.0-20200625001655-4c5254603344
    go get: added golang.org/x/oauth2 v0.0.0-20190604053449-0f29369cfe45
    go get: added golang.org/x/sys v0.0.0-20200323222414-85ca7c5b95cd
    go get: added golang.org/x/text v0.3.2
    go get: added golang.org/x/time v0.0.0-20190308202827-9d24e82272b4
    go get: added google.golang.org/appengine v1.5.0
    go get: added gopkg.in/inf.v0 v0.9.1
    go get: added gopkg.in/yaml.v2 v2.3.0
    go get: added gopkg.in/yaml.v3 v3.0.0-20200313102051-9f266ea9e77c
    go get: added k8s.io/api v0.18.10
    go get: added k8s.io/apimachinery v0.18.10
    go get: added k8s.io/client-go v0.18.10
    go get: added k8s.io/klog v1.0.0
    go get: added k8s.io/kube-openapi v0.0.0-20200410145947-61e04a5be9a6
    go get: added k8s.io/utils v0.0.0-20200324210504-a9aa75ae1b89
    go get: added sigs.k8s.io/kustomize/api v0.6.5
    go get: added sigs.k8s.io/kustomize/cmd/config v0.8.5
    go get: added sigs.k8s.io/kustomize/kustomize/v3 v3.8.7
    go get: added sigs.k8s.io/kustomize/kyaml v0.9.4
    go get: added sigs.k8s.io/structured-merge-diff/v3 v3.0.0
    go get: added sigs.k8s.io/yaml v1.2.0
    /Users/masato-naka/repos/nakamasato/kubebuilder-cronjob-controller/bin/kustomize build config/crd | kubectl apply -f -
    customresourcedefinition.apiextensions.k8s.io/cronjobs.batch.tutorial.kubebuilder.io configured
    /Users/masato-naka/repos/nakamasato/kubebuilder-cronjob-controller/bin/controller-gen object:headerFile="hack/boilerplate.go.txt" paths="./..."
    go fmt ./...
    go vet ./...
    go run ./main.go
    1.646696407505743e+09   INFO    controller-runtime.metrics      Metrics server is starting to listen    {"addr": ":8080"}
    1.646696407506041e+09   INFO    setup   starting manager
    1.646696407506217e+09   INFO    Starting server {"kind": "health probe", "addr": "[::]:8081"}
    1.646696407506217e+09   INFO    Starting server {"path": "/metrics", "kind": "metrics", "addr": "[::]:8080"}
    1.646696407506294e+09   INFO    controller.cronjob      Starting EventSource    {"reconciler group": "batch.tutorial.kubebuilder.io", "reconciler kind": "CronJob", "source": "kind source: *v1.CronJob"}
    1.646696407506331e+09   INFO    controller.cronjob      Starting Controller     {"reconciler group": "batch.tutorial.kubebuilder.io", "reconciler kind": "CronJob"}
    1.646696407607889e+09   INFO    controller.cronjob      Starting workers        {"reconciler group": "batch.tutorial.kubebuilder.io", "reconciler kind": "CronJob", "worker count": 1}
    ```

    </details>

1. Commit the change.

### 6. [What's in a controller (Reading)](https://book.kubebuilder.io/cronjob-tutorial/controller-overview.html)

### 7. [Implementing a controller](https://book.kubebuilder.io/cronjob-tutorial/controller-implementation.html)

> It’s a controller’s job to ensure that, for any given object, the actual state of the world matches the desired state in the object. Each controller focuses on one root Kind, but may interact with other Kinds.

code: https://github.com/kubernetes-sigs/kubebuilder/blob/master/docs/book/src/cronjob-tutorial/testdata/project/controllers/cronjob_controller.go

#### Reconcile

1. Load the named CronJob.
    ```go
	var cronJob batch.CronJob
	if err := r.Get(ctx, req.NamespacedName, &cronJob); err != nil {
		log.Error(err, "unable to fetch CronJob")
		// we'll ignore not-found errors, since they can't be fixed by an immediate
		// requeue (we'll need to wait for a new notification), and we can get them
		// on deleted requests.
		return ctrl.Result{}, client.IgnoreNotFound(err)
	}
    ```
1. List all active jobs, and update the status.

    <details><summary>code</summary>

    ```go
    var childJobs kbatch.JobList
    if err := r.List(ctx, &childJobs, client.InNamespace(req.Namespace), client.MatchingFields{jobOwnerKey: req.Name}); err != nil {
        log.Error(err, "unable to list child Jobs")
        return ctrl.Result{}, err
    }
    // find the active list of jobs
    var activeJobs []*kbatch.Job
    var successfulJobs []*kbatch.Job
    var failedJobs []*kbatch.Job
    var mostRecentTime *time.Time // find the last run so we can update the status

    isJobFinished := func(job *kbatch.Job) (bool, kbatch.JobConditionType) {
        for _, c := range job.Status.Conditions {
            if (c.Type == kbatch.JobComplete || c.Type == kbatch.JobFailed) && c.Status == corev1.ConditionTrue {
                return true, c.Type
            }
        }

        return false, ""
    }

    getScheduledTimeForJob := func(job *kbatch.Job) (*time.Time, error) {
        timeRaw := job.Annotations[scheduledTimeAnnotation]
        if len(timeRaw) == 0 {
            return nil, nil
        }

        timeParsed, err := time.Parse(time.RFC3339, timeRaw)
        if err != nil {
            return nil, err
        }
        return &timeParsed, nil
    }

    for i, job := range childJobs.Items {
        _, finishedType := isJobFinished(&job)
        switch finishedType {
        case "": // ongoing
            activeJobs = append(activeJobs, &childJobs.Items[i])
        case kbatch.JobFailed:
            failedJobs = append(failedJobs, &childJobs.Items[i])
        case kbatch.JobComplete:
            successfulJobs = append(successfulJobs, &childJobs.Items[i])
        }

        // We'll store the launch time in an annotation, so we'll reconstitute that from
        // the active jobs themselves.
        scheduledTimeForJob, err := getScheduledTimeForJob(&job)
        if err != nil {
            log.Error(err, "unable to parse schedule time for child job", "job", &job)
            continue
        }
        if scheduledTimeForJob != nil {
            if mostRecentTime == nil {
                mostRecentTime = scheduledTimeForJob
            } else if mostRecentTime.Before(*scheduledTimeForJob) {
                mostRecentTime = scheduledTimeForJob
            }
        }
    }

    if mostRecentTime != nil {
        cronJob.Status.LastScheduleTime = &metav1.Time{Time: *mostRecentTime}
    } else {
        cronJob.Status.LastScheduleTime = nil
    }
    cronJob.Status.Active = nil
    for _, activeJob := range activeJobs {
        jobRef, err := ref.GetReference(r.Scheme, activeJob)
        if err != nil {
            log.Error(err, "unable to make reference to active job", "job", activeJob)
            continue
        }
        cronJob.Status.Active = append(cronJob.Status.Active, *jobRef)
    }
    log.V(1).Info("job count", "active jobs", len(activeJobs), "successful jobs", len(successfulJobs), "failed jobs", len(failedJobs))
    if err := r.Status().Update(ctx, &cronJob); err != nil {
        log.Error(err, "unable to update CronJob status")
        return ctrl.Result{}, err
    }
    ```

    </details>

1. Clean up old jobs according to the history limits.

    <details><summary>failed jobs</summary>

    ```go
	if cronJob.Spec.FailedJobsHistoryLimit != nil {
		sort.Slice(failedJobs, func(i, j int) bool {
			if failedJobs[i].Status.StartTime == nil {
				return failedJobs[j].Status.StartTime != nil
			}
			return failedJobs[i].Status.StartTime.Before(failedJobs[j].Status.StartTime)
		})
		for i, job := range failedJobs {
			if int32(i) >= int32(len(failedJobs))-*cronJob.Spec.FailedJobsHistoryLimit {
				break
			}
			if err := r.Delete(ctx, job, client.PropagationPolicy(metav1.DeletePropagationBackground)); client.IgnoreNotFound(err) != nil {
				log.Error(err, "unable to delete old failed job", "job", job)
			} else {
				log.V(0).Info("deleted old failed job", "job", job)
			}
		}
	}
    ```

    </details>

    <details><summary>successful jobs</summary>

    ```go
	if cronJob.Spec.SuccessfulJobsHistoryLimit != nil {
		sort.Slice(successfulJobs, func(i, j int) bool {
			if successfulJobs[i].Status.StartTime == nil {
				return successfulJobs[j].Status.StartTime != nil
			}
			return successfulJobs[i].Status.StartTime.Before(successfulJobs[j].Status.StartTime)
		})
		for i, job := range successfulJobs {
			if int32(i) >= int32(len(successfulJobs))-*cronJob.Spec.SuccessfulJobsHistoryLimit {
				break
			}
			if err := r.Delete(ctx, job, client.PropagationPolicy(metav1.DeletePropagationBackground)); (err) != nil {
				log.Error(err, "unable to delete old successful job", "job", job)
			} else {
				log.V(0).Info("deleted old successful job", "job", job)
			}
		}
	}
    ```

    </details>

1. Check if we’re suspended (and don’t do anything else if we are).

    ```go
	if cronJob.Spec.Suspend != nil && *cronJob.Spec.Suspend {
		log.V(1).Info("cronjob suspended, skipping")
		return ctrl.Result{}, nil
	}
    ```

1. Get the next scheduled run.

    <details><summary>getNextSchedule()</summary>

    ```go
	getNextSchedule := func(cronJob *batch.CronJob, now time.Time) (lastMissed time.Time, next time.Time, err error) {
		sched, err := cron.ParseStandard(cronJob.Spec.Schedule)
		if err != nil {
			return time.Time{}, time.Time{}, fmt.Errorf("Unparseable schedule %q: %v", cronJob.Spec.Schedule, err)
		}

		// for optimization purposes, cheat a bit and start from our last observed run time
		// we could reconstitute this here, but there's not much point, since we've
		// just updated it.
		var earliestTime time.Time
		if cronJob.Status.LastScheduleTime != nil {
			earliestTime = cronJob.Status.LastScheduleTime.Time
		} else {
			earliestTime = cronJob.ObjectMeta.CreationTimestamp.Time
		}
		if cronJob.Spec.StartingDeadlineSeconds != nil {
			// controller is not going to schedule anything below this point
			schedulingDeadline := now.Add(-time.Second * time.Duration(*cronJob.Spec.StartingDeadlineSeconds))

			if schedulingDeadline.After(earliestTime) {
				earliestTime = schedulingDeadline
			}
		}
		if earliestTime.After(now) {
			return time.Time{}, sched.Next(now), nil
		}

		starts := 0
		for t := sched.Next(earliestTime); !t.After(now); t = sched.Next(t) {
			lastMissed = t
			// An object might miss several starts. For example, if
			// controller gets wedged on Friday at 5:01pm when everyone has
			// gone home, and someone comes in on Tuesday AM and discovers
			// the problem and restarts the controller, then all the hourly
			// jobs, more than 80 of them for one hourly scheduledJob, should
			// all start running with no further intervention (if the scheduledJob
			// allows concurrency and late starts).
			//
			// However, if there is a bug somewhere, or incorrect clock
			// on controller's server or apiservers (for setting creationTimestamp)
			// then there could be so many missed start times (it could be off
			// by decades or more), that it would eat up all the CPU and memory
			// of this controller. In that case, we want to not try to list
			// all the missed start times.
			starts++
			if starts > 100 {
				// We can't get the most recent times so just return an empty slice
				return time.Time{}, time.Time{}, fmt.Errorf("Too many missed start times (> 100). Set or decrease .spec.startingDeadlineSeconds or check clock skew.")
			}
		}
		return lastMissed, sched.Next(now), nil
	}
    ```

    </details>

    <details><summary>code</summary>

    ```go
	missedRun, nextRun, err := getNextSchedule(&cronJob, r.Now())
	if err != nil {
		log.Error(err, "unable to figure out CronJob schedule")
		// we don't really care about requeuing until we get an update that
		// fixes the schedule, so don't return an error
		return ctrl.Result{}, nil
	}
	scheduledResult := ctrl.Result{RequeueAfter: nextRun.Sub(r.Now())} // save this so we can re-use it elsewhere
	log = log.WithValues("now", r.Now(), "next run", nextRun)
    ```

    </details>

1. Run a new job if it’s on schedule, not past the deadline, and not blocked by our concurrency policy

    <details><summary>constructJobForCronJob</summary>

    ```go
	constructJobForCronJob := func(cronJob *batch.CronJob, scheduledTime time.Time) (*kbatch.Job, error) {
		// We want job names for a given nominal start time to have a deterministic name to avoid the same job being created twice
		name := fmt.Sprintf("%s-%d", cronJob.Name, scheduledTime.Unix())

		job := &kbatch.Job{
			ObjectMeta: metav1.ObjectMeta{
				Labels:      make(map[string]string),
				Annotations: make(map[string]string),
				Name:        name,
				Namespace:   cronJob.Namespace,
			},
			Spec: *cronJob.Spec.JobTemplate.Spec.DeepCopy(),
		}
		for k, v := range cronJob.Spec.JobTemplate.Annotations {
			job.Annotations[k] = v
		}
		job.Annotations[scheduledTimeAnnotation] = scheduledTime.Format(time.RFC3339)
		for k, v := range cronJob.Spec.JobTemplate.Labels {
			job.Labels[k] = v
		}
		if err := ctrl.SetControllerReference(cronJob, job, r.Scheme); err != nil {
			return nil, err
		}

		return job, nil
	}
    ```

    </details>


    <details><summary>code</summary>

    ```go
	if missedRun.IsZero() {
		log.V(1).Info("no upcoming scheduled times, sleeping until next")
		return scheduledResult, nil
	}

	// make sure we're not too late to start the run
	log = log.WithValues("current run", missedRun)
	tooLate := false
	if cronJob.Spec.StartingDeadlineSeconds != nil {
		tooLate = missedRun.Add(time.Duration(*cronJob.Spec.StartingDeadlineSeconds) * time.Second).Before(r.Now())
	}
	if tooLate {
		log.V(1).Info("missed starting deadline for last run, sleeping till next")
		// TODO(directxman12): events
		return scheduledResult, nil
	}

	// figure out how to run this job -- concurrency policy might forbid us from running
	// multiple at the same time...
	if cronJob.Spec.ConcurrencyPolicy == batch.ForbidConcurrent && len(activeJobs) > 0 {
		log.V(1).Info("concurrency policy blocks concurrent runs, skipping", "num active", len(activeJobs))
		return scheduledResult, nil
	}

	// ...or instruct us to replace existing ones...
	if cronJob.Spec.ConcurrencyPolicy == batch.ReplaceConcurrent {
		for _, activeJob := range activeJobs {
			// we don't care if the job was already deleted
			if err := r.Delete(ctx, activeJob, client.PropagationPolicy(metav1.DeletePropagationBackground)); client.IgnoreNotFound(err) != nil {
				log.Error(err, "unable to delete active job", "job", activeJob)
				return ctrl.Result{}, err
			}
		}
	}

	// actually make the job...
	job, err := constructJobForCronJob(&cronJob, missedRun)
	if err != nil {
		log.Error(err, "unable to construct job from template")
		// don't bother requeuing until we get a change to the spec
		return scheduledResult, nil
	}

	// ...and create it on the cluster
	if err := r.Create(ctx, job); err != nil {
		log.Error(err, "unable to create Job for CronJob", "job", job)
		return ctrl.Result{}, err
	}

	log.V(1).Info("created Job for CronJob run", "job", job)
    ```

    </details>

1. Requeue when we either see a running job (done automatically) or it’s time for the next scheduled run.

    ```go
    return scheduledResult, nil
    ```

#### Setup

1. Index to quickly look up Jobs by their owner.

    ```go
	if err := mgr.GetFieldIndexer().IndexField(context.Background(), &kbatch.Job{}, jobOwnerKey, func(rawObj client.Object) []string {
		// grab the job object, extract the owner...
		job := rawObj.(*kbatch.Job)
		owner := metav1.GetControllerOf(job)
		if owner == nil {
			return nil
		}
		// ...make sure it's a CronJob...
		if owner.APIVersion != apiGVStr || owner.Kind != "CronJob" {
			return nil
		}

		// ...and if so, return it
		return []string{owner.Name}
	}); err != nil {
		return err
	}
    ```

1. Inform the manager that this controller owns some Jobs

    ```go
    return ctrl.NewControllerManagedBy(mgr).
		For(&batch.CronJob{}).
		Owns(&kbatch.Job{}).
		Complete(r)
    ```

#### Final codes

<details><summary>controllers/cronjob_controller.go</summary>

```go
/*
Copyright 2022.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
*/

package controllers

import (
	"context"
	"fmt"
	"sort"
	"time"

	"github.com/go-logr/logr"
	"github.com/robfig/cron"
	kbatch "k8s.io/api/batch/v1"
	corev1 "k8s.io/api/core/v1"
	metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
	"k8s.io/apimachinery/pkg/runtime"
	ref "k8s.io/client-go/tools/reference"
	ctrl "sigs.k8s.io/controller-runtime"
	"sigs.k8s.io/controller-runtime/pkg/client"

	batch "tutorial.kubebuilder.io/api/v1"
)

type realClock struct{}

func (_ realClock) Now() time.Time { return time.Now() }

// clock knows how to get the current time.
// It can be used to fake out timing for testing.
type Clock interface {
	Now() time.Time
}

// CronJobReconciler reconciles a CronJob object
type CronJobReconciler struct {
	client.Client
	Log    logr.Logger
	Scheme *runtime.Scheme
	Clock
}

var (
	scheduledTimeAnnotation = "batch.tutorial.kubebuilder.io/scheduled-at"
)

//+kubebuilder:rbac:groups=batch.tutorial.kubebuilder.io,resources=cronjobs,verbs=get;list;watch;create;update;patch;delete
//+kubebuilder:rbac:groups=batch.tutorial.kubebuilder.io,resources=cronjobs/status,verbs=get;update;patch
//+kubebuilder:rbac:groups=batch.tutorial.kubebuilder.io,resources=cronjobs/finalizers,verbs=update
//+kubebuilder:rbac:groups=batch,resources=jobs,verbs=get;list;watch;create;update;patch;delete
//+kubebuilder:rbac:groups=batch,resources=jobs/status,verbs=get

// Reconcile is part of the main kubernetes reconciliation loop which aims to
// move the current state of the cluster closer to the desired state.
// TODO(user): Modify the Reconcile function to compare the state specified by
// the CronJob object against the actual cluster state, and then
// perform operations to make the cluster state reflect the state specified by
// the user.
//
// For more details, check Reconcile and its Result here:
// - https://pkg.go.dev/sigs.k8s.io/controller-runtime@v0.8.3/pkg/reconcile
func (r *CronJobReconciler) Reconcile(ctx context.Context, req ctrl.Request) (ctrl.Result, error) {
	log, err := logr.FromContext(ctx)

	// 1. Load the named CronJob
	var cronJob batch.CronJob
	if err := r.Get(ctx, req.NamespacedName, &cronJob); err != nil {
		log.Error(err, "unable to fetch CronJob")
		// we'll ignore not-found errors, since they can't be fixed by an immediate
		// requeue (we'll need to wait for a new notification), and we can get them
		// on deleted requests.
		return ctrl.Result{}, client.IgnoreNotFound(err)
	}

	// 2. List all active jobs, and update the status
	var childJobs kbatch.JobList
	if err := r.List(ctx, &childJobs, client.InNamespace(req.Namespace), client.MatchingFields{jobOwnerKey: req.Name}); err != nil {
		log.Error(err, "unable to list child Jobs")
		return ctrl.Result{}, err
	}
	// find the active list of jobs
	var activeJobs []*kbatch.Job
	var successfulJobs []*kbatch.Job
	var failedJobs []*kbatch.Job
	var mostRecentTime *time.Time // find the last run so we can update the status

	isJobFinished := func(job *kbatch.Job) (bool, kbatch.JobConditionType) {
		for _, c := range job.Status.Conditions {
			if (c.Type == kbatch.JobComplete || c.Type == kbatch.JobFailed) && c.Status == corev1.ConditionTrue {
				return true, c.Type
			}
		}

		return false, ""
	}

	getScheduledTimeForJob := func(job *kbatch.Job) (*time.Time, error) {
		timeRaw := job.Annotations[scheduledTimeAnnotation]
		if len(timeRaw) == 0 {
			return nil, nil
		}

		timeParsed, err := time.Parse(time.RFC3339, timeRaw)
		if err != nil {
			return nil, err
		}
		return &timeParsed, nil
	}

	for i, job := range childJobs.Items {
		_, finishedType := isJobFinished(&job)
		switch finishedType {
		case "": // ongoing
			activeJobs = append(activeJobs, &childJobs.Items[i])
		case kbatch.JobFailed:
			failedJobs = append(failedJobs, &childJobs.Items[i])
		case kbatch.JobComplete:
			successfulJobs = append(successfulJobs, &childJobs.Items[i])
		}

		// We'll store the launch time in an annotation, so we'll reconstitute that from
		// the active jobs themselves.
		scheduledTimeForJob, err := getScheduledTimeForJob(&job)
		if err != nil {
			log.Error(err, "unable to parse schedule time for child job", "job", &job)
			continue
		}
		if scheduledTimeForJob != nil {
			if mostRecentTime == nil {
				mostRecentTime = scheduledTimeForJob
			} else if mostRecentTime.Before(*scheduledTimeForJob) {
				mostRecentTime = scheduledTimeForJob
			}
		}
	}

	if mostRecentTime != nil {
		cronJob.Status.LastScheduleTime = &metav1.Time{Time: *mostRecentTime}
	} else {
		cronJob.Status.LastScheduleTime = nil
	}
	cronJob.Status.Active = nil
	for _, activeJob := range activeJobs {
		jobRef, err := ref.GetReference(r.Scheme, activeJob)
		if err != nil {
			log.Error(err, "unable to make reference to active job", "job", activeJob)
			continue
		}
		cronJob.Status.Active = append(cronJob.Status.Active, *jobRef)
	}
	log.V(1).Info("job count", "active jobs", len(activeJobs), "successful jobs", len(successfulJobs), "failed jobs", len(failedJobs))
	if err := r.Status().Update(ctx, &cronJob); err != nil {
		log.Error(err, "unable to update CronJob status")
		return ctrl.Result{}, err
	}

	// 3. Clean up old jobs according to the history limits
	// NB: deleting these is "best effort" -- if we fail on a particular one,
	// we won't requeue just to finish the deleting.
	if cronJob.Spec.FailedJobsHistoryLimit != nil {
		sort.Slice(failedJobs, func(i, j int) bool {
			if failedJobs[i].Status.StartTime == nil {
				return failedJobs[j].Status.StartTime != nil
			}
			return failedJobs[i].Status.StartTime.Before(failedJobs[j].Status.StartTime)
		})
		for i, job := range failedJobs {
			if int32(i) >= int32(len(failedJobs))-*cronJob.Spec.FailedJobsHistoryLimit {
				break
			}
			if err := r.Delete(ctx, job, client.PropagationPolicy(metav1.DeletePropagationBackground)); client.IgnoreNotFound(err) != nil {
				log.Error(err, "unable to delete old failed job", "job", job)
			} else {
				log.V(0).Info("deleted old failed job", "job", job)
			}
		}
	}

	if cronJob.Spec.SuccessfulJobsHistoryLimit != nil {
		sort.Slice(successfulJobs, func(i, j int) bool {
			if successfulJobs[i].Status.StartTime == nil {
				return successfulJobs[j].Status.StartTime != nil
			}
			return successfulJobs[i].Status.StartTime.Before(successfulJobs[j].Status.StartTime)
		})
		for i, job := range successfulJobs {
			if int32(i) >= int32(len(successfulJobs))-*cronJob.Spec.SuccessfulJobsHistoryLimit {
				break
			}
			if err := r.Delete(ctx, job, client.PropagationPolicy(metav1.DeletePropagationBackground)); (err) != nil {
				log.Error(err, "unable to delete old successful job", "job", job)
			} else {
				log.V(0).Info("deleted old successful job", "job", job)
			}
		}
	}

	// 4. Check if we're suspended
	if cronJob.Spec.Suspend != nil && *cronJob.Spec.Suspend {
		log.V(1).Info("cronjob suspended, skipping")
		return ctrl.Result{}, nil
	}

	// 5. Get the next schedule run
	getNextSchedule := func(cronJob *batch.CronJob, now time.Time) (lastMissed time.Time, next time.Time, err error) {
		sched, err := cron.ParseStandard(cronJob.Spec.Schedule)
		if err != nil {
			return time.Time{}, time.Time{}, fmt.Errorf("Unparseable schedule %q: %v", cronJob.Spec.Schedule, err)
		}

		// for optimization purposes, cheat a bit and start from our last observed run time
		// we could reconstitute this here, but there's not much point, since we've
		// just updated it.
		var earliestTime time.Time
		if cronJob.Status.LastScheduleTime != nil {
			earliestTime = cronJob.Status.LastScheduleTime.Time
		} else {
			earliestTime = cronJob.ObjectMeta.CreationTimestamp.Time
		}
		if cronJob.Spec.StartingDeadlineSeconds != nil {
			// controller is not going to schedule anything below this point
			schedulingDeadline := now.Add(-time.Second * time.Duration(*cronJob.Spec.StartingDeadlineSeconds))

			if schedulingDeadline.After(earliestTime) {
				earliestTime = schedulingDeadline
			}
		}
		if earliestTime.After(now) {
			return time.Time{}, sched.Next(now), nil
		}

		starts := 0
		for t := sched.Next(earliestTime); !t.After(now); t = sched.Next(t) {
			lastMissed = t
			// An object might miss several starts. For example, if
			// controller gets wedged on Friday at 5:01pm when everyone has
			// gone home, and someone comes in on Tuesday AM and discovers
			// the problem and restarts the controller, then all the hourly
			// jobs, more than 80 of them for one hourly scheduledJob, should
			// all start running with no further intervention (if the scheduledJob
			// allows concurrency and late starts).
			//
			// However, if there is a bug somewhere, or incorrect clock
			// on controller's server or apiservers (for setting creationTimestamp)
			// then there could be so many missed start times (it could be off
			// by decades or more), that it would eat up all the CPU and memory
			// of this controller. In that case, we want to not try to list
			// all the missed start times.
			starts++
			if starts > 100 {
				// We can't get the most recent times so just return an empty slice
				return time.Time{}, time.Time{}, fmt.Errorf("Too many missed start times (> 100). Set or decrease .spec.startingDeadlineSeconds or check clock skew.")
			}
		}
		return lastMissed, sched.Next(now), nil
	}

	// figure out the next times that we need to create
	// jobs at (or anything we missed).
	missedRun, nextRun, err := getNextSchedule(&cronJob, r.Now())
	if err != nil {
		log.Error(err, "unable to figure out CronJob schedule")
		// we don't really care about requeuing until we get an update that
		// fixes the schedule, so don't return an error
		return ctrl.Result{}, nil
	}
	scheduledResult := ctrl.Result{RequeueAfter: nextRun.Sub(r.Now())} // save this so we can re-use it elsewhere
	log = log.WithValues("now", r.Now(), "next run", nextRun)

	// 6. Run a new job if it's on schedule, not past the deadline, and not blocked by our concurrency policy
	if missedRun.IsZero() {
		log.V(1).Info("no upcoming scheduled times, sleeping until next")
		return scheduledResult, nil
	}

	// make sure we're not too late to start the run
	log = log.WithValues("current run", missedRun)
	tooLate := false
	if cronJob.Spec.StartingDeadlineSeconds != nil {
		tooLate = missedRun.Add(time.Duration(*cronJob.Spec.StartingDeadlineSeconds) * time.Second).Before(r.Now())
	}
	if tooLate {
		log.V(1).Info("missed starting deadline for last run, sleeping till next")
		// TODO(directxman12): events
		return scheduledResult, nil
	}

	// figure out how to run this job -- concurrency policy might forbid us from running
	// multiple at the same time...
	if cronJob.Spec.ConcurrencyPolicy == batch.ForbidConcurrent && len(activeJobs) > 0 {
		log.V(1).Info("concurrency policy blocks concurrent runs, skipping", "num active", len(activeJobs))
		return scheduledResult, nil
	}

	// ...or instruct us to replace existing ones...
	if cronJob.Spec.ConcurrencyPolicy == batch.ReplaceConcurrent {
		for _, activeJob := range activeJobs {
			// we don't care if the job was already deleted
			if err := r.Delete(ctx, activeJob, client.PropagationPolicy(metav1.DeletePropagationBackground)); client.IgnoreNotFound(err) != nil {
				log.Error(err, "unable to delete active job", "job", activeJob)
				return ctrl.Result{}, err
			}
		}
	}

	constructJobForCronJob := func(cronJob *batch.CronJob, scheduledTime time.Time) (*kbatch.Job, error) {
		// We want job names for a given nominal start time to have a deterministic name to avoid the same job being created twice
		name := fmt.Sprintf("%s-%d", cronJob.Name, scheduledTime.Unix())

		job := &kbatch.Job{
			ObjectMeta: metav1.ObjectMeta{
				Labels:      make(map[string]string),
				Annotations: make(map[string]string),
				Name:        name,
				Namespace:   cronJob.Namespace,
			},
			Spec: *cronJob.Spec.JobTemplate.Spec.DeepCopy(),
		}
		for k, v := range cronJob.Spec.JobTemplate.Annotations {
			job.Annotations[k] = v
		}
		job.Annotations[scheduledTimeAnnotation] = scheduledTime.Format(time.RFC3339)
		for k, v := range cronJob.Spec.JobTemplate.Labels {
			job.Labels[k] = v
		}
		if err := ctrl.SetControllerReference(cronJob, job, r.Scheme); err != nil {
			return nil, err
		}

		return job, nil
	}

	// actually make the job...
	job, err := constructJobForCronJob(&cronJob, missedRun)
	if err != nil {
		log.Error(err, "unable to construct job from template")
		// don't bother requeuing until we get a change to the spec
		return scheduledResult, nil
	}

	// ...and create it on the cluster
	if err := r.Create(ctx, job); err != nil {
		log.Error(err, "unable to create Job for CronJob", "job", job)
		return ctrl.Result{}, err
	}

	log.V(1).Info("created Job for CronJob run", "job", job)

	// 7. Requeue when we either see a running job or it’s time for the next scheduled run
	return ctrl.Result{}, nil
}

var (
	jobOwnerKey = ".metadata.controller"
	apiGVStr    = batch.GroupVersion.String()
)

// SetupWithManager sets up the controller with the Manager.
func (r *CronJobReconciler) SetupWithManager(mgr ctrl.Manager) error {
	// set up a real clock, since we're not in a test
	if r.Clock == nil {
		r.Clock = realClock{}
	}

	if err := mgr.GetFieldIndexer().IndexField(context.Background(), &kbatch.Job{}, jobOwnerKey, func(rawObj client.Object) []string {
		// grab the job object, extract the owner...
		job := rawObj.(*kbatch.Job)
		owner := metav1.GetControllerOf(job)
		if owner == nil {
			return nil
		}
		// ...make sure it's a CronJob...
		if owner.APIVersion != apiGVStr || owner.Kind != "CronJob" {
			return nil
		}

		// ...and if so, return it
		return []string{owner.Name}
	}); err != nil {
		return err
	}

	return ctrl.NewControllerManagedBy(mgr).
		For(&batch.CronJob{}).
		Owns(&kbatch.Job{}).
		Complete(r)
}
```

</details>

1. Generate rbac yaml file from the markers.

    ```
    make manifests
    ```

1. Commit the change.

### 8. [Implementing defaulting/validating webhooks](https://book.kubebuilder.io/cronjob-tutorial/webhook-implementation.html#implementing-defaultingvalidating-webhooks)


The only thing you need to do is to implement the `Defaulter` and (or) the `Validator` interface.

kubebuilder does:

1. Creating the webhook server.
1. Ensuring the server has been added in the manager.
1. Creating handlers for your webhooks.
1. Registering each handler with a path in your server.

Steps:
1. Create webhook by `kubebuilder`.

    ```
    kubebuilder create webhook --group batch --version v1 --kind CronJob --defaulting --programmatic-validation
    ```

    <details><summary>Result</summary>

    ```
    Writing kustomize manifests for you to edit...
    Writing scaffold for you to edit...
    api/v1/cronjob_webhook.go
    Update dependencies:
    $ go mod tidy
    Running make:
    $ make generate
    /Users/masato-naka/repos/nakamasato/kubebuilder-cronjob-controller/bin/controller-gen object:headerFile="hack/boilerplate.go.txt" paths="./..."
    Next: implement your new Webhook and generate the manifests with:
    $ make manifests
    ```

    </details>

1. Write logic in `Default()` in `cronjob_webhook.go`.

    ```go
    if r.Spec.ConcurrencyPolicy == "" {
        r.Spec.ConcurrencyPolicy = AllowConcurrent
    }
    if r.Spec.Suspend == nil {
        r.Spec.Suspend = new(bool)
    }
    if r.Spec.SuccessfulJobsHistoryLimit == nil {
        r.Spec.SuccessfulJobsHistoryLimit = new(int32)
        *r.Spec.SuccessfulJobsHistoryLimit = 3
    }
    if r.Spec.FailedJobsHistoryLimit == nil {
        r.Spec.FailedJobsHistoryLimit = new(int32)
        *r.Spec.FailedJobsHistoryLimit = 1
    }
    ```
1. Manually update `main.go`.
    ```go
	if os.Getenv("ENABLE_WEBHOOKS") != "false" {
		if err = (&batchv1.CronJob{}).SetupWebhookWithManager(mgr); err != nil {
			setupLog.Error(err, "unable to create webhook", "webhook", "CronJob")
			os.Exit(1)
		}
	}
    ```
#### Final codes

<details><summary>api/v1/cronjob_webhook.go</summary>

```go
/*
Copyright 2022.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
*/

package v1

import (
	"github.com/robfig/cron"
	apierrors "k8s.io/apimachinery/pkg/api/errors"
	"k8s.io/apimachinery/pkg/runtime"
	"k8s.io/apimachinery/pkg/runtime/schema"
	"k8s.io/apimachinery/pkg/util/validation/field"
	ctrl "sigs.k8s.io/controller-runtime"
	logf "sigs.k8s.io/controller-runtime/pkg/log"
	"sigs.k8s.io/controller-runtime/pkg/webhook"
)

// log is for logging in this package.
var cronjoblog = logf.Log.WithName("cronjob-resource")

func (r *CronJob) SetupWebhookWithManager(mgr ctrl.Manager) error {
	return ctrl.NewWebhookManagedBy(mgr).
		For(r).
		Complete()
}

// EDIT THIS FILE!  THIS IS SCAFFOLDING FOR YOU TO OWN!

//+kubebuilder:webhook:path=/mutate-batch-tutorial-kubebuilder-io-v1-cronjob,mutating=true,failurePolicy=fail,sideEffects=None,groups=batch.tutorial.kubebuilder.io,resources=cronjobs,verbs=create;update,versions=v1,name=mcronjob.kb.io,admissionReviewVersions={v1,v1beta1}

var _ webhook.Defaulter = &CronJob{}

// Default implements webhook.Defaulter so a webhook will be registered for the type
func (r *CronJob) Default() {
	cronjoblog.Info("default", "name", r.Name)

	if r.Spec.ConcurrencyPolicy == "" {
		r.Spec.ConcurrencyPolicy = AllowConcurrent
	}
	if r.Spec.Suspend == nil {
		r.Spec.Suspend = new(bool)
	}
	if r.Spec.SuccessfulJobsHistoryLimit == nil {
		r.Spec.SuccessfulJobsHistoryLimit = new(int32)
		*r.Spec.SuccessfulJobsHistoryLimit = 3
	}
	if r.Spec.FailedJobsHistoryLimit == nil {
		r.Spec.FailedJobsHistoryLimit = new(int32)
		*r.Spec.FailedJobsHistoryLimit = 1
	}
}

// TODO(user): change verbs to "verbs=create;update;delete" if you want to enable deletion validation.
//+kubebuilder:webhook:path=/validate-batch-tutorial-kubebuilder-io-v1-cronjob,mutating=false,failurePolicy=fail,sideEffects=None,groups=batch.tutorial.kubebuilder.io,resources=cronjobs,verbs=create;update,versions=v1,name=vcronjob.kb.io,admissionReviewVersions={v1,v1beta1}

var _ webhook.Validator = &CronJob{}

// ValidateCreate implements webhook.Validator so a webhook will be registered for the type
func (r *CronJob) ValidateCreate() error {
	cronjoblog.Info("validate create", "name", r.Name)

	// TODO(user): fill in your validation logic upon object creation.
	return r.validateCronJob()
}

// ValidateUpdate implements webhook.Validator so a webhook will be registered for the type
func (r *CronJob) ValidateUpdate(old runtime.Object) error {
	cronjoblog.Info("validate update", "name", r.Name)

	// TODO(user): fill in your validation logic upon object update.
	return r.validateCronJob()
}

// ValidateDelete implements webhook.Validator so a webhook will be registered for the type
func (r *CronJob) ValidateDelete() error {
	cronjoblog.Info("validate delete", "name", r.Name)

	// TODO(user): fill in your validation logic upon object deletion.
	return nil
}

func (r *CronJob) validateCronJob() error {
	var allErrs field.ErrorList
	if err := r.validateCronJobName(); err != nil {
		allErrs = append(allErrs, err)
	}
	if err := r.validateCronJobSpec(); err != nil {
		allErrs = append(allErrs, err)
	}
	if len(allErrs) == 0 {
		return nil
	}
	return apierrors.NewInvalid(
		schema.GroupKind{Group: "batch.tutorial.kubebuilder.io", Kind: "CronJob"},
		r.Name, allErrs)
}

func (r *CronJob) validateCronJobName() *field.Error {
	return nil
}

func (r *CronJob) validateCronJobSpec() *field.Error {
	return validateScheduleFormat(
		r.Spec.Schedule,
		field.NewPath("spec").Child("schedule"))
}

func validateScheduleFormat(schedule string, fldPath *field.Path) *field.Error {
	if _, err := cron.ParseStandard(schedule); err != nil {
		return field.Invalid(fldPath, schedule, err.Error())
	}
	return nil
}
```

</details>

```
make manifests
```

### 9. [Run the controller](https://book.kubebuilder.io/cronjob-tutorial/running.html)

**Important!**

> If you want to run the webhooks locally, you’ll have to generate certificates for serving the webhooks, and place them in the right directory (/tmp/k8s-webhook-server/serving-certs/tls.{crt,key}, by default).
> If you’re not running a local API server, you’ll also need to figure out how to proxy traffic from the remote cluster to your local webhook server. For this reason, we generally recommend disabling webhooks when doing your local code-run-test cycle, as we do below.

#### Run with `make run` (without webhooks)
1. Run without webhooks

    ```
    make install run ENABLE_WEBHOOKS=false
    ```

    which is enabled by the following code:

    <details><summary>ENABLE_WEBHOOKS</summary>

    ```go
    if os.Getenv("ENABLE_WEBHOOKS") != "false" {
        if err = (&batchv1.CronJob{}).SetupWebhookWithManager(mgr); err != nil {
            setupLog.Error(err, "unable to create webhook", "webhook", "CronJob")
            os.Exit(1)
        }
    }
    ```

    </details>
1. Create `Cronjob`.
    ```
    kubectl create -f config/samples/batch_v1_cronjob.yaml
    ```
1. Check.
    ```
    kubectl get cronjob.batch.tutorial.kubebuilder.io
    NAME             AGE
    cronjob-sample   19s
    ```

    ```
    kubectl get job
    NAME                        COMPLETIONS   DURATION   AGE
    cronjob-sample-1646616960   0/1           1s         1s
    ```

    ```
    kubectl get po
    NAME                                       READY   STATUS              RESTARTS   AGE
    cronjob-sample-1646616960-nph7l            0/1     ContainerCreating   0          4s
    ```

1. Delete `CronJob`.
    ```
    kubectl delete -f config/samples/batch_v1_cronjob.yaml
    ```
1. Stop `make run`.

#### Run in Kubernetes cluster (with webhook)
1. Build docker image

    - If you use a container image registry:

        ```
        IMG=<some-registry>/<project-name>:tag
        make docker-build docker-push IMG=$IMG
        ```
    - If you test with Kubernetes cluster created by [kind](https://kind.sigs.k8s.io/):

        ```
        IMG=kubebuilder-cronjob-controller:latest
        make docker-build IMG=$IMG
        kind load docker-image $IMG
        ```

        Also need to add `imagePullPolicy: NotIfPresent` to [config/manager/manager.yaml](config/manager/manager.yaml)

1. Install cert-manager

    ```
    kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.7.1/cert-manager.yaml
    ```

    <details><summary>result</summary>

    ```
    customresourcedefinition.apiextensions.k8s.io/certificaterequests.cert-manager.io created
    customresourcedefinition.apiextensions.k8s.io/certificates.cert-manager.io created
    customresourcedefinition.apiextensions.k8s.io/challenges.acme.cert-manager.io created
    customresourcedefinition.apiextensions.k8s.io/clusterissuers.cert-manager.io created
    customresourcedefinition.apiextensions.k8s.io/issuers.cert-manager.io created
    customresourcedefinition.apiextensions.k8s.io/orders.acme.cert-manager.io created
    namespace/cert-manager created
    serviceaccount/cert-manager-cainjector created
    serviceaccount/cert-manager created
    serviceaccount/cert-manager-webhook created
    configmap/cert-manager-webhook created
    clusterrole.rbac.authorization.k8s.io/cert-manager-cainjector created
    clusterrole.rbac.authorization.k8s.io/cert-manager-controller-issuers created
    clusterrole.rbac.authorization.k8s.io/cert-manager-controller-clusterissuers created
    clusterrole.rbac.authorization.k8s.io/cert-manager-controller-certificates created
    clusterrole.rbac.authorization.k8s.io/cert-manager-controller-orders created
    clusterrole.rbac.authorization.k8s.io/cert-manager-controller-challenges created
    clusterrole.rbac.authorization.k8s.io/cert-manager-controller-ingress-shim created
    clusterrole.rbac.authorization.k8s.io/cert-manager-view created
    clusterrole.rbac.authorization.k8s.io/cert-manager-edit created
    clusterrole.rbac.authorization.k8s.io/cert-manager-controller-approve:cert-manager-io created
    clusterrole.rbac.authorization.k8s.io/cert-manager-controller-certificatesigningrequests created
    ```

    </details>

1. Add webhook.

    `config/crd/kustomization.yaml`:

    <details><summary>result</summary>

    ```diff
    --- a/config/crd/kustomization.yaml
    +++ b/config/crd/kustomization.yaml
    @@ -8,12 +8,12 @@ resources:
     patchesStrategicMerge:
     # [WEBHOOK] To enable webhook, uncomment all the sections with [WEBHOOK] prefix.
     # patches here are for enabling the conversion webhook for each CRD
    -#- patches/webhook_in_cronjobs.yaml
    +- patches/webhook_in_cronjobs.yaml
     #+kubebuilder:scaffold:crdkustomizewebhookpatch

     # [CERTMANAGER] To enable webhook, uncomment all the sections with [CERTMANAGER] prefix.
     # patches here are for enabling the CA injection for each CRD
    -#- patches/cainjection_in_cronjobs.yaml
    +- patches/cainjection_in_cronjobs.yaml
     #+kubebuilder:scaffold:crdkustomizecainjectionpatch
    ```

    </details>

    `config/default/kustomization.yaml`:

    <details><summary>result</summary>

    ```diff
    -namePrefix: kubebuilder-cronjob-controller-
    +namePrefix: kubebuilder-cronjob-

     # Labels to add to all resources and selectors.
     #commonLabels:
    @@ -18,9 +18,9 @@ bases:
     - ../manager
     # [WEBHOOK] To enable webhook, uncomment all the sections with [WEBHOOK] prefix including the one in
     # crd/kustomization.yaml
    -#- ../webhook
    +- ../webhook
     # [CERTMANAGER] To enable cert-manager, uncomment all sections with 'CERTMANAGER'. 'WEBHOOK' components are required.
    -#- ../certmanager
    +- ../certmanager
     # [PROMETHEUS] To enable prometheus monitor, uncomment all sections with 'PROMETHEUS'.
     #- ../prometheus

    @@ -36,39 +36,39 @@ patchesStrategicMerge:
     # [WEBHOOK] To enable webhook, uncomment all the sections with [WEBHOOK] prefix including the one in
     # crd/kustomization.yaml
    -#- manager_webhook_patch.yaml
    +- manager_webhook_patch.yaml

     # [CERTMANAGER] To enable cert-manager, uncomment all sections with 'CERTMANAGER'.
     # Uncomment 'CERTMANAGER' sections in crd/kustomization.yaml to enable the CA injection in the admission webhooks.
     # 'CERTMANAGER' needs to be enabled to use ca injection
    -#- webhookcainjection_patch.yaml
    +- webhookcainjection_patch.yaml

     # the following config is for teaching kustomize how to do var substitution
     vars:
     # [CERTMANAGER] To enable cert-manager, uncomment all sections with 'CERTMANAGER' prefix.
    -#- name: CERTIFICATE_NAMESPACE # namespace of the certificate CR
    -#  objref:
    -#    kind: Certificate
    -#    group: cert-manager.io
    -#    version: v1
    -#    name: serving-cert # this name should match the one in certificate.yaml
    -#  fieldref:
    -#    fieldpath: metadata.namespace
    -#- name: CERTIFICATE_NAME
    -#  objref:
    -#    kind: Certificate
    -#    group: cert-manager.io
    -#    version: v1
    -#    name: serving-cert # this name should match the one in certificate.yaml
    -#- name: SERVICE_NAMESPACE # namespace of the service
    -#  objref:
    -#    kind: Service
    -#    version: v1
    -#    name: webhook-service
    -#  fieldref:
    -#    fieldpath: metadata.namespace
    -#- name: SERVICE_NAME
    -#  objref:
    -#    kind: Service
    -#    version: v1
    -#    name: webhook-service
    +- name: CERTIFICATE_NAMESPACE # namespace of the certificate CR
    +  objref:
    +    kind: Certificate
    +    group: cert-manager.io
    +    version: v1
    +    name: serving-cert # this name should match the one in certificate.yaml
    +  fieldref:
    +    fieldpath: metadata.namespace
    +- name: CERTIFICATE_NAME
    +  objref:
    +    kind: Certificate
    +    group: cert-manager.io
    +    version: v1
    +    name: serving-cert # this name should match the one in certificate.yaml
    +- name: SERVICE_NAMESPACE # namespace of the service
    +  objref:
    +    kind: Service
    +    version: v1
    +    name: webhook-service
    +  fieldref:
    +    fieldpath: metadata.namespace
    +- name: SERVICE_NAME
    +  objref:
    +    kind: Service
    +    version: v1
    +    name: webhook-service
    ```

    </details>

1. Deploy.

    ```
    make deploy IMG=$IMG
    ```

    <details><summary>result</summary>

    ```
    /Users/masato-naka/repos/nakamasato/kubebuilder-cronjob-controller/bin/controller-gen "crd:trivialVersions=true,preserveUnknownFields=false" rbac:roleName=manager-role webhook paths="./..." output:crd:artifacts:config=config/crd/bases
    cd config/manager && /Users/masato-naka/repos/nakamasato/kubebuilder-cronjob-controller/bin/kustomize edit set image controller=kubebuilder-cronjob-controller:latest
    /Users/masato-naka/repos/nakamasato/kubebuilder-cronjob-controller/bin/kustomize build config/default | kubectl apply -f -
    namespace/kubebuilder-cronjob-controller-system created
    customresourcedefinition.apiextensions.k8s.io/cronjobs.batch.tutorial.kubebuilder.io created
    serviceaccount/kubebuilder-cronjob-controller-manager created
    role.rbac.authorization.k8s.io/kubebuilder-cronjob-leader-election-role created
    clusterrole.rbac.authorization.k8s.io/kubebuilder-cronjob-manager-role created
    clusterrole.rbac.authorization.k8s.io/kubebuilder-cronjob-metrics-reader created
    clusterrole.rbac.authorization.k8s.io/kubebuilder-cronjob-proxy-role created
    rolebinding.rbac.authorization.k8s.io/kubebuilder-cronjob-leader-election-rolebinding created
    clusterrolebinding.rbac.authorization.k8s.io/kubebuilder-cronjob-manager-rolebinding created
    clusterrolebinding.rbac.authorization.k8s.io/kubebuilder-cronjob-proxy-rolebinding created
    configmap/kubebuilder-cronjob-manager-config created
    service/kubebuilder-cronjob-controller-manager-metrics-service created
    service/kubebuilder-cronjob-webhook-service created
    deployment.apps/kubebuilder-cronjob-controller-manager created
    certificate.cert-manager.io/kubebuilder-cronjob-serving-cert created
    issuer.cert-manager.io/kubebuilder-cronjob-selfsigned-issuer created
    mutatingwebhookconfiguration.admissionregistration.k8s.io/kubebuilder-cronjob-mutating-webhook-configuration created
    validatingwebhookconfiguration.admissionregistration.k8s.io/kubebuilder-cronjob-validating-webhook-configuration created
    ```

    </details>

    Resources related to the webhook and certificate:
    1. `certificate.cert-manager.io/kubebuilder-cronjob-serving-cert`
    1. `issuer.cert-manager.io/kubebuilder-cronjob-selfsigned-issuer`
    1. `mutatingwebhookconfiguration.admissionregistration.k8s.io/kubebuilder-cronjob-mutating-webhook-configuration`
    1. `validatingwebhookconfiguration.admissionregistration.k8s.io/kubebuilder-cronjob-validating-webhook-configuration`


1. Create sample resource.

    ```
    kubectl create -f config/samples/batch_v1_cronjob.yaml
    ```

    Check:

    ```
    kubectl get job
    NAME                        COMPLETIONS   DURATION   AGE
    cronjob-sample-1646657880   1/1           5s         19s
    ```

    ```
    kubectl get pod
    NAME                              READY   STATUS      RESTARTS   AGE
    cronjob-sample-1646657880-5vhgr   0/1     Completed   0          26s
    ```

1. Check invalid sample.

    ```
    kubectl create -f config/samples/batch_v1_cronjob_invalid.yaml
    The CronJob "cronjob-sample" is invalid: spec.schedule: Invalid value: "*/1 * * * * *": Expected exactly 5 fields, found 6: */1 * * * * *
    ```

    ```diff
    diff config/samples/batch_v1_cronjob.yaml config/samples/batch_v1_cronjob_invalid.yaml
    6c6
    <   schedule: "*/1 * * * *"
    ---
    >   schedule: "*/1 * * * * *"
    ```
1. Clean up.
    ```
    kubectl delete -f config/samples/batch_v1_cronjob.yaml
    make undeploy IMG=$IMG
    kubectl delete -f https://github.com/cert-manager/cert-manager/releases/download/v1.7.1/cert-manager.yaml
    ```

### 10. [Writing controller tests](https://book.kubebuilder.io/cronjob-tutorial/writing-tests.html)

Overview:
1. `suite_test.go`: autogenerated file
    1. Use `envtest` for Kubernetes API server.
    1. Instantiate your controller.
1. `*_test.go`: Create your file for testing.
    1. Write your test using `Ginkgo`.

Steps:
1. `suite_test.go`:
    1. The marker for new API
        This is what allows new schemas to be added here automatically when a new API is added to the project.
        ```go
        //+kubebuilder:scaffold:scheme
        ```
    1. Client
        ```go
        k8sClient, err = client.New(cfg, client.Options{Scheme: scheme.Scheme})
        ```
    1. Run controller
        ```go
        k8sManager, err := ctrl.NewManager(cfg, ctrl.Options{
            Scheme: scheme.Scheme,
        })
        err = (&CronJobReconciler{
            Client: k8sManager.GetClient(),
            Scheme: k8sManager.GetScheme(),
        }).SetupWithManager(k8sManager)
        Expect(err).ToNot(HaveOccurred())

        go func() {
            defer GinkgoRecover()
            err = k8sManager.Start(ctx)
            Expect(err).ToNot(HaveOccurred(), "failed to run manager")
        }()
        ```
    1. Cleanup in `AfterSuite`
    1. Final codes.

        <details>

        ```go
        /*
        Copyright 2022.

        Licensed under the Apache License, Version 2.0 (the "License");
        you may not use this file except in compliance with the License.
        You may obtain a copy of the License at

            http://www.apache.org/licenses/LICENSE-2.0

        Unless required by applicable law or agreed to in writing, software
        distributed under the License is distributed on an "AS IS" BASIS,
        WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
        See the License for the specific language governing permissions and
        limitations under the License.
        */

        package controllers

        import (
            "context"
            "path/filepath"
            "testing"

            ctrl "sigs.k8s.io/controller-runtime"

            . "github.com/onsi/ginkgo"
            . "github.com/onsi/gomega"
            "k8s.io/client-go/kubernetes/scheme"
            "k8s.io/client-go/rest"
            "sigs.k8s.io/controller-runtime/pkg/client"
            "sigs.k8s.io/controller-runtime/pkg/envtest"
            "sigs.k8s.io/controller-runtime/pkg/envtest/printer"
            logf "sigs.k8s.io/controller-runtime/pkg/log"
            "sigs.k8s.io/controller-runtime/pkg/log/zap"

            batchv1 "tutorial.kubebuilder.io/api/v1"
            //+kubebuilder:scaffold:imports
        )

        // These tests use Ginkgo (BDD-style Go testing framework). Refer to
        // http://onsi.github.io/ginkgo/ to learn more about Ginkgo.

        var (
            cfg       *rest.Config
            k8sClient client.Client // You'll be using this client in your tests.
            testEnv   *envtest.Environment
            ctx       context.Context
            cancel    context.CancelFunc
        )

        func TestAPIs(t *testing.T) {
            RegisterFailHandler(Fail)

            RunSpecsWithDefaultAndCustomReporters(t,
                "Controller Suite",
                []Reporter{printer.NewlineReporter{}})
        }

        var _ = BeforeSuite(func() {
            logf.SetLogger(zap.New(zap.WriteTo(GinkgoWriter), zap.UseDevMode(true)))

            ctx, cancel = context.WithCancel(context.TODO())

            By("bootstrapping test environment")
            testEnv = &envtest.Environment{
                CRDDirectoryPaths:     []string{filepath.Join("..", "config", "crd", "bases")},
                ErrorIfCRDPathMissing: true,
            }

            cfg, err := testEnv.Start()
            Expect(err).NotTo(HaveOccurred())
            Expect(cfg).NotTo(BeNil())

            err = batchv1.AddToScheme(scheme.Scheme)
            Expect(err).NotTo(HaveOccurred())

            //+kubebuilder:scaffold:scheme

            k8sClient, err = client.New(cfg, client.Options{Scheme: scheme.Scheme})
            Expect(err).NotTo(HaveOccurred())
            Expect(k8sClient).NotTo(BeNil())

            k8sManager, err := ctrl.NewManager(cfg, ctrl.Options{
                Scheme: scheme.Scheme,
            })
            Expect(err).ToNot(HaveOccurred())

            err = (&CronJobReconciler{
                Client: k8sManager.GetClient(),
                Scheme: k8sManager.GetScheme(),
            }).SetupWithManager(k8sManager)
            Expect(err).ToNot(HaveOccurred())

            go func() {
                defer GinkgoRecover()
                err = k8sManager.Start(ctx)
                Expect(err).ToNot(HaveOccurred(), "failed to run manager")
            }()

        }, 60)

        var _ = AfterSuite(func() {
            cancel()
            By("tearing down the test environment")
            err := testEnv.Stop()
            Expect(err).NotTo(HaveOccurred())
        })
        ```

        </details>
1. Write controller tests in [controllers/crontab_controller_test.go](controllers/crontab_controller_test.go)
    1. Use `package controllers`
    1. Add `import`
        <details>

        ```go
        import (
            "context"
            "reflect"
            "time"

            . "github.com/onsi/ginkgo"
            . "github.com/onsi/gomega"
            batchv1 "k8s.io/api/batch/v1"
            batchv1beta1 "k8s.io/api/batch/v1beta1"
            v1 "k8s.io/api/core/v1"
            metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
            "k8s.io/apimachinery/pkg/types"

            cronjobv1 "tutorial.kubebuilder.io/project/api/v1"
        )
        ```

        </details>

    1. Create test case.
        1. Initialize and create a `CronJob` object.
        1. Expect to successfully get the created `CronJob` with `k8sclient`.
        1. No active Jobs are found consistently.
        1. Create a new Job with ownerReference. -> Trigger reconciler logic.
        1. Expect to update the `CronJob`'s Status field.
    1. Final code:

        <details>

        ```go
        package controllers

        import (
            "context"
            "fmt"
            "reflect"
            "time"

            . "github.com/onsi/ginkgo"
            . "github.com/onsi/gomega"
            batchv1 "k8s.io/api/batch/v1"
            batchv1beta1 "k8s.io/api/batch/v1beta1"
            v1 "k8s.io/api/core/v1"
            metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
            "k8s.io/apimachinery/pkg/types"

            cronjobv1 "tutorial.kubebuilder.io/api/v1"
        )

        var _ = Describe("CronJob controller", func() {

            // Define utility constants for object names and testing timeouts/durations and intervals.
            const (
                CronjobName      = "test-cronjob"
                CronjobNamespace = "default"
                JobName          = "test-job"

                timeout  = time.Second * 10
                duration = time.Second * 10
                interval = time.Millisecond * 250
            )

            Context("When updating CronJob Status", func() {
                It("Should increase CronJob Status.Active count when new Jobs are created", func() {
                    By("By creating a new CronJob")
                    ctx := context.Background()
                    cronJob := &cronjobv1.CronJob{
                        TypeMeta: metav1.TypeMeta{
                            APIVersion: "batch.tutorial.kubebuilder.io/v1",
                            Kind:       "CronJob",
                        },
                        ObjectMeta: metav1.ObjectMeta{
                            Name:      CronjobName,
                            Namespace: CronjobNamespace,
                        },
                        Spec: cronjobv1.CronJobSpec{
                            Schedule: "1 * * * *",
                            JobTemplate: batchv1beta1.JobTemplateSpec{
                                Spec: batchv1.JobSpec{
                                    // For simplicity, we only fill out the required fields.
                                    Template: v1.PodTemplateSpec{
                                        Spec: v1.PodSpec{
                                            // For simplicity, we only fill out the required fields.
                                            Containers: []v1.Container{
                                                {
                                                    Name:  "test-container",
                                                    Image: "test-image",
                                                },
                                            },
                                            RestartPolicy: v1.RestartPolicyOnFailure,
                                        },
                                    },
                                },
                            },
                        },
                    }
                    Expect(k8sClient.Create(ctx, cronJob)).Should(Succeed())

                    cronjobLookupKey := types.NamespacedName{Name: CronjobName, Namespace: CronjobNamespace}
                    createdCronjob := &cronjobv1.CronJob{}

                    // We'll need to retry getting this newly created CronJob, given that creation may not immediately happen.
                    Eventually(func() bool {
                        err := k8sClient.Get(ctx, cronjobLookupKey, createdCronjob)
                        return err == nil
                    }, timeout, interval).Should(BeTrue())
                    // Let's make sure our Schedule string value was properly converted/handled.
                    Expect(createdCronjob.Spec.Schedule).Should(Equal("1 * * * *"))

                    By("By checking the CronJob has zero active Jobs")
                    Consistently(func() (int, error) {
                        err := k8sClient.Get(ctx, cronjobLookupKey, createdCronjob)
                        if err != nil {
                            return -1, err
                        }
                        return len(createdCronjob.Status.Active), nil
                    }, duration, interval).Should(Equal(0))

                    By("By creating a new Job")
                    testJob := &batchv1.Job{
                        ObjectMeta: metav1.ObjectMeta{
                            Name:      JobName,
                            Namespace: CronjobNamespace,
                        },
                        Spec: batchv1.JobSpec{
                            Template: v1.PodTemplateSpec{
                                Spec: v1.PodSpec{
                                    // For simplicity, we only fill out the required fields.
                                    Containers: []v1.Container{
                                        {
                                            Name:  "test-container",
                                            Image: "test-image",
                                        },
                                    },
                                    RestartPolicy: v1.RestartPolicyOnFailure,
                                },
                            },
                        },
                        Status: batchv1.JobStatus{
                            Active: 2,
                        },
                    }

                    // Note that your CronJob’s GroupVersionKind is required to set up this owner reference.
                    kind := reflect.TypeOf(cronjobv1.CronJob{}).Name()
                    gvk := cronjobv1.GroupVersion.WithKind(kind)

                    controllerRef := metav1.NewControllerRef(createdCronjob, gvk)
                    testJob.SetOwnerReferences([]metav1.OwnerReference{*controllerRef})
                    Expect(k8sClient.Create(ctx, testJob)).Should(Succeed())

                    jobLookupKey := types.NamespacedName{Name: JobName, Namespace: CronjobNamespace}
                    job := &batchv1.Job{}
                    Eventually(func() string {
                        err := k8sClient.Get(ctx, jobLookupKey, job)
                        if err != nil {
                            return ""
                        }
                        ownerReferences := job.GetOwnerReferences()
                        if len(ownerReferences) >= 0 {
                            for _, ref := range ownerReferences {
                                return fmt.Sprintf("%s, %s, %s", ref.APIVersion, ref.Kind, ref.Name)
                            }
                        }
                        return ""
                    }, timeout, interval).Should(Equal("batch.tutorial.kubebuilder.io/v1, CronJob, test-cronjob"))

                    By("By checking that the CronJob has one active Job")
                    Eventually(func() ([]string, error) {
                        err := k8sClient.Get(ctx, cronjobLookupKey, createdCronjob)
                        if err != nil {
                            return nil, err
                        }

                        names := []string{}
                        for _, job := range createdCronjob.Status.Active {
                            names = append(names, job.Name)
                        }
                        return names, nil
                    }, timeout, interval).Should(ConsistOf(JobName), "should list our active job %s in the active jobs list in status", JobName)
                })
            })
        })
        ```

        </details>

1. Run tests.
    ```
    make test
    ```
