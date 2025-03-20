## Hard Test 2

Screengrab for setting up workspace on DevPod 

![image](https://github.com/avinabneogy23/r-dev-env-test/blob/main/assets/hard_15.png)

### Key Steps and Issues Encountered

#### 1. Docker Group Permissions

Initially, DevPod lacked sufficient permissions to access the Docker daemon socket. This was resolved by:

- Adding the user to the Docker group
- Refreshing the session to apply group changes

![image](https://github.com/avinabneogy23/r-dev-env-test/blob/main/assets/hard_10.png)



#### 2. Container Image Reference Mismatch

After resolving permission issues, DevPod failed to find the specified container image. The error message indicated:

```
Image ghcr.io/r-devel/r-dev-env:main not found
```
![image](https://github.com/avinabneogy23/r-dev-env-test/blob/main/assets/hard_11.png)


#### 3. Local Image Creation and Workspace Setup

To address the image issue, we created a local Docker image with the proper directory structure:

1. Built a local Docker image:

```bash
docker run -d --name r-dev-container \
  -v "$(pwd):/workspaces/r-dev-env" \
  -w /workspaces/r-dev-env \
  rocker/r-ver:latest \
  sleep infinity
```

2. Configured the container:

```bash
docker exec r-dev-container bash -c "mkdir -p /workspaces/r-dev-env"
docker exec r-dev-container bash -c "chown -R $(id -u):$(id -g) /workspaces/r-dev-env"
docker exec r-dev-container bash -c "apt-get update && apt-get install -y git"
```

3. Modified devcontainer.json to use the local image:

```json
{
  "name": "R-Dev-Env",
  "image": "r-dev-env:local",
  "workspaceFolder": "/workspaces/r-dev-env",
  "workspaceMount": "source=${localWorkspaceFolder},target=/workspaces/r-dev-env,type=bind,consistency=cached"
}
```

4. Created a DevPod workspace using the local image

![image](https://github.com/avinabneogy23/r-dev-env-test/blob/main/assets/hard_12.png)

Screengrab of positron connected to the Devpod workspace using the local image 

![image](https://github.com/avinabneogy23/r-dev-env-test/blob/main/assets/hard_13.png)

Screengrab of a sample R program 

```R
hist(rnorm(1000))
```

![image](https://github.com/avinabneogy23/r-dev-env-test/blob/main/assets/hard_14.png)

