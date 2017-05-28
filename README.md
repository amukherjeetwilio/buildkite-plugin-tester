# Buildkite Plugin Tester

A base [Docker](https://www.docker.com/) image for testing Buildkite plugins with BATS. It includes:

* [bats](https://github.com/sstephenson/bats)
* [bats-mock](https://github.com/jasonkarns/bats-mock)

Your plugin’s code is expected to be mounted to `/plugin`, and by default the image will run the command `bats tests/`.

## Usage

Add the following `docker-compose.yml` file to your plugin:

```bash
version: '2'
services:
  tests:
    image: buildkite/plugin-tester
```

Test it locally:

```bash
docker-compose run --rm tests
```

To set up CI, you can create a `.buildkite/pipeline.yml` file and use the [docker-compose Buildkite Plugin](https://github.com/buildkite-plugins/docker-compose-buildkite-plugin), for example:

```yml
steps:
  - plugins:
      docker-compose#x.y.z:
        run: tests
```

## License

MIT (see [LICENSE](LICENSE))