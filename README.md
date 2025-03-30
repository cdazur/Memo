<picture>
<img src="https://raw.githubusercontent.com/MoonKraken/DrawsNotes/main/demo.gif" />
</picture>

# DrawsNotes

More details about the creation of this project can be found in [this video](https://youtu.be/Pr6T0Phjvgc).

A [later video](https://youtu.be/zLTaN5fJUY8?si=VC4r-pVeFnuMIfnC) demonstrated how to deploy it with Terraform and GitLab CI/CD.

A very simple note-taking app built with an all-Rust stack: Dioxus for the frontend and backend (Axum under the hood on the backend), and SurrealDB as the database.

1. `cargo install dioxus-cli` *Run this again even if you've done it before, outdated versions may cause breakage*
1. `rustup target add wasm32-unknown-unknown`
1. Install SurrealDB (on a mac, `brew install surrealdb/tap/surreal`)
1. Start surrealdb with no authentication `surreal start --unauthenticated` *We assume it is running on the same machine as DrawsNotes, you'll need to make a code change if it is to run elsewhere*
1. Install tailwindcss if you don't already have it `npm install -g tailwindcss`
1. In this project directory
    1. `npx tailwindcss -i ./input.css -o ./public/tailwind.css --watch`
    1. `dx serve`
  

# YouTube video 
### Tech Stack

* **Frontend Framework:** Dioxus
* **Backend Framework:** Axum (mentioned as an option)
* **Database:** SurrealDB
* **Terminal:** Warp

### Prerequisites

* **Rust Toolchain:** Ensure you have Rust installed. You'll need Cargo, Rust's package manager and build tool.
    * You can install it from: [Rust Installation](https://www.rust-lang.org/tools/install)
* **Dioxus CLI:** Install the Dioxus command-line interface.
    * `cargo install dioxus-cli`
* **Docker:** Required for containerization.
    * Install Docker: [Docker Installation](https://docs.docker.com/get-docker/)
* **Terraform:** For infrastructure as code.
    * Install Terraform: [Terraform Installation](https://learn.hashicorp.com/tutorials/terraform/install)
* **AWS CLI:** For interacting with Amazon Web Services.
    * Install AWS CLI: [AWS CLI Installation](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
* **GitLab Account:** For CI/CD pipelines.

### Setup Instructions

1.  **Clone the Repository:**
    * `git clone <repository_url>` (Replace `<repository_url>` with the actual URL of the project's repository).
2.  **Install SurrealDB:**
    * The video mentions different storage options for SurrealDB. For local development, you can use a file-based storage.
    * Start SurrealDB: `surreal start file:notes.db`
3.  **Backend Dependencies:**
    * Navigate to the backend directory (if the project is structured that way).
    * Add `surrealdb` as an optional dependency with the `ssr` feature. The video uses a custom workflow for this, but you can manually edit your `Cargo.toml` file.
4.  **Frontend Dependencies:**
    * Navigate to the frontend directory (if applicable).
    * Install any necessary JavaScript dependencies using `npm` or `yarn` if the Dioxus project uses any.

### Running the Application (Development)

1.  **Start the Backend:**
    * If you're using Axum or another backend framework, follow its specific instructions to run the server.
2.  **Run the Frontend:**
    * Use the Dioxus CLI to build and serve the frontend with hot reloading:
    * `dx build`
    * This command typically compiles the Rust frontend code and serves it, often with hot-reloading capabilities for development.
3.  **Access the Application:**
    * Open your web browser and go to the address where the Dioxus app is being served (usually `http://localhost:8080` or similar).

### Deployment to Production

1.  **Create Dockerfile:**
    * Create a file named `Dockerfile` in the root of your project.
    * Use a Rust base image for building.
    * Set the working directory and copy necessary files.
    * Install required packages and set environment variables. Set `IP` to `0.0.0.0` for Docker container deployment.
    * Example Dockerfile content (adapt as necessary):

        ```dockerfile
        FROM rust:1.74 as builder
        WORKDIR /app
        COPY . .
        RUN cargo build --release
        FROM debian:bookworm-slim
        COPY --from=builder /app/target/release/your_app_name /app/your_app_name
        CMD ["/app/your_app_name"]
        ```

2.  **AWS Infrastructure Setup with Terraform:**
    * Create a `terraform` directory within your project.
    * Define your infrastructure using Terraform modules, including networking, SurrealDB cluster, and the application cluster.
    * Set up an ECS cluster and load balancers.
    * Ensure your service has a `/health` endpoint for load balancer health checks.
    * Use `terraform plan` to preview changes and `terraform apply` to deploy them.
    * Example Terraform configuration (adapt as necessary):

        ```terraform
        # Example - requires proper AWS credentials and configuration
        terraform {
          required_providers {
            aws = {
              source  = "hashicorp/aws"
              version = "~> 4.0"
            }
          }
        }
        provider "aws" {
          region = "your_aws_region"
        }

        # Define resources here (VPC, ECS Cluster, Load Balancer, etc.)
        ```

3.  **GitLab CI/CD Pipeline Setup:**
    * Create a file named `.gitlab-ci.yml` in your project.
    * Define stages (e.g., build, test, deploy) and jobs within those stages.
    * Configure the pipeline to build the Docker image, run tests, and deploy to AWS ECS.
    * Set up AWS credentials and other necessary variables in your GitLab project settings.
    * The deploy stage should update the ECS service with the new image, ensuring zero downtime.
    * Example `.gitlab-ci.yml` content (adapt as necessary):

        ```yaml
        stages:
          - build
          - test
          - deploy

        build:
          stage: build
          # Define build steps here (e.g., docker build)

        test:
          stage: test
          # Define test steps here

        deploy:
          stage: deploy
          # Define deployment steps here (e.g., update ECS service)
        ```

4.  **Deployment and Verification:**
    * Push changes to GitLab to trigger the CI/CD pipeline.
    * Monitor the pipeline in GitLab to ensure successful deployment.
    * Retrieve the public DNS address of the load balancer from the AWS console.
    * Access the application in a web browser using the load balancer's DNS address.

### Additional Notes

* **Database Setup:** The video uses an in-memory or file-based SurrealDB for development. For production, you'd likely use a TiKV cluster.
* **Environment Variables:** If your application requires environment variables (e.g., for database connection strings, AWS credentials), make sure to set them appropriately in your Dockerfile, Terraform configuration, and GitLab CI/CD variables.
* **Warp Terminal:** While the video highlights Warp, it's not strictly required to run the app. You can use any terminal.
* **Error Handling:** The video demonstrates using Warp's AI integration to help with Rust errors. If you encounter issues, consider using similar tools or online resources to debug.
* **Workflows:** The video showcases Warp's workflow feature for automating tasks. You can adapt these concepts to your development environment.
* **Infrastructure as Code:** This tutorial uses Terraform for infrastructure as code, which is recommended for efficient and repeatable deployments.
* **GitLab Duo:** The video also shows how GitLab Duo can assist in generating and understanding code snippets.

This README provides a more comprehensive guide, including production deployment. You'll need to adapt it based on the specific structure and requirements of the project you're working with.
