Turbine infra CLI
============================================

### Turbine Framework

Turbine is a set of tools and conventions that establish best practice for
managing “infrastructure as code” in a Cloud Computing context. A set of
Turbine conventions help you to _describe_ the Cloud infrastructure resources
you need. Then a CLI helps you to _converge_ these resources to their expected
state. “_Converging to an expected state_” means both creating your
infrastructure from scratch, apply any evolution over time, like configuration
changes or scale in/out, and decommission anything that needs to be destroyed.


Concepts
--------

### Cloud infrastructure as code (IaC)

In the context of Cloud Computing, IT infrastructure is a set of resources
created in the Cloud. Current best practice leads you towards those requirements:

1. Describe those infrastructure resources in their desired state (e.g. in
   some YAML files, or through some DSL syntax).

2. Track the history of this description (e.g. in a Git repository).

3. Persist the known state of the managed Cloud resources, inlcuding handles
   like unique identifiers in the Cloud.

4. Persist secrets like passwords and private keys in some secure encrypted
   storage.


### Converging Cloud infrastructure

Converging Cloud infrastructure resources either embraces prototyping (day 0),
creating an initial deployment (day 1), deploying any further updates or
upgrades (day 2), or decommissioning (day Z). In particular, Turbine brings
solutions for both day 1 and day 2 concerns.


### Distributing standard infrastructure descriptions

Ship standard descriptions and possible variants.


### Follow a distribution

1. Describe customizations that apply to some description of a standard
   infrastructure, comming from an upstream distribution.

2. Pin a specific version of an upstream distribution.

3. Track history of the customizations

4. Be able to rebase customizations on top of a new version of the upstream
   distribution.



Overall picture
---------------

Turbine brings a solution to several separate concerns:

1. Bootstrap a BOSH environment. This is mainly about creating a BOSH server,
   properly configured to pilot some _automated infrastructure_*. Here we
   basically propose a (big) wrapper around `bosh create-env` for making
   things easy, smooth, and provide a completely automated bootstrap process.

2. Converge [infrastructure modules](./docs/components.md), targeting a given
   BOSH environment. This could be seen as a (big) wrapper around
   `bosh deploy` for making things easy, smooth, and ensure modularity.

3. Converge application deployments on top of the converged infrastructure
   modules. This is not finished yet, but is planned for the near future.

Those are separated. You could create your BOSH environment with any other
solution like [BUCC][bucc], and still be able to deploy Easy Foundry on top of
it.


_* Automated Infrastructure_: BOSH supports various [Iaas][iaas] like public
or private clouds, [Bare-Metal][bare_metal]-as-a-Service solutions like
RackHD, or container orchestration platforms like Kubernetes.

[bucc]: https://github.com/starkandwayne/bucc
[iaas]: https://en.wikipedia.org/wiki/Infrastructure_as_a_service
[bare_metal]: https://en.wikipedia.org/wiki/Bare-metal_server



Topologies
----------

### Deployment modes

We address several deployment modes that are worth being explained.

- **Multi VMs**: This is the classical BOSH mode, as used by many Fortune 500
  companies for their production infrastructures. The BOSH server runs alone
  on its VM, and all managed nodes are running on their own VM too. This mode
  provides proper isolation between workloads and thus higher robustness, for
  higher costs.

- **Single VM**: This is the classical _BOSH-Lite_ mode, where one single VM
  runs both the BOSH server, and managed nodes. The BOSH server is on the VM
  itself, whereas managed nodes are deployed in containers, on the same VM.
  This mode provides poor isolation of workloads, for absolute minimal costs.

- **Hybrid VMs**: One VM, that only runs the BOSH server, and a second one for
  running the managed nodes only. This is a hybrid setup where the BOSH server
  is on its VM alone, whereas managed nodes are deployed in containers, on a
  separate VM. This mode provides improved isolation, for very reduced costs.

### Support status

#### `infra up`-based environments

Infrastructure     | Topology   | System | Status             | Turbine Flavor
-------------------|------------|--------|--------------------|---------------
Bare-Metal server  | Single VM  | Linux  | Supported          | `ddbox`
Bare-Metal server  | Hybrid VMs | Linux  | Actively supported | `ddbox`
Bare-Metal server  | Multi VMs  | Linux  | Not planned        | `ddbox`
Google Cloud (GCP) | Single VM  | Linux  | Supported          | `gcp`
Google Cloud (GCP) | Hybrid VMs | Linux  | Not supported      | `gcp`
Google Cloud (GCP) | Multi VMs  | Linux  | Not supported      | `gcp`
Local Virtualbox   | Single VM  | Linux  | Supported          | `ddbox`
Local Virtualbox   | Hybrid VMs | Linux  | Actively supported | `ddbox`
Local Virtualbox   | Multi VMs  | Linux  | Not planned        | `ddbox`
Local Virtualbox   | Single VM  | macOS  | Supported          | `ddbox`
Local Virtualbox   | Hybrid VMs | macOS  | Supported          | `ddbox`
Local Virtualbox   | Multi VMs  | macOS  | Not planned        | `ddbox`

#### `bucc up`-based environments

Infrastructure     | Topology  | System | Status
-------------------|-----------|--------|--------
AWS                | Multi VM  | Linux  | Supported
Azure              | Multi VM  | Linux  | Supported
Azure Stack        | Multi VM  | Linux  | Supported
Docker             | Single VM | Linux  | Not supported
Google Cloud (GCP) | Multi VM  | Linux  | Supported
OpenStack          | Multi VM  | Linux  | Supported
Virtualbox         | Single VM | Linux  | Supported
vSphere            | Multi VM  | Linux  | Supported



Getting started
---------------

Basically, you need to:

1. Check some prerequisites.

2. `infra up` for converging the base infrastructure environment.

3. `infra converge` for converging [infrastructure modules](./docs/components.md)
   or application deployments.


### Deployment guides

We have several deployment guides for some topologies that we support.

- [Bare-Metal server](./docs/getting-started/bare-metal.md)
- [Google Cloud VM](./docs/getting-started/gcp-vm.md)
- [Local laptop](./docs/getting-started/local-vbox.md)

In case you encounter issues, please refer to the [troubleshooting](./docs/troubleshooting.md)
chapter of the documentation.



Documentation
-------------

The `infra` CLI provides inline help with `infra help`. And generally, help on
a given command can be obtained with `infra <command> -h`.

Other documentations are available in the [docs](./docs/) directory:

- [Turbine Command Line Interface workflow](./docs/cli-workflow.md)
- [Turbine Command Line Interface reference](./docs/cli-reference.md)
- [Turbine file structure reference](./docs/turbine-structure-reference.md)
- [Troubleshooting Guide](./docs/troubleshooting.md)



Contributing
------------

Please feel free to submit issues and pull requests.



Author and License
------------------

Copyright © 2017-present, Benjamin Gandon, Gstack

Like the rest of BOSH, the Turbine framework and Turbine infra CLI are
released under the terms of the
[Apache 2.0 license](http://www.apache.org/licenses/LICENSE-2.0).

<!--
# Local Variables:
# indent-tabs-mode: nil
# End:
-->
