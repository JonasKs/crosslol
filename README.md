# provider-crosslol

`provider-crosslol` is a minimal [Crossplane](https://crossplane.io/) Provider
that is meant to be used as a crosslol for implementing new Providers. It comes
with the following features that are meant to be refactored:

- A `ProviderConfig` type that only points to a credentials `Secret`.
- A `MyType` resource type that serves as an example managed resource.
- A managed resource controller that reconciles `MyType` objects and simply
  prints their configuration in its `Observe` method.

## Developing

1. Use this repository as a crosslol to create a new one.
1. Run `make submodules` to initialize the "build" Make submodule we use for CI/CD.
1. Rename the provider by running the following command:
```shell
  export provider_name=MyProvider # Camel case, e.g. GitHub
  make provider.prepare provider=${provider_name}
```
4. Add your new type by running the following command:
```shell
  export group=sample # lower case e.g. core, cache, database, storage, etc.
  export type=MyType # Camel casee.g. Bucket, Database, CacheCluster, etc.
  make provider.addtype provider=${provider_name} group=${group} kind=${type}
```
5. Replace the *sample* group with your new group in apis/{provider}.go
5. Replace the *mytype* type with your new type in internal/controller/{provider}.go
5. Replace the default controller and ProviderConfig implementations with your own
5. Run `make reviewable` to run code generation, linters, and tests.
5. Run `make build` to build the provider.

Refer to Crossplane's [CONTRIBUTING.md] file for more information on how the
Crossplane community prefers to work. The [Provider Development][provider-dev]
guide may also be of use.

[CONTRIBUTING.md]: https://github.com/crossplane/crossplane/blob/master/CONTRIBUTING.md
[provider-dev]: https://github.com/crossplane/crossplane/blob/master/contributing/guide-provider-development.md



# Crosslol

### Installation 
1. Clone this repository. 
2. `git submodule init`
3. `git submodule update -- ./yang`
4. Install [kind](https://kind.sigs.k8s.io/docs/user/quick-start/#installation) for a local k8s cluster
5. Install [`kubectl`](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/) as the k8s CLI tool
6. Install [`k9s`](https://k9scli.io/topics/install/) to view the k8s cluster
7. Create a cluster `kind create cluster --name crossnet`
7.1. `kubectl cluster-info --context kind-crossnet` to see that the cluster is running
8. Browse cluster by typing `k9s` 
9. Install [`helm`](https://helm.sh/docs/intro/install/)
10. Install [`crossplane`](https://docs.crossplane.io/latest/software/install/)
10.1. `helm repo add crossplane-stable https://charts.crossplane.io/stable`
10.2. `helm install crossplane --kube-context kind-crossnet --namespace crossplane-system --create-namespace crossplane-stable/crossplane --` (ensure your context is the crossnet cluster, see `kubectl config get-contexts`)
