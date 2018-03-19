# Locust Docker
[![](https://images.microbadger.com/badges/image/netrasys/locust.svg)](https://microbadger.com/images/netrasys/locust)

Docker image for the [Locust](http://locust.io/) load testing tool and sample Kubernetes configuration files for distributed deployment.

## Supported tags and respective `Dockerfile` links

- [`1.1.1`, `1.1`, `latest`  (*1.1/Dockerfile*)](https://github.com/netrasys/locust-docker/tree/master/1.1)
- [`1.1.1-python2`, `1.1-python2`, `python2`  (*1.1/python2/Dockerfile*)](https://github.com/netrasys/locust-docker/tree/master/1.1/python2)

## Usage
The Docker image can be run standalone by passing a URL to your locustfile:

```bash
docker run -d -p 8089:8089 \
-e LOCUST_LOCUSTFILE_URL='https://example.com/locustfile.py' \
-e LOCUST_TARGET_HOST='http://example.com' \
--name locust peterevans/locust:latest
```
Then point your web browser to [http://localhost:8089/](http://localhost:8089/)

It can also run in `no-web` mode by passing `LOCUST_EXTRA_FLAGS_MASTER`:

```bash
docker run -d -p 8089:8089 \
-e LOCUST_LOCUSTFILE_URL='https://example.com/locustfile.py' \
-e LOCUST_TARGET_HOST='http://example.com' \
-e LOCUST_EXTRA_FLAGS_MASTER='--csv=foobar --no-web -c 100 -r 10' \
--name locust peterevans/locust:latest
```
This will save the test results periodically to a CSV file.

## Kubernetes Deployment

1. Create a ConfigMap containing your locustfile.py and its dependencies. The command below creates a ConfigMap containing files placed in the local directory `locust-tasks`.

	```bash
	kubectl create configmap locust-configmap --from-file=locust-tasks/
	```

2. Edit the deployment configuration files and set environment variable `LOCUST_TARGET_HOST`.

3. Deploy the master and slave deployments.

	```bash
    kubectl create -f ./locust-master.yaml
    kubectl create -f ./locust-slave.yaml
    ```

## License

MIT License - see the [LICENSE](LICENSE) file for details
