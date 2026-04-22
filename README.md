# [Application Name]

## Overview

<!-- 2-3 sentences: What does this app launch? Who is it for? What problem does it solve? -->
<!-- Pick the app type that matches yours: -->

<!-- Batch Connect: -->
[Application Name] is an Open OnDemand Batch Connect app that launches [software name and version] as an interactive [desktop / web server / notebook] session on HPC clusters. It is designed for [researchers / students / etc.] who need [brief use case].

<!-- Passenger: -->
<!-- [Application Name] is an Open OnDemand Passenger app that provides [web-based tool / service] for [audience]. -->

<!-- Widget / Dashboard: -->
<!-- [Application Name] is an Open OnDemand [widget / dashboard] that [what it does] within the OOD dashboard. -->

<!-- If this app is for sysadmins rather than end users, say so here. -->
<!-- Note whether installation requires root permissions. -->

- Upstream project: [software homepage](https://example.com)

## Screenshots

<!-- A screenshot helps deployers verify their installation and helps users understand what they'll get. -->
<!-- Place images in a screenshots/ or docs/ directory. -->

![Application running in browser](docs/screenshot.png)

## Features

<!-- List the key capabilities specific to THIS OOD app (not the upstream software). -->

- Launches [software] via [VNC desktop / web server / Jupyter] on compute nodes
- Supports [GPU / CPU / both] execution
- Configurable [memory / cores / wall time] via the launch form
- [Containerized via Singularity/Apptainer | Module-based]
- [Any other notable features: multi-version support, custom dashboards, etc.]

## Requirements

### Compute Node Software

<!-- Batch Connect: What must be installed on the compute nodes where jobs will run? -->
<!-- Passenger: What must be installed on the OOD host? -->

- [Software name] [version constraints, e.g., 4.2+]
- [Runtime dependencies, e.g., Python 3.9+, CUDA 12.0+, Java 11+]
- [Container runtime, e.g., Singularity 3.8+ / Apptainer 1.0+] (if containerized)
- [Window manager, e.g., XFCE, Fluxbox] (if VNC-based Batch Connect app)
- [Operating System, e.g., Rocky9/RHEL9, Ubuntu]

### Open OnDemand

- Open OnDemand [minimum version, e.g., 3.0+]
- [Scheduler: Slurm / PBS / LSF] (Batch Connect apps)
- [Lmod or Environment Modules] (if using `module load`)

### Optional

- [TurboVNC, VirtualGL] (for GPU-accelerated rendering)
- [Database or dataset paths] (if the app needs reference data)

## App Installation

Please see the [References section](#software-installation) below for instructions on how to install the software that is launched by this App.

### 1. Clone the repository

```bash
# Batch Connect / Passenger apps:
cd /var/www/ood/apps/sys

# Widgets / Dashboards — check OOD docs for the correct path

git clone https://github.com/YOUR-ORG/YOUR-APP.git
cd YOUR-APP

# Pin to a release (recommended)
git checkout v1.0.0
```

### 2. Configure for your site

<!-- Point deployers to the ONE place they need to edit. -->
#### form.yml Attributes

Edit `form.yml` and update these values for your cluster:

| Attribute | Description | Default |
|-----------|-------------|---------|
| `cluster` | Target cluster ID | `"my_cluster"` |
| `modules` | Module to load on compute node | `"software/1.0"` |
| `bc_num_hours` | Maximum wall time (hours) | `4` |
| `bc_num_slots` | Number of cores | `1` |
| `partition` | Default scheduler partition | `"batch"` |
| `memory` | Memory per job (GB) | `8` |

#### manifest.yml Attributes

Edit `manifest.yml` and update these values for your organization:

| Attribute | Change to |
|-----------|-----------|
| `description` | Your cluster and your documentation |


#### Environment variables

<!-- Only include this section if your scripts expect variables beyond what OOD provides. -->

| Variable | Required | Description |
|----------|----------|-------------|
| `SOFTWARE_DB_PATH` | Yes | Path to reference database directory |
| `SINGULARITY_IMAGE` | No | Override default container image path |

<!-- Passenger apps: describe any config files, environment setup, or bundle install steps. -->
<!-- If there are additional config files, list them too. -->

<!-- Passenger: -->
<!-- Restart the app from the OOD developer dashboard, or restart the PUN. Visit your OOD dashboard and navigate to [App URL]. -->


<!-- Document ALL site-specific values and where they live. -->
<!-- This is the most important section for deployers at other sites. -->

<!-- Batch Connect apps: document form.yml attributes -->
<!-- Passenger apps: document config files, environment variables, or database setup -->






### 3. Verify

<!-- Batch Connect: -->
No OOD restart is needed (Batch Connect apps are detected automatically). Visit your OOD dashboard and look for **[App Name]** under **Interactive Apps > [Category]**.


## Troubleshooting

### Job starts but app doesn't appear (Batch Connect)

1. Check the job's `output.log` in `~/ondemand/data/sys/YOUR-APP/`
2. Verify the module loads correctly: `module load software/1.0`
3. For VNC apps, verify the window manager is installed: `which xfwm4`

### "Module not found" error

The module name in `form.yml` doesn't match your system. Run `module spider software` to find the correct name and update the `modules` attribute.

### Connection timeout

The app may need more time to start. Increase the connection timeout or check that the compute node can open the required port.

<!-- Add real issues you've encountered during testing. -->

## Testing

<!-- Where has this app been deployed and verified? -->

| Site | OOD Version | Scheduler | Status |
|------|-------------|-----------|--------|
| [Your Institution] | 3.1 | Slurm 23.02 | Tested |

<!-- How can a deployer verify it works? -->

To verify your installation:

1. Launch the app from the OOD dashboard with default settings
2. Confirm the application loads in the browser
3. [Any app-specific verification, e.g., "run a small test job"]

## Known Limitations

<!-- Be honest about what doesn't work or hasn't been tested. -->

- [e.g., Multi-node jobs are not supported]
- [e.g., GPU rendering requires VirtualGL, which is not configured by default]
- [e.g., Only tested on RHEL 8; may not work on Ubuntu]

## Contributing

Contributions are welcome. To contribute:

1. Fork this repository
2. Create a feature branch (`git checkout -b feature/my-improvement`)
3. Submit a pull request with a description of your changes

For bugs or feature requests, [open an issue](https://github.com/YOUR-ORG/YOUR-APP/issues).

This app is part of the [OOD Appverse](https://ondemand.connectci.org/affinity-groups/ood-appverse). Join the [Appverse Affinity Group](https://ondemand.connectci.org/affinity-groups/ood-appverse) to connect with other contributors.

## References

<!-- Credit upstream projects and any code you borrowed. -->

- [Software Name](https://example.com) — the application launched by this OOD app
- [Open OnDemand](https://openondemand.org/) — the HPC portal framework

### Software Installation

In this optional section you can include details of how to install the underlying software that is launched by this app. For example, include information about Blender if you are publishing a Blender Batch Connect App. You could potentially include the following:

* Steps on how to install the software from source, including critical dependencies
* Steps on how to install and enable a Python environment with the software
* A link to container orchestration files that can reproduce a container from scratch
* A link to a pre-existing container that can be downloaded
* Some general info on how to obtain and configure the software, especially if it is distributed as binaries and/or is commercial or proprietary software

If this documentation is too large or unwieldy, consider adding it to a separate markdown file and linking it here.

## License

[MIT License](LICENSE)

<!-- If dual-licensing, specify: -->
<!-- Code: [MIT License](LICENSE) -->
<!-- Documentation: [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/) -->

## Acknowledgments

<!-- Funding, institutional support, collaborators. -->
<!-- Replace with your own funding information. -->

This work is supported by [funding agency] award number [award number].
