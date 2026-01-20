# Multi-Region App - Express.js Server

A simple Express.js web server configured for deployment to AWS App Runner.

## Features

- Express.js server
- Single route at "/" returning "Hello from AWS!"
- Configured to listen on process.env.PORT (defaults to 8080)
- Ready for AWS App Runner deployment

## Setup

1. Install dependencies:
   `ash
   npm install
   `

2. Run locally:
   `ash
   npm start
   `

3. The server will start on port 8080 (or the port specified in PORT environment variable)

## AWS App Runner Deployment

This application is configured to work with AWS App Runner. App Runner will:
- Automatically detect the package.json file
- Run 
pm install to install dependencies
- Execute the start script to launch the application
- Use the PORT environment variable provided by App Runner

## License

MIT