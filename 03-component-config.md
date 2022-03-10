## 3. [Component Config](https://book.kubebuilder.io/component-config-tutorial/tutorial.html)

### 3.1 [Changing things up](https://book.kubebuilder.io/component-config-tutorial/api-changes.html)

For new project

```
kubebuilder init --domain tutorial.kubebuilder.io --component-config
```

For existing project:

```go
func main() {
    // ...
    // config file
    var configFile string
    flag.StringVar(&configFile, "config", "",
        "The controller will load its initial configuration from this file. "+
            "Omit this flag to use the default configuration values. "+
                "Command-line flags override configuration from this file.")
    var err error
    options := ctrl.Options{
        Scheme:                 scheme,
        MetricsBindAddress:     metricsAddr,
        Port:                   9443,
        HealthProbeBindAddress: probeAddr,
        LeaderElection:         enableLeaderElection,
        LeaderElectionID:       "fed99f69.tutorial.kubebuilder.io",
    }
    if configFile != "" {
        options, err = options.AndFrom(ctrl.ConfigFile().AtPath(configFile))
        if err != nil {
            setupLog.Error(err, "unable to load the config file")
            os.Exit(1)
        }
    }
    // ...
    mgr, err := ctrl.NewManager(ctrl.GetConfigOrDie(), options)
    // ...
```

### 3.2 [Defining Config](https://book.kubebuilder.io/component-config-tutorial/define-config.html)

[config/manager/controller_manager_config.yaml](config/manager/controller_manager_config.yaml)

For more details: https://pkg.go.dev/sigs.k8s.io/controller-runtime/pkg/config/v1alpha1#ControllerManagerConfigurationSpec

### 3.3. [Using a Custom Type](https://book.kubebuilder.io/component-config-tutorial/custom-type.html)

TODO:
- [3.3.1 Adding a new Config Type](https://book.kubebuilder.io/component-config-tutorial/config-type.html)
- [3.3.2 Updating main](https://book.kubebuilder.io/component-config-tutorial/updating-main.html)
- [3.3.3 Defining your Custom Config](https://book.kubebuilder.io/component-config-tutorial/define-custom-config.html)
