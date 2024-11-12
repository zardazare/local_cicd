# Local CI/CD Environment with Jenkins and GitLab

This setup provides a local CI/CD environment using Docker with Jenkins and GitLab. It includes a docker_compose.yml configuration for launching both services in separate containers on a custom Docker network with bridge driver.

### Prerequisites

- Docker and Docker Compose installed on your machine.
- Local directories for Jenkins and GitLab data storage (see instructions below).

### Setup

1. Clone this repository and navigate to the directory containing docker_compose.yml.

2. Create the necessary directories for Jenkins and GitLab data persistence on your machine:

    ```bash
    # Jenkins
    mkdir -p /path/to/jenkins_home

    # GitLab
    mkdir -p /path/to/gitlab/config
    mkdir -p /path/to/gitlab/logs
    mkdir -p /path/to/gitlab/data
    ```

3. Update docker_compose.yml: Replace the TODO placeholders with the paths to the directories you just created.

    ```yaml
    volumes:
    - "/path/to/jenkins_home:/var/jenkins_home"
    - "/path/to/gitlab/config:/etc/gitlab"
    - "/path/to/gitlab/logs:/var/log/gitlab"
    - "/path/to/gitlab/data:/var/opt/gitlab"
    ```

4. Modify your hosts file to map the custom hostnames to localhost:

    - Edit your /etc/hosts file (or C:\Windows\System32\drivers\etc\hosts on Windows).

    - Add the following lines:

        ```plaintext
        127.0.0.1 jenkins.localhost
        127.0.0.1 gitlab.localhost
        ```
5. Launch the containers:

    Run the following command to start Jenkins and GitLab:

    ```bash
    docker-compose -f docker_compose.yml up -d --wait
    ```
    - The --wait flag ensures services are fully initialized before they're marked as "up."

### Accessing the Services

- Jenkins: Visit http://jenkins.localhost:36036
- GitLab: Visit http://gitlab.localhost:36037

### Configuration Details

- Jenkins:

    - Ports: 36036 (mapped to Jenkins's internal port 8080)
    - Data storage: /var/jenkins_home mounted from the host directory.

- GitLab:

    - Ports: 36037 (mapped to GitLab's internal port 80)
    - Config, logs, and data are mounted from the host to /etc/gitlab, /var/log/gitlab, and /var/opt/gitlab, respectively.
    - Uncomment and configure the 443 port line if you want to enable HTTPS support.

### Stopping the Services

To stop the services, use:

```bash
docker-compose -f docker_compose.yml down
```

This command will stop and remove the containers but preserve the volumes, allowing data persistence between sessions.
