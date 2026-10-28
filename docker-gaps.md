# Deployment Gaps
In order to take this to production, we will need to address the following gaps:

## Critical 
- **Background jobs**: The existing installer runs Delayed Job workers. This can be implemented with roles in Kamal: It will spin up a worker using the same image.
- **Scheduled tasks**: The existing setup uses Whenever and cron.  Again, this can be achieved by defining another Kamal 'cron' role.
- **Persistent application files**: The installer maintains shared directories for application data. Need to identify which directories are actively used and mount persistent volumes or use external storage so that files survive container replacement and redeployment.
- **Email configuration**: The current Kamal configuration does not specify SMTP settings. We need to configure the required mail server credentials through environment variables or secrets, then test outgoing email.
- **Caching**: The existing installer includes Memcached. Need to confirm whether the application still depends on it and, if so, deploy or provision a compatible caching service, this can be easily configured as an accessory service in Kamal config

## Good to haves
- **CI/CD and automated testing**: The deployment process has not yet been integrated with an automated pipeline. We can use GitHub Actions to run tests, build and publish the image, and trigger deployments after successful checks.
- **Production operations**: Document how secrets, logs, monitoring, server updates, failed deployments, and rollback will be handled. We shoudl define these procedures before using the setup for production workloads.
- **Database setup and recovery**: Verify that the deployment creates the required PostgreSQL extensions and schemas, and applies migrations correctly. Ideally, we should prescribe automated backups and test restoration: Unsure if this is prescribed by consul, or something that the installers generally handle in their own way
