<!-- markdownlint-disable MD030 -->

# Flowise - Deploy to Render.com

## Prerequisites

1. A GitHub account.
2. A Render account (free or starter plan depending on use case).
3. Access to the chatbot’s source code repository on GitHub.
4. Environment variables required for the chatbot (e.g., username, password, Node.js version).

---

## Step 1: Fork the Chatbot Repository

1. Log in to your GitHub account.
2. Navigate to the chatbot repository and click on the **Fork** button.
3. Authorize and confirm the fork.
4. _(Optional)_ Synchronize the fork if updates are available:
    - Go to **Settings** > **Synchronize Fork** > **Update Branch**.

---

## Step 2: Set Up a Render Account

1. Visit [Render](https://render.com) and create an account.
2. Log in using Google or manually create an account.
3. Select **Start for Free** for testing or upgrade to the **Starter Plan** for permanent hosting.

---

## Step 3: Create a New Web Service on Render

1. Go to the Render dashboard and click **New** > **Web Service**.
2. Choose **Deploy from a Git Repository**.
3. If prompted, connect your GitHub account.
4. Select the forked repository containing the chatbot code.
5. Name the service (e.g., `flowwise-chatbot`).
6. Choose your region (e.g., Frankfurt EU Central for EU users).
7. Leave the branch as `main`.

---

## Step 4: Configure Runtime

1. **Runtime Environment**: Choose `Docker`.
2. **Plan**:
    - Free Plan: Suitable for testing but has limitations (e.g., no persistent storage, services spin down after inactivity).
    - Starter Plan: Required for permanent hosting, persistent disk, and faster response times.

---

## Step 5: Set Environment Variables

1. Refer to the chatbot documentation for the required environment variables:
    - `FLOWWISE_USERNAME` (e.g., `admin1`)
    - `FLOWWISE_PASSWORD` (e.g., `your_password`)
    - `NODE_VERSION` (e.g., `18.1`)
2. Add these variables on Render:
    - Go to **Environment Variables**.
    - Add variables one by one, entering the name and value.
3. For Starter Plan users, add additional variables for persistent storage:
    - `DATABASE_PATH`: `/opt/render/.flowwise`
    - `API_KEY_PATH`: `/opt/render/.flowwise`
    - `LOG_PATH`: `/opt/render/.flowwise/logs`
    - `SECRET_KEY_PATH`: `/opt/render/.flowwise`

---

## Step 6: Configure Persistent Disk (Starter Plan Only)

1. Navigate to **Advanced Settings** and enable a persistent disk.
2. Mount path: `/opt/render/.flowwise`.
3. Disk size: Set to `1 GB`.

---

## Step 7: Deploy the Service

1. Click **Deploy Web Service**.
2. Wait for the build and deployment to complete (2–5 minutes).
3. Once live, note the service URL for accessing the chatbot.

---

## Step 8: Test the Chatbot

1. Access the chatbot using the service URL.
2. Log in with the credentials you set (`FLOWWISE_USERNAME` and `FLOWWISE_PASSWORD`).
3. Test creating and managing chat flows to ensure the chatbot is functional.

---

## Key Considerations

-   **Free Plan**: Suitable for testing but has limitations (no persistent disk, services spin down after inactivity).
-   **Starter Plan**: Required for permanent hosting; costs approximately $7/month.
-   **Client Use**: Charge clients for hosting and maintenance, potentially earning significant revenue.

---

## Next Steps

-   Explore integrating the hosted chatbot into websites or client projects (to be covered in the next tutorial).

# Flowise - Build LLM Apps Easily

[![Release Notes](https://img.shields.io/github/release/FlowiseAI/Flowise)](https://github.com/FlowiseAI/Flowise/releases)
[![Discord](https://img.shields.io/discord/1087698854775881778?label=Discord&logo=discord)](https://discord.gg/jbaHfsRVBW)
[![Twitter Follow](https://img.shields.io/twitter/follow/FlowiseAI?style=social)](https://twitter.com/FlowiseAI)
[![GitHub star chart](https://img.shields.io/github/stars/FlowiseAI/Flowise?style=social)](https://star-history.com/#FlowiseAI/Flowise)
[![GitHub fork](https://img.shields.io/github/forks/FlowiseAI/Flowise?style=social)](https://github.com/FlowiseAI/Flowise/fork)

English | [中文](./i18n/README-ZH.md) | [日本語](./i18n/README-JA.md) | [한국어](./i18n/README-KR.md)

<h3>Drag & drop UI to build your customized LLM flow</h3>
<a href="https://github.com/FlowiseAI/Flowise">
<img width="100%" src="https://github.com/FlowiseAI/Flowise/blob/main/images/flowise.gif?raw=true"></a>

## ⚡Quick Start

Download and Install [NodeJS](https://nodejs.org/en/download) >= 18.15.0

1. Install Flowise
    ```bash
    npm install -g flowise
    ```
2. Start Flowise

    ```bash
    npx flowise start
    ```

    With username & password

    ```bash
    npx flowise start --FLOWISE_USERNAME=user --FLOWISE_PASSWORD=1234
    ```

3. Open [http://localhost:3000](http://localhost:3000)

## 🐳 Docker

### Docker Compose

1. Clone the Flowise project
2. Go to `docker` folder at the root of the project
3. Copy `.env.example` file, paste it into the same location, and rename to `.env` file
4. `docker compose up -d`
5. Open [http://localhost:3000](http://localhost:3000)
6. You can bring the containers down by `docker compose stop`

### Docker Image

1. Build the image locally:
    ```bash
    docker build --no-cache -t flowise .
    ```
2. Run image:

    ```bash
    docker run -d --name flowise -p 3000:3000 flowise
    ```

3. Stop image:
    ```bash
    docker stop flowise
    ```

## 👨‍💻 Developers

Flowise has 3 different modules in a single mono repository.

-   `server`: Node backend to serve API logics
-   `ui`: React frontend
-   `components`: Third-party nodes integrations
-   `api-documentation`: Auto-generated swagger-ui API docs from express

### Prerequisite

-   Install [PNPM](https://pnpm.io/installation)
    ```bash
    npm i -g pnpm
    ```

### Setup

1.  Clone the repository

    ```bash
    git clone https://github.com/FlowiseAI/Flowise.git
    ```

2.  Go into repository folder

    ```bash
    cd Flowise
    ```

3.  Install all dependencies of all modules:

    ```bash
    pnpm install
    ```

4.  Build all the code:

    ```bash
    pnpm build
    ```

    <details>
    <summary>Exit code 134 (JavaScript heap out of memory)</summary>  
      If you get this error when running the above `build` script, try increasing the Node.js heap size and run the script again:

        export NODE_OPTIONS="--max-old-space-size=4096"
        pnpm build

    </details>

5.  Start the app:

    ```bash
    pnpm start
    ```

    You can now access the app on [http://localhost:3000](http://localhost:3000)

6.  For development build:

    -   Create `.env` file and specify the `VITE_PORT` (refer to `.env.example`) in `packages/ui`
    -   Create `.env` file and specify the `PORT` (refer to `.env.example`) in `packages/server`
    -   Run

        ```bash
        pnpm dev
        ```

    Any code changes will reload the app automatically on [http://localhost:8080](http://localhost:8080)

## 🔒 Authentication

To enable app level authentication, add `FLOWISE_USERNAME` and `FLOWISE_PASSWORD` to the `.env` file in `packages/server`:

```
FLOWISE_USERNAME=user
FLOWISE_PASSWORD=1234
```

## 🌱 Env Variables

Flowise support different environment variables to configure your instance. You can specify the following variables in the `.env` file inside `packages/server` folder. Read [more](https://github.com/FlowiseAI/Flowise/blob/main/CONTRIBUTING.md#-env-variables)

## 📖 Documentation

[Flowise Docs](https://docs.flowiseai.com/)

## 🌐 Self Host

Deploy Flowise self-hosted in your existing infrastructure, we support various [deployments](https://docs.flowiseai.com/configuration/deployment)

-   [AWS](https://docs.flowiseai.com/deployment/aws)
-   [Azure](https://docs.flowiseai.com/deployment/azure)
-   [Digital Ocean](https://docs.flowiseai.com/deployment/digital-ocean)
-   [GCP](https://docs.flowiseai.com/deployment/gcp)
-   [Alibaba Cloud](https://computenest.console.aliyun.com/service/instance/create/default?type=user&ServiceName=Flowise社区版)
-   <details>
      <summary>Others</summary>

    -   [Railway](https://docs.flowiseai.com/deployment/railway)

        [![Deploy on Railway](https://railway.app/button.svg)](https://railway.app/template/pn4G8S?referralCode=WVNPD9)

    -   [Render](https://docs.flowiseai.com/deployment/render)

        [![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://docs.flowiseai.com/deployment/render)

    -   [HuggingFace Spaces](https://docs.flowiseai.com/deployment/hugging-face)

        <a href="https://huggingface.co/spaces/FlowiseAI/Flowise"><img src="https://huggingface.co/datasets/huggingface/badges/raw/main/open-in-hf-spaces-sm.svg" alt="HuggingFace Spaces"></a>

    -   [Elestio](https://elest.io/open-source/flowiseai)

        [![Deploy on Elestio](https://elest.io/images/logos/deploy-to-elestio-btn.png)](https://elest.io/open-source/flowiseai)

    -   [Sealos](https://cloud.sealos.io/?openapp=system-template%3FtemplateName%3Dflowise)

        [![](https://raw.githubusercontent.com/labring-actions/templates/main/Deploy-on-Sealos.svg)](https://cloud.sealos.io/?openapp=system-template%3FtemplateName%3Dflowise)

    -   [RepoCloud](https://repocloud.io/details/?app_id=29)

        [![Deploy on RepoCloud](https://d16t0pc4846x52.cloudfront.net/deploy.png)](https://repocloud.io/details/?app_id=29)

      </details>

## ☁️ Flowise Cloud

[Get Started with Flowise Cloud](https://flowiseai.com/)

## 🙋 Support

Feel free to ask any questions, raise problems, and request new features in [discussion](https://github.com/FlowiseAI/Flowise/discussions)

## 🙌 Contributing

Thanks go to these awesome contributors

<a href="https://github.com/FlowiseAI/Flowise/graphs/contributors">
<img src="https://contrib.rocks/image?repo=FlowiseAI/Flowise" />
</a>

See [contributing guide](CONTRIBUTING.md). Reach out to us at [Discord](https://discord.gg/jbaHfsRVBW) if you have any questions or issues.
[![Star History Chart](https://api.star-history.com/svg?repos=FlowiseAI/Flowise&type=Timeline)](https://star-history.com/#FlowiseAI/Flowise&Date)

## 📄 License

Source code in this repository is made available under the [Apache License Version 2.0](LICENSE.md).
