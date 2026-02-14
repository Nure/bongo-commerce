# BongoCommerce Frontend

A modern e-commerce frontend built with **React**, **Vite**, and **Tailwind CSS**, connected to an **AWS Lambda backend** for CRUD operations on products.

---

## Features

- Create, view, and delete products
- Responsive UI with Tailwind CSS
- Single AWS Lambda CRUD backend
- Stores Lambda URL in `.env` file for easy configuration
- Instant UI updates without page reloads

---

## Prerequisites

- Node.js >= **v24.12.0**
- npm >= **10.x** (comes with Node 24)
- AWS Lambda URL for your backend (single CRUD Lambda)

---

## Folder Structure


```bash
BongoCommerce/frontend/
                    ├─ .env
                    ├─ package.json
                    ├─ vite.config.mjs
                    ├─ postcss.config.js
                    ├─ tailwind.config.js
                    ├─ vite.config.mjs
                    ├─ index.html
                    ├─ src/
                    │ ├─ main.jsx
                    │ ├─ index.css
                    │ ├─ App.jsx
                    │ └─ components/
                    │ ├─ ProductForm.jsx
                    │ └─ ProductList.jsx
```

---

## Step 1 — Clone the Repository to EC2 machine

```bash
git clone <your-github-repo-url>
cd BongoCommerce
cd frontend
```
## Step 2 — Create .env File

Create a .env file in the root directory:
```bash 
touch .env
```

Add your AWS Lambda URL:
```bash 
VITE_LAMBDA_URL=https://your-lambda-url.amazonaws.com
```
Replace https://your-lambda-url.amazonaws.com with your real Lambda URL.

## Step 3 — Install Dependencies
```bash 
npm install
```



# BongoCommerce Backend

## Create DynamoDB

### DynamoDB Table Setup

**Table name:** BongoProducts

**Partition key:** productId (String)

No sort key needed

Enable On-Demand capacity for simplicity.

## Create AWS Lambda Function

### Go to your backend directory 
    
    cd backend

## Make a zip file with node_modules, lambda.js, package.json

```bash 
zip -r lambda.zip lambda.js package.json node_modules
```

## In Lambda Function code → Upload the lambda.zip file containing:

```bash 
lambda.js

node_modules/

package.json
```

```bash 
Hit Deploy
```

## AWS Lambda needs to work with DynamoDB
<p>Allows Lambda functions to call AWS services (DynamoDB) on your behalf.</p>

<strong> Create a role with DynamoDBFull Access and CloudWatchEventFull Access </strong>

**Role Name:** BongoCommerceCRUDRole

## Give the new role to Lambda Function

Functions > BongoCommerceCRUD > Configuration > Permissions > Edit > Select the new role

## Create Lambda Function URL

### Function URL > Create function URL > Auth Type = None > Save

Cpy the Function URL and paste it to your **.env** file

```bash 
VITE_LAMBDA_URL=FunctionURL
```

Lambda Function > Runtime settings > Edit > Select Runtime = Node.js 24.x > Write Handler=lambda.handler


### If you want to Run on EC2 Machine:
```bash 
ssh -i your-key.pem ec2-user@EC2_PUBLIC_IP
git clone <your-repo-url>
cd frontend
npm install
```
### Run Vite Dev Server on EC2
    npm run dev -- --host 0.0.0.0

### Set Inbound rules:

    TCP 5173 → your IP (or 0.0.0.0/0 for testing)

### Access from your browser
    http://EC2_PUBLIC_IP:5173
