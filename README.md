kubetools
=========

A collection of small tools to speed up deployments and administrative tasks on different Kubernetes clusters

Requirements
------------

Drop each cluster's configuration file under the directory `.kube` in your home directory, and rename it in the format `config.NS_NAME`.

For example, assuming we have 2 clusters: sandbox and dev, then the `~/.kube` directory must have the following files
```bash
user@hostname $ ls ~/.kube
config.dev
config.sandbox
```
Now, lets assume we just deployed a namespace called `qa` in the same infrastructure hosting dev, then all we have to do is just copy the config file with a different `NS_NAME`
```bash
user@hostname $ cp ~/.kube/config.dev ~/config.qa
user@hostname $ ls ~/.kube
config.dev
config.qa
config.sandbox
```
Notice that you must copy the file, not a symlink to `config.NS_NAME` file, as this might create conflict with the way how `kubeconfig` works.

Examples
--------

### kubeconfig

By default `kubeconfig` returns the list of context availables:
```bash
user@hostname $ kubeconfig
The available kubernetes clusters are:
	 * dev [ Currently Selected ]
	 * qa
	 * sandbox

Context: dev
```

If you want to change to sandbox, just append the name to the command
```bash
user@hostname $ kubeconfig sandbox
The available kubernetes clusters are:
         * dev
         * qa
         * sandbox [ Currently Selected ]

Context: sandbox
```

*NOTE:* If there are more than one context in the cluster, then, the first on the list be selected.

### kubedeploy

The `kubedeploy` command, is a tool to assist restarting a deployment and leaving an annotation on the selected deployment.

The tool will attempt to find and list all the deployments available on the selected cluster by the `kubeconfig` tool.

### kubetoken

The `kubetoken` command will lookup for all the tokens on the specified namespace. If no namespace is specified, then I'll try to fetch the token from kube-system.
