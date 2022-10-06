# Env Caching

I have a few envs that I use for general analysis and don't always add an env-specification file (e.g. `environment.yml` or `requirements.txt`) to my analysis repos, but for the sake of reproducibility and convenience (and so that I can delete these envs and reuse the env_names without having to worry about losing the prior envs).

## General `conda` advice

You can avoid a lot of issues and cut down time spend resolving dependencies by strictly pulling `conda` packages from a single channel (I recommend and prefer conda-forge as it is the most active and complete `conda` channel)

```bash
conda config --add channels conda-forge
conda config --set channel_priority strict
```

## `conda` env Export Commands

* `conda list --explicit`
    * Does: Exports download urls of the exact conda package version builds installed in the active env.
        * Pros:
            * Extremely fast to recreate the env (doesn't do any dependency solving; just downloads and installs the listed package builds).
            * Can reliably reproduce envs that have packages from multiple channels (given the caveat in "Cons").
        * Cons: Only reliable when recreating the env a system that matches the exporting system.
    * Export command:

        ```bash
        (geo_env) user@host:~/...$ conda list --explicit > geo_env_2022_10_05_ubuntu_LTS_20_04.txt
        ```

    * Recreation command:

        ```bash
        (base) user@host:~/...$ conda create --name geo_env --file geo_env_2022_10_05_ubuntu_LTS_20_04.txt
        ```

* `conda conda env export --from-history`
    * Does: Exports just the names of the `conda` and `pip` packages the user explicitly installed in the active `conda` env (i.e. dependencies aren't included in this export).
        * Pros: Most reliable method for creating a coherent env (with the explicitly installed packages) on a different platform (i.e. different OS and/or chip architecture).
        * Cons: Doesn't necessarily perfectly recreate an env (as a package may only have a build for the exporting platform).
    * Export command:

        ```bash
        (geo_env) user@host:~/...$ conda env export --from-history > geo_env_2022_10_05_from_history.txt
        ```

    * Recreation command:

        ```bash
        (base) user@host:~/...$ conda env create --file geo_env_2022_10_05_from_history.txt
        ```

* `conda conda env export`
    * Does: Exports the name, version number, and build number of each of the `conda` and `pip` packages installed in the active env.
        * Pros: Most resiliant method for recreating an env in platform matching the exporting platform.
        * Cons: Slowest to resolve; can fail to reproduce an env where packages were installed from channels with inconsistent package dependencies.
    * Export command:

        ```bash
        (geo_env) user@host:~/...$ conda env export > geo_env_2022_10_05.txt
        ```

    * Recreation command:

        ```bash
        (base) user@host:~/...$ conda env create --file geo_env_2022_10_05.txt
        ```