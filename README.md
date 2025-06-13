<h1 align="center">
  <br>
  <div style="width:80px; height:80px; border-radius:50%; overflow:hidden; border:4px solid #333; margin: 0 auto; display:flex; align-items:center; justify-content:center;">
    <img src="src/resources/norganas.png" alt="Norganas Helm of Oblivion" style="width:30%; height:30%; object-fit:contain;">
  </div>
  Norganas Helm of Oblivion
  <br>
</h1>

<h4 align="center">
  A Kubernetes CronJob implemented to identify and purge stale deployments or workloads by analyzing ingress metrics from Prometheus over a specified interval to determine if the workload was idle or unused. The application compiled a list of idle workloads and forwarded it to a database or an object storage service for reporting or automated deletion.
</h4>

<div align="center">
<em>
The <strong>Norganas' Helm of Oblivion</strong> channels the cryptic power of Norganas — known as the Finger of Oblivion, one of the dark vestiges imprisoned within a sarcophagus in the Amber Temple of D&D's Curse of Strahd adventure. This tool brings that same silent, inexorable force to Kubernetes: cleansing clusters of forgotten, idle workloads to reclaim resources and reduce waste.
</em>
</div>

<br>

<p align="center">
  <a href="#"><img src="https://img.shields.io/badge/version-1.0.0-blue.svg" alt="Version"></a>
    <a href="#"><img src="https://img.shields.io/github/license/brunohaf/norganas-helm-oblivion" alt="License"></a>
  <a href="#"><img src="https://img.shields.io/badge/build-passing-brightgreen.svg" alt="Build"></a>
</p>

<p align="center">
  <a href="#key-features">Key Features</a> •
  <a href="#requirements">Requirements</a> •
  <a href="#how-to-use">How To Use</a> •
  <a href="#know-limitations">Know Limitations</a> •
  <a href="#related-projects">Related Projects</a> •
  <a href="#license">License</a>
</p>

## Key Features

- Fetches ingress volumetry through Prometheus monitoring (`http_requests_total` metric).  
- Filters purging scope to releases tagged with labels.  
- Logs purging events for observability and auditing.  
- Uses Azure File Share as persistent file server and datalake for logs and release lists.  
- Supports gathering cluster node capacity and allocation info alongside workload metrics.  
- Automates idle Helm release cleanup to enhance Kubernetes cluster cost-effectiveness.  

## Requirements

- Kubernetes cluster with Helm installed  
- Prometheus monitoring configured, scraping ingress controller metrics  
- A proper ServiceAccount with the required permissions  
- *Access to Azure File Share for list and log storage  
- *Azure DevOps Pipelines configured for the Purger process
  
> <em>*See [Know Limitations](#know-limitations).</em>

## How To Use

1. Setup the specifications at [config.json](src/configs/configs.json) and [values.yaml](charts/norganas-helm-oblivion/values.yaml).
2. Deploy the **norganas-helm-of-oblivion** cronjob in your Kubernetes cluster.  
3. Ensure Prometheus is scraping the ingress controller metrics correctly.  
4. Configure the Azure File Share secrets and access for uploading release lists and logs.  
5. Setup the Purger Azure Pipeline to consume the file share lists and delete the releases.  
6. Monitor logs and metrics for purging activity in Seq and Prometheus dashboards.  

## Know Limitations

This solution was originally tailored to address a specific need: purging stale, idle workloads in non-production Kubernetes clusters for cost optimization. While effective for this scenario, its current implementation has limitations that should be considered when applying it more broadly:

- Hardcoded configurations — Destinations for release lists (e.g., database or object storage) and log storage are fixed; dynamic runtime flags or environment-based selection are not yet implemented.
- Basic observability hooks — While purging events are logged, integration with external log sinks (e.g., ELK, Loki) or forwarding to centralized auditing systems is not available out of the box.
- Limited alerting integrations — No built-in support for connecting to external alerting systems (e.g., PagerDuty, OpsGenie, Slack webhooks).
- No bundled dashboards — There are no pre-built Grafana dashboards or Prometheus recording rules provided for easy monitoring of purge activities or workload idleness.
- Manual scaling — The tool does not yet support dynamic scaling or adaptive scheduling based on cluster conditions or workload types.

These limitations can be addressed through future enhancements, such as adding configurable flags, log forwarding capabilities, richer alerting service integrations, and packaged observability assets (Grafana dashboards, Prometheus rules).

## Related Projects

- [KEDA (Kubernetes Event-Driven Autoscaling)](https://keda.sh/) — Inspiration for event-driven, metrics-based automation in Kubernetes.  
- [Karpenter](https://karpenter.sh/) — Kubernetes cluster autoscaler focusing on workload-driven scaling and efficient resource utilization.  
- [Helm](https://helm.sh/) — Kubernetes package manager.  
- [Prometheus.io](https://prometheus.io/docs/prometheus) — Open source metrics and monitoring for systems and services.

## License

[Apache 2.0](LICENSE)

---

> GitHub [@brunohaf](https://github.com/brunohaf)
