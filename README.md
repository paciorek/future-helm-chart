# future-helm-chart

A helm chart for using R's future package on a kubernetes cluster.

The future package provides for parallel computation in R on one or more machines.

- <https://cran.r-project.org/package=future>
- <https://github.com/HenrikBengtsson/future>

This helm chart will deploy:

 - one RStudio server instance with port 8787 exposed running in a 'scheduler' pod
 - four R workers that you can connect to the scheduler by invoking `future::plan()` in R or RStudio, running on the 'scheduler'.

See [this repository](https://github.com/paciorek/future-kubernetes) for how to use this chart, but if you are familiar with Kubernetes, you can just follow the instructions below to install the helm chart.

## Installing the Chart

For the moment the chart is only available from Github and not from a helm repo.


Clone this repository and create a tarball of the chart.

```bash
git clone https://github.com/paciorek/future-helm-chart
cd future-helm-chart
tar -cvzf future-helm.tgz
helm install ./future-helm.tgz 
```

Alternatively, to install the chart with a user specified release name, here `my-release`:

```bash
helm install --name my-release ./future-helm.tgz 
```

## Acknowledgments

This helm chart is based heavily on the [Dask helm chart](https://github.com/dask/helm-chart).
