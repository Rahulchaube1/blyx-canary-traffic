# Canary Traffic

![Package logo](https://github.com/Rahulchaube1/blyx-canary-traffic/raw/v0.1.1/editor/icon.png)

Gate ordered traffic increases on the health of the current rollout percentage.

Reliability and delivery · framework · 0.1.1

## Drive the workflow

Start with `canary_traffic_start()`. For each event call `canary_traffic_step(snapshot, event, value)`, persist the returned snapshot, and dispatch `canary_traffic_command(snapshot)` to your application’s adapter. The host owns I/O, event ordering, duplicate detection and durable command delivery. A matching event produces one command. Rejected events retain the phase and saved quantity. An invalid snapshot yields an empty vector.

The snapshot contains phase ID, saved quantity and last command ID. Event values are whole numbers from zero through one billion. In the table, **x** is the event value and **r** is the saved quantity. Rules match in listed order. To validate a whole history without running effects use `canary_traffic_replay(events, values)`. It returns the final snapshot or an empty vector on rejection.

| Current phase | Event | Guard | Next phase | Command | New quantity |
|---|---|---|---|---|---|
| idle | launch | x>0 and x<=100 | probing | apply_weight | x |
| probing | healthy | x=r | ready | authorize_ramp | r |
| probing | unhealthy | always | rolledback | route_baseline | 0 |
| ready | ramp | x>r and x<=100 | probing | apply_weight | x |
| ready | finish | r=100 | done | retire_baseline | r |

Phase IDs in order from zero: idle, probing, ready, rolledback, done.

## Replay your own events

After installing the demo, run the following from its directory. Supply alternating event names and integer values. The command prints each requested action and exits with status 1 at the first rejected transition, or 2 for malformed arguments.

```sh
blyxc native-run main.blyx -- launch 5 healthy 5 ramp 100 healthy 100 finish 0
```

## Run this checkout

Run `blyxc native-run main.blyx` to reproduce `expected.txt`. Run `blyxc native-run checks.blyx` and compare the complete output with `checks-expected.txt`.

## Try the published project

```sh
nest demo canary_traffic@0.1.1
blyxc native-run canary_traffic-demo/main.blyx
```

Install with `nest add canary_traffic@0.1.1` and import `vendor::canary_traffic::canary_traffic`.

Maintained by Rahul Chaube. [Blyx](https://www.blyx-lang.space) · [GitHub](https://github.com/Rahulchaube1) · [LinkedIn](https://www.linkedin.com/in/rahulchaube1) · [Sponsor](https://github.com/sponsors/Rahulchaube1)


## Editor snippets

Use this extension with the Blyx Language extension. In a .blyx file, type one of the following prefixes and invoke editor completion.

- `canary_traffic_start`
- `canary_traffic_step`
- `canary_traffic_command`
- `canary_traffic_replay`
- `use_canary_traffic`
