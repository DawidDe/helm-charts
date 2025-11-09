# Pelican Helm Charts Repository

This repository contains Helm charts for the Pelican project.

## Available Helm Charts

- [Pelican Panel Helm Chart](https://github.com/DawidDe/helm-charts/tree/main/charts/pelican-panel)

## How to Use the Charts

### 1. Add the Repository

To use the charts in this repository, first add it to Helm:

```bash
helm repo add pelican https://dawidde.github.io/helm-charts
helm repo update
```

### 2. Install a Chart

To install a chart from this repository, use:

```bash
helm install <release-name> pelican/<chart-name>
```

### 3. Customize the Installation

You can customize the installation by setting values with the `--set`flag or by using a custom `values.yaml` file.

#### Option 1: Using `--set`

```bash
helm install <release-name> pelican/<chart-name> --set key=value
```

#### Option 2: Using a `values.yaml` file

```bash
helm install <release-name> pelican/<chart-name> -f values.yaml
```