# Security Policy

LifeLink Local is a development package, not a production medical or identity system. Do not place real patient information, donor medical information, production passwords, API tokens, or private keys in this repository.

The in-memory API uses a development authentication seam and resets data when it stops. Before production use, connect a verified identity provider, enforce ownership from verified claims, add rate limiting and audit logging, use HTTPS, review privacy and retention requirements, and validate all workflows with the responsible clinical or blood-bank organization.

To report a vulnerability, do not open a public issue containing sensitive details. Contact the repository owner privately through GitHub.
