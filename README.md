# To the Power of Math! 🔢

A lightweight, serverless web app that calculates exponents (`base ^ power`) in real time — built to explore a full AWS serverless architecture end-to-end, from static frontend hosting to a REST API backed by a NoSQL database.

**🔗 Live demo:** [staging.drpc7v0uo6b3g.amplifyapp.com](https://staging.drpc7v0uo6b3g.amplifyapp.com)

---

## Overview

"To the Power of Math!" lets a user enter a **base number** and an **exponent**, click **Calculate**, and instantly get the result — computed server-side via a Lambda function rather than in the browser. It's a small project, but it's built the way a production serverless app would be: static hosting + CI/CD, an API layer, compute, persistence, and least-privilege IAM roles wired together.

## Features

- Simple, single-page UI — enter a base number and a power, get the result instantly
- Calculation is performed server-side (not client-side JS math), via an API call to AWS Lambda
- Fully serverless backend — no servers to manage or patch
- Continuous deployment from Git via AWS Amplify

## Architecture

```
User Browser
     │
     ▼
AWS Amplify (static hosting + CI/CD)
     │
     │  API call
     ▼
Amazon API Gateway  (REST endpoint)
     │
     ▼
AWS Lambda  (computes base ^ power)
     │
     ▼
Amazon DynamoDB  (stores/logs calculations)
```

- **AWS Amplify** — hosts the frontend and handles CI/CD, so every push to the connected branch auto-deploys
- **Amazon API Gateway** — exposes a REST endpoint that the frontend calls with the base/power values
- **AWS Lambda** — receives the request, performs the exponent calculation, and returns the result
- **Amazon DynamoDB** — stores calculation records (e.g. inputs, result, timestamp)
- **IAM** — a scoped policy grants the Lambda function only the permissions it needs (e.g. `dynamodb:PutItem`) rather than broad access

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | HTML / CSS / JavaScript |
| Hosting & CI/CD | AWS Amplify |
| API Layer | Amazon API Gateway |
| Compute | AWS Lambda |
| Database | Amazon DynamoDB |
| Access Control | AWS IAM |

## How It Works

1. The user enters a base number and an exponent in the two input fields.
2. Clicking **Calculate** sends a request to the API Gateway endpoint with both values.
3. API Gateway triggers the Lambda function, which computes `base ^ power`.
4. The Lambda function writes the calculation to DynamoDB and returns the result.
5. The frontend displays the result to the user.

## Getting Started (Local Setup)

```bash
# Clone the repository
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>

# Install dependencies (if applicable)
npm install

# Run locally
npm start
```

> Replace the commands above with your actual setup steps if they differ (e.g. plain static file, or a bundler like Vite).

## Deployment

This project is deployed via **AWS Amplify**, connected directly to this repository. Any push to the main/staging branch triggers an automatic build and deploy.

To deploy your own copy:
1. Fork/clone this repo
2. Connect the repo to a new Amplify app in the AWS Console
3. Set up the API Gateway + Lambda + DynamoDB backend (see `/backend` if included, or recreate manually)
4. Attach an IAM role to the Lambda with least-privilege permissions for DynamoDB access
5. Deploy

## IAM Permissions

The Lambda execution role follows least-privilege principles, granted only what it needs to:
- Write calculation logs to the specific DynamoDB table
- Write logs to Amazon CloudWatch

## Future Improvements

- [ ] Add input validation and error handling for non-numeric inputs
- [ ] Support negative/fractional exponents
- [ ] Add a calculation history view (reading back from DynamoDB)
- [ ] Add unit tests for the Lambda function

## Author

**Sumit Kumar Sharma**
B.Tech IT, DR BC ROY ENGINEERING COLLEGE, DURGAPUR

---

*This project was built as a hands-on exercise in designing and deploying a serverless architecture on AWS.*
