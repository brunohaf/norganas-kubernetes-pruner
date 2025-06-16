<div align="center" style="max-width: 700px; margin: auto;">
<br>
  <div style="width:80px; height:80px; border-radius:50%; overflow:hidden; border:4px solid #333; margin: 0 auto; display:flex; align-items:center; justify-content:center;">
    <img src="src/resources/norganas-logo.png" alt="Norganas Helm of Oblivion" style="width:30%; height:30%; object-fit:contain;">
  </div>
  <h1 style="margin-bottom: 0.5em; font-weight: 700;">
    norganas-kube-prune
  </h1>
  
  <p style="font-size: 1.1rem; line-height: 1.5; margin-top: 0;">
    A lightweight, battle-tested tool that minimizes resource waste in Kubernetes clusters with minimal setup.  
    Leveraging Prometheus metrics, it identifies idle APIs and workloads, enabling one-time deployment, easy customization to your environment, and silent reclamation of unused resources — efficiently reducing cloud costs.
  </p>

  <p style="font-size: 0.9rem; font-style: italic; color: #555; margin-top: 1.5em; line-height: 1.4;">
    Named after <strong>Norganas, the Finger of Oblivion</strong>, a mysterious entity imprisoned within the Amber Temple in the <em>Curse of Strahd</em> D&D adventure.  
    Though open to interpretation, Norganas represents a neutral force trading forgotten memories for new skills. Similarly, this tool silently removes stale Kubernetes workloads, freeing resources for fresh deployments.
  </p>

  <blockquote style="border-left: 4px solid #d9534f; padding-left: 1em; color: #d9534f; margin-top: 2em; max-width: 650px; margin-left: auto; margin-right: auto;">
    Despite its proven effectiveness, norganas-kube-prune is a bare-bones utility that requires careful customization to fit your specific cluster setup and operational needs.  
   Use at your own discretion.
  </blockquote>

  <p align="center" style="margin-top: 2em;">
    <a href="#" style="margin: 0 0.5em;"><img src="https://img.shields.io/badge/version-1.0.0-blue.svg" alt="Version"></a>
    <a href="#" style="margin: 0 0.5em;"><img src="https://img.shields.io/github/license/brunohaf/norganas-kube-prune" alt="License"></a>
    <a href="#" style="margin: 0 0.5em;"><img src="https://img.shields.io/badge/build-passing-brightgreen.svg" alt="Build"></a>
  </p>

</div>


<p align="center">
  <a href="#key-features">Key Features</a> •
  <a href="#requirements">Requirements</a> •
  <a href="#how-to-use">How To Use</a> •
  <a href="#know-limitations">Know Limitations</a> •
  <a href="#related-projects">Related Projects</a> •
  <a href="#license">License</a>
</p>

## Key Features

> For safety, norganas’ automatic resource purging was replaced with a reporting mechanism that generates a “to-purge” list. This list can be sent to external repositories—such as an Azure file share in the current implementation—enabling easy integration with notification systems for auditing or contesting, as well as automation pipelines for orchestrating resource deletion.

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

1. Setup the specifications at [config.json](src/configs/configs.json) and [values.yaml](charts/norganas-kube-prune/values.yaml).
2. Deploy the **norganas-kube-prune** cronjob in your Kubernetes cluster.  
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
