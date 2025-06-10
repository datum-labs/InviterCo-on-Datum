# InviterCo-on-Datum
Deploy a Datum Gateway with Kustomize Manifests

[InviterCo](https://inviter.co) provides a seamless Slack integration for managing invitations and access control.

**Q:** Inviter.co is great, but what if you want to brand behind your own domain name?

**A:** Use Datum Gateways! This example deploys a Datum Gateway to act as a transparent reverse proxy for your vanity domain name (in our case `slack.datum.net`) and the inviter.co URL (in our case `inviter.co/datum`). This functionality allows you to bring your own domain name and service requests from our global anycast reverse proxy service.

## Requirements

The following pre-requisites are required to deploy InviterCo-on-Datum:

- Working [Datum CLI tools](https://docs.datum.net/docs/tasks/tools/)
- A functional [Datum Project](https://docs.datum.net/docs/tasks/create-project/)
- Slack Workspace and InviterCo app setup

## Architecture

The nested set of Kustomize manifests in this repository can deploy the following:

- A production instance of gateway using production hostnames.
- A staging instance of a gateway using test hostnames.

The manifest files are nested to allow on the fly selection of production vs. staging environments.

## Deployment

### Clone Repository

- Clone this repository locally to have Kustomize files available for modification to your environment

### Prepare Kustomize Manifests

- Adjust the staging/kustomization.yaml file and/or production/kustomization.yaml file for your environment.

### Deploy using Kustomize

   ```bash
   kubectl apply -k staging/
   kubectl apply -k production/
   ```

### Review Gateway Resources

- Use `kubectl get gateways -o wide` to locate your gateway FQDNs / hostnames.
- Use `kubectl get routes -o wide` to see your routes.
- Use `kubectl get endpointslices -o wide` to see your endpoints.

## Monitoring and Maintenance

- Monitor your deployment in [Datum Cloud Portal](https://cloud.datum.net/login)

## Security Considerations

- All sensitive credentials are stored as Kubernetes secrets
- The service is protected behind the Datum Gateway
