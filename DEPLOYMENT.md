# AWS App Runner Deployment Guide

This guide will help you deploy your Express.js application to AWS App Runner.

## Prerequisites

- AWS Account
- AWS CLI configured (optional, can use AWS Console)
- GitHub repository: https://github.com/SFitz911/multi-region-app.git

## Deployment Steps

### Option 1: Deploy via AWS Console

1. **Navigate to AWS App Runner**
   - Go to AWS Console → App Runner
   - Click "Create service"

2. **Source Configuration**
   - Choose "Source code repository"
   - Select "GitHub" as your provider
   - Authorize AWS to access your GitHub account (if first time)
   - Select repository: `SFitz911/multi-region-app`
   - Branch: `main`
   - Deployment trigger: "Automatic" (deploys on every push)

3. **Build Settings**
   - Build type: "Automatic"
   - App Runner will auto-detect Node.js and use `npm install` and `npm start`

4. **Service Settings**
   - Service name: `multi-region-app` (or your preferred name)
   - Virtual CPU: 0.25 vCPU (minimum, can increase if needed)
   - Memory: 0.5 GB (minimum, can increase if needed)
   - Port: `8080` (App Runner will set PORT environment variable automatically)

5. **Review and Create**
   - Review all settings
   - Click "Create & deploy"

### Option 2: Deploy via AWS CLI

```bash
aws apprunner create-service \
  --service-name multi-region-app \
  --source-configuration '{
    "CodeRepository": {
      "RepositoryUrl": "https://github.com/SFitz911/multi-region-app",
      "SourceCodeVersion": {
        "Type": "BRANCH",
        "Value": "main"
      },
      "CodeConfiguration": {
        "ConfigurationSource": "REPOSITORY",
        "CodeConfigurationValues": {
          "Runtime": "NODEJS_18",
          "BuildCommand": "npm install",
          "StartCommand": "npm start",
          "RuntimeEnvironmentVariables": {
            "PORT": "8080"
          }
        }
      }
    },
    "AutoDeploymentsEnabled": true
  }' \
  --instance-configuration '{
    "Cpu": "0.25 vCPU",
    "Memory": "0.5 GB"
  }'
```

## Environment Variables

App Runner automatically sets the `PORT` environment variable. Your application uses:
```javascript
const PORT = process.env.PORT || 8080;
```

This will work automatically with App Runner.

## Post-Deployment

After deployment:
1. App Runner will provide a service URL (e.g., `https://xxxxx.us-east-1.awsapprunner.com`)
2. Visit the URL to see "Hello from AWS!"
3. The service will auto-deploy on every push to the `main` branch

## Monitoring

- View logs in AWS App Runner console
- Monitor service health and metrics
- Set up CloudWatch alarms if needed

## Cost Considerations

- App Runner charges based on:
  - vCPU hours
  - Memory GB-hours
  - Requests processed
- Minimum configuration (0.25 vCPU, 0.5 GB) is suitable for low-traffic applications
- Review AWS App Runner pricing for current rates

## Troubleshooting

- **Build fails**: Check that `package.json` has correct `start` script
- **Port errors**: Ensure app listens on `process.env.PORT`
- **Deployment issues**: Check App Runner logs in AWS Console
