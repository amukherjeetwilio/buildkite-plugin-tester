# Buildkite Plugin Tester

A base [Docker](https://www.docker.com/) image for testing Buildkite plugins with BATS. It includes:

* [bats](https://github.com/sstephenson/bats)
* [bats-mock](https://github.com/jasonkarns/bats-mock)

Your plugin’s code is expected to be mounted to `/plugin`, and by default the image will run the command `bats tests/`.

## Usage

For example, say your plugin had the following command hook test in `tests/command.bats`:

```bash
load "$BATS_PATH/load.bash"

# Uncomment to add stub debug information:
# export FOO_STUB_DEBUG=/dev/tty

@test "calls git log" {
  stub git "log : echo some-log"
  git log
  assert_success
  unstub git
}
```

To use the plugin-tester, add the following `docker-compose.yml` file to your plugin:

```bash
version: '2'
services:
  tests:
    image: buildkite/plugin-tester
```

Now you can run it locally:

```bash
docker-compose run --rm tests
```

To set up it up in Buildkite, create a `.buildkite/pipeline.yml` file that uses the [docker-compose Buildkite Plugin](https://github.com/buildkite-plugins/docker-compose-buildkite-plugin), for example:

```yml
steps:
  - plugins:
      docker-compose#x.y.z:
        run: tests
```

## Developing

To test plugin-tester, use the following command:

```bash
docker-compose run --rm tests
```

## License

MIT (see [LICENSE](LICENSE))