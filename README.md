# future-helm-chart

A Helm chart for using R's future package on a Kubernetes cluster. One can think of a Helm chart as a package for a Kubernetes cluster.

The future package provides for parallel computation in R on one or more machines.

- <https://cran.r-project.org/package=future>
- <https://github.com/HenrikBengtsson/future>

This Helm chart will deploy:

 - one RStudio server instance with port 8787 exposed running in a 'scheduler' pod
 - four R workers that you can connect to the scheduler by invoking `future::plan()` in R or RStudio, running on the 'scheduler'.

The following R packages are pre-installed on the cluster (because the Helm chart uses the [paciorek/future-kubernetes Docker container](https://github.com/paciorek/future-kubernetes-docker):

 - future
 - future.apply
 - doFuture

See below for how to modify this chart to change the number of workers and install additional R packages in the cluster.

See [this repository](https://github.com/paciorek/future-kubernetes) for how to use this chart, but if you are familiar with Kubernetes, you can just follow the instructions below to install the Helm chart.

## Installing the Chart

For the moment the chart is only available from Github and not from a Helm repo.


Clone this repository and create a tarball of the chart.

```bash
VERSION=0.1
helm install --wait my-release https://github.com/paciorek/future-helm-chart/archive/${VERSION}.tar.gz 
```

Alternatively, you can clone the repository containing the chart and then
install the chart. This will allow you to modify the chart.

```bash
git clone https://github.com/paciorek/future-helm-chart
tar czf future-helm.tgz -C future-helm-chart .
helm install --wait my-release ./future-helm.tgz
```

## Modifying the chart

To increase the number of R workers (before running `helm install` above), edit `values.yaml`. In particular, you'll need to modify the `replicas` line that is in the 'worker' stanza (the block of code under `worker:`. Don't modify the 'replicas' line in the 'scheduler' stanza.

To add additional R packages, edit `values.yaml`. Simply modify the lines that look like this:

```
  env:
  #  - name: EXTRA_R_PACKAGES
  #    value: data.table
```

by removing the "#" comment character and putting the R packages you want installed in place of 'data.table', with the names of the packages separated by spaces.

In many cases you may want these packages installed on both the scheduler (where RStudio runs) and on the workers. If so, make sure to modify the lines above in both the `scheduler` and `worker` stanzas.

WARNING: installing R packages can take substantial time. This is done when the pods are started, so the RStudio interface will not be available until all packages are installed in the scheduler pod.

## Acknowledgments

This Helm chart is based heavily on the [Dask helm chart](https://github.com/dask/helm-chart).
