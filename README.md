# @metrics/prometheus v0.1.0

Prometheus metrics for Zeta — a port of the Rust `prometheus` crate.

## Exports

### Counter
```zeta
let c = Counter::new("requests_total", "Total HTTP requests");
c.inc();
c.inc_by(5.0);
let val: f64 = c.get();
```

### Gauge
```zeta
let g = Gauge::new("temperature", "Current temperature");
g.set(23.5);
g.inc();
g.dec();
g.add(2.0);
g.sub(1.0);
```

### Histogram
```zeta
let h = Histogram::new("latency_seconds", "Request latency", default_buckets());
h.observe(0.042);
```

Default buckets: .005, .01, .025, .05, .1, .25, .5, 1, 2.5, 5, 10

### Registry
```zeta
let reg = Registry::new();
reg.register_counter(c);
reg.register_gauge(g);
reg.register_histogram(h);
let families = reg.gather();
```

### Text Encoding
```zeta
let text: str = encode_text(families);
```

### FNV-1a Hash
```zeta
let hash: u64 = fnv1a_hash(data);
```

## Text Format (Prometheus Exposition)

```
# HELP requests_total Total HTTP requests
# TYPE requests_total counter
requests_total 7

# HELP latency_seconds Request latency
# TYPE latency_seconds histogram
latency_seconds_bucket{le="0.005"} 0
latency_seconds_bucket{le="0.01"} 0
...
latency_seconds_bucket{le="10"} 42
latency_seconds_count 42
latency_seconds_sum 3.14
```

## Build & Test

```bash
zeta build
zeta test
```
