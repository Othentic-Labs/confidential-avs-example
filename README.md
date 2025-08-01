# Confidential AVS Example

---

## Table of Contents

1. [Overview](#overview)
2. [Project Structure](#project-structure)
3. [Architecture](#architecture)
4. [Prerequisites](#prerequisites)
5. [Installation](#installation)
6. [Usage](#usage)

---

## Overview

The Simple Price Oracle AVS Example demonstrates how to deploy a minimal AVS using Othentic Stack.


## Project Structure

```mdx
📂 simple-price-oracle-avs-example
├── 📂 Execution_Service         # Implements Task execution logic - Express JS Backend
│   ├── 📂 config/
│   │   └── app.config.js        # An Express.js app setup with dotenv, and a task controller route for handling `/task` endpoints.
│   ├── 📂 src/
│   │   └── dal.service.js       # A module that interacts with Pinata for IPFS uploads
│   │   ├── oracle.service.js    # A utility module to fetch the current price of a cryptocurrency pair from the Binance API
│   │   ├── task.controller.js   # An Express.js router handling a `/execute` POST endpoint
│   │   ├── 📂 utils             # Defines two custom classes, CustomResponse and CustomError, for standardizing API responses
│   ├── Dockerfile               # A Dockerfile that sets up a Node.js (22.6) environment, exposes port 8080, and runs the application via index.js
|   ├── index.js                 # A Node.js server entry point that initializes the DAL service, loads the app configuration, and starts the server on the specified port
│   └── package.json             # Node.js dependencies and scripts
│
├── 📂 Validation_Service         # Implements task validation logic - Express JS Backend
│   ├── 📂 config/
│   │   └── app.config.js         # An Express.js app setup with a task controller route for handling `/task` endpoints.
│   ├── 📂 src/
│   │   └── dal.service.js        # A module that interacts with Pinata for IPFS uploads
│   │   ├── oracle.service.js     # A utility module to fetch the current price of a cryptocurrency pair from the Binance API
│   │   ├── task.controller.js    # An Express.js router handling a `/validate` POST endpoint
│   │   ├── validator.service.js  # A validation module that checks if a task result from IPFS matches the ETH/USDT price within a 5% margin.
│   │   ├── 📂 utils              # Defines two custom classes, CustomResponse and CustomError, for standardizing API responses.
│   ├── Dockerfile                # A Dockerfile that sets up a Node.js (22.6) environment, exposes port 8080, and runs the application via index.js.
|   ├── index.js                  # A Node.js server entry point that initializes the DAL service, loads the app configuration, and starts the server on the specified port.
│   └── package.json              # Node.js dependencies and scripts
│
├── 📂 grafana                    # Grafana monitoring configuration
├── docker-compose.yml            # Docker setup for Operator Nodes (Performer, Attesters, Aggregator), Execution Service, Validation Service, and monitoring tools
├── .env.example                  # An example .env file containing configuration details and contract addresses
├── README.md                     # Project documentation
└── prometheus.yaml               # Prometheus configuration for logs
```

## Architecture

![Price oracle sample](https://github.com/user-attachments/assets/03d544eb-d9c3-44a7-9712-531220c94f7e)

The Performer node executes tasks using the Task Execution Service and sends the results to the p2p network.

Attester Nodes validate task execution through the Validation Service. Based on the Validation Service's response, attesters sign the tasks. In this AVS:

Task Execution logic:


Validation Service logic:

---

## Prerequisites

- Node.js (v 22.6.0 )
- Foundry
- [Yarn](https://yarnpkg.com/)
- [Docker](https://docs.docker.com/engine/install/)

## Usage

1. Clone the repository:

   ```bash
   git clone https://github.com/Othentic-Labs/simple-price-oracle-avs-example.git
   cd simple-price-oracle-avs-example
   git checkout kyc-avs
   ```

2. Install Othentic CLI:

   ```bash
   npm i -g @othentic/cli
   npm i -g @othentic/node
   ```

3. Set up the TEE server by following the instructions below. Build the Docker image and start the server. Make sure to populate the `.env` file with `TEE_KYC_SERVER_URL` and any other required environment variables.
   ```bash
   cd ..
   git clone https://github.com/scrtlabs/kyc-avs-demo
   cd kyc-avs-demo
   docker build -t kyc-avs .
   ```

4. Follow the steps in the official documentation's [Quickstart](https://docs.othentic.xyz/main/welcome/getting-started/install-othentic-cli) Guide for setup and deployment.

   ```
   cd simple-price-oracle-avs-example
   docker compose build --no-cache
   docker compose up
   curl -X POST http://localhost:4003/task/execute
   ```

Happy Building! 🚀

