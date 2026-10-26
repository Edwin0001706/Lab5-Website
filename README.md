# Lab 5 — Application Development and Deployment Using GitHub, Jenkins, and Docker

This is a simple static website for the Lab 5 CI/CD assignment.

## Project files

- `index.html` — website content
- `styles.css` — website styling
- `Dockerfile` — packages the website in an Nginx container
- `Jenkinsfile` — Jenkins pipeline to validate, build, and deploy the Docker image

## Run locally with Docker

Open PowerShell in this folder and run:

```powershell
docker build -t lab5-website:latest .
docker run -d --name lab5-website -p 8081:80 lab5-website:latest
```

Then open http://localhost:8081 in your browser.

To stop and remove the container:

```powershell
docker rm -f lab5-website
```

## GitHub and Jenkins setup

1. Create a GitHub repository named `Lab5-Website` (or another name approved by your instructor).
2. Upload the files in this folder to the repository.
3. In Jenkins, create a Pipeline job and configure its Pipeline script source as **Pipeline script from SCM**.
4. Select **Git**, enter your GitHub repository URL, set the branch to `*/main`, and set Script Path to `Jenkinsfile`.
5. Save and click **Build Now**.
6. To trigger builds on GitHub pushes, configure a GitHub webhook pointing to your Jenkins server's webhook endpoint. A local-only Jenkins instance may need a reachable URL for GitHub to call it.

## Important notes

- Jenkins must be able to run the `docker` command and communicate with the Docker Desktop engine. If the pipeline reports that Docker cannot be found or cannot connect to the daemon, check Jenkins service permissions and Docker CLI availability.
- The pipeline deploys the site on port `8081`. If that port is already in use, change `-p 8081:80` in the Jenkinsfile and local run command.
- This project uses a static HTML/CSS site served by Nginx in Docker, so Node.js or Maven is not required for the website itself.
