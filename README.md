# Load Testing

load-testing is a tool that lets you to benchmark post api request.

# Roadmap
- Search an available crate to write its own loading test.
- Focused on `drill` and `rlt`.
- Choice on `rlt`, I create a very simple benchmark to test load testing on post api request.

# Warning

The API server must be launch before.

In the following examples we show how to use this tool with the promocode API. 

[https://github.com/tooremdrama/promocode]

Test on is-promocode-valid has Error as response.

Test on add-promocode has Success as response only for the first request because we can't add a promo code with a name already used.


# Examples of command to test promocode API :

Infinite mode

```
cargo run --release http://localhost:8080/is-promocode-valid "{\"promocode_name\": \"test_is_valid_promocode_meteo_invalid\", \"arguments\": { \"age\": 20, \"town\": \"lyon\" }}"
```

With a limit of 1000 iterations

```
cargo run --release http://localhost:8080/is-promocode-valid "{\"promocode_name\": \"test_is_valid_promocode_meteo_invalid\", \"arguments\": { \"age\": 20, \"town\": \"lyon\" }}" -n 10000
```

With a limit of 8 workers to run concurrently

```
cargo run --release http://localhost:8080/is-promocode-valid "{\"promocode_name\": \"test_is_valid_promocode_meteo_invalid\", \"arguments\": { \"age\": 20, \"town\": \"lyon\" }}" -c 8
```

Another example with a different route and json

```
cargo run --release http://localhost:8080/add-promocode "{\"name\": \"Promocode-test_add_promocode_with_weather\",\"advantage\": {\"percent\": 39}, \"restrictions\": [{\"weather\" : {\"is\": \"Clouds\",\"temp\": {\"gt\": -1000}}}]}"
```


# Usage

```
Usage : cargo run --release -- <URL> <JSON> [Options]


Arguments:
  <URL>
          Target URL
  <JSON>
          Post data to send

Options:
  -c, --concurrency <CONCURRENCY>
          Number of workers to run concurrently

          [default: 1]

  -n, --iterations <ITERATIONS>
          Number of iterations

          When set, benchmark stops after reaching the number of iterations.

  -d, --duration <DURATION>
          Duration to run the benchmark

          When set, benchmark stops after reaching the duration.

          Examples: -z 10s, -z 5m, -z 1h

  -r, --rate <RATE>
          Rate limit for benchmarking, in iterations per second (ips)

          When set, benchmark will try to run at the specified rate.

  -q, --quiet
          Run benchmark in quiet mode

          Implies --collector silent.

      --collector <COLLECTOR>
          Collector for the benchmark

          Possible values:
          - tui:    TUI based collector
          - silent: Collector that does not print anything

      --fps <FPS>
          Refresh rate for the tui collector, in frames per second (fps)

          [default: 32]

  -o, --output <OUTPUT>
          Output format for the report

          [default: text]

          Possible values:
          - text: Report in plain text format
          - json: Report in JSON format

  -h, --help
          Print help (see a summary with '-h')
```
