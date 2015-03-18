# Docker Registry

This Docker image contains a Docker registry you can use to run your own, private repository in your organization. Please see this [blog post][https://blog.codecentric.de/en/2014/02/docker-registry-run-private-docker-image-repository/] for details.

In the default configuration (local), the registry container uses a local directory in the file system to store images. This directory is configured to use a separate volume `/docker-registry-storage` which can be mapped by the `docker run -v` parameter -- see `Makefile` target `start-registry`. In order to correctly run parallel work threads, the configuration file `config.yml` contains a `secret_key`. 

In case you would like to customize the Docker image, please clone the corresponding [GitHub Repository][github-repo] and follow the instructions below. There is also a Vagrant file included in case you are on a system not directly supported by Docker.


## Customization

This Docker registry image is prepared for customizing the storage backend, the `secret_key`, and the flavor to run:

* In order to change the storage backend, edit the configuration file template `config.yml.template`. For S3 there are already the necessary keys which only need to be filled. Then run 
  `make -B config.yml registry`
* In case you would like to change the secret key, edit the same template as above and replace `@@SEC_KEY@@` with your key of choice. If you keep the file unchanged, the command above will automatically create a random key using *openssl*.
* If you choose to run a different flavor, e.g., because you changed the storage backend, you can overwrite the environment variable `FLAVOR` to select the desired flavor. See below.

## Running

Run from Docker Index -- you can omit the *-e* parameter to run the `local` flavor:

    docker run --name registry -d -e FLAVOR=<flavor> -p 5000:5000 -v <absolute path to storage>:/docker-registry-storage lukaspustina/registry

Run from GitHub repository:

* By default, the registry can be started in flavor `local` by running `make start-registry`
* In order to select a different flavor, use `FLAVOR=<your flavor> make -e start-registry`
* To stop the registry, run `make stop-registry`
* To clean all artifacts including the locally stored images, run `make clean`

[blog-post]: blog
[github-repo]: https://github.com/lukaspustina/docker-registry

