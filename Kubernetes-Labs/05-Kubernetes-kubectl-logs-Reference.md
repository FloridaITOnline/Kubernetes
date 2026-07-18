# Kubernetes Logging with `kubectl logs`

> A practical reference guide for troubleshooting Kubernetes workloads using `kubectl logs`.

---

# Kubernetes Logging with `kubectl logs`

## Overview

The `kubectl logs` command retrieves logs from containers running in Kubernetes Pods. It can be be used to:

- Troubleshoot application errors
- Monitor applications in real time
- Investigate crashed containers
- Search logs for specific events
- Retrieve logs from multiple Pods

This guide demonstrates the most commonly used options with practical examples.

---

## Basic Syntax

```bash
kubectl logs <pod-name>
```

Example:

```bash
kubectl logs bunnies
```

Retrieves logs from the single container inside the **bunnies** Pod.

---

## Specify a Container

```bash
kubectl logs bunnies -c carrots
```

Example output:

```text
Starting carrot service...
Waiting for bunny requests...
Pick me!
Pick me!
Pick me!
Carrot selected successfully.
```

---

## Follow Logs in Real Time

```bash
kubectl logs -f bunnies -c carrots
```

Press **Ctrl+C** to stop following the logs.

---

## View Previous Container Logs

```bash
kubectl logs -p bunnies -c carrots
```

Useful when troubleshooting container crashes.

---

## View Only Recent Logs

```bash
kubectl logs --since=1h bunnies
kubectl logs --since=15m bunnies
```

---

## Show Only the Last Few Lines

```bash
kubectl logs --tail=20 bunnies
kubectl logs --tail=100 bunnies
```

---

## Include Timestamps

```bash
kubectl logs bunnies --timestamps
```

Example:

```text
2026-07-18T13:55:20Z Pick me!
2026-07-18T13:55:22Z Pick me!
2026-07-18T13:55:23Z Pick me!
```

---

## View Logs from Every Container

```bash
kubectl logs bunnies --all-containers=true
```

---

## View Logs from Multiple Pods

```text
app=bunnies
```

```bash
kubectl logs -l app=bunnies --all-containers=true
```

---

## Prefix Each Log Entry

```bash
kubectl logs -l app=bunnies --all-containers=true --prefix
```

Example:

```text
bunnies-5f8d carrots Pick me!
bunnies-7c2f carrots Pick me!
bunnies-91aa carrots Pick me!
```

---

## Search the Logs

```bash
kubectl logs bunnies -c carrots | grep "Pick me!"
```

```bash
kubectl logs bunnies -c carrots | grep -i "pick me"
```

```bash
kubectl logs bunnies -c carrots | grep -Ei "pick me|selected|warning"
```

```bash
kubectl logs bunnies -c carrots | grep -C 2 "Pick me!"
```

---

## Limit the Number of Bytes

```bash
kubectl logs --limit-bytes=1024 bunnies
```

---

## View Logs After a Specific Time

```bash
kubectl logs --since-time="2026-07-18T12:00:00Z" bunnies
```

---

## Stream Logs from Multiple Pods

```bash
kubectl logs -f -l app=bunnies --all-containers=true
```

---

# Common Troubleshooting Workflow

```bash
kubectl get pods
kubectl describe pod bunnies
kubectl logs bunnies -c carrots
kubectl logs bunnies -c carrots | grep -Ei "error|warning|failed"
kubectl logs -p bunnies -c carrots
kubectl logs -f bunnies -c carrots
```

---

## Tips

- Single container Pods do not require `-c`.
- Use `--follow` to watch logs live.
- Use `--tail` or `--since` to reduce output.
- Use `grep` for fast searches.
- Use `--previous` after container restarts.
- Use label selectors to aggregate logs across Pods.

---

## Common Options

| Option | Description |
|---------|-------------|
| `-c` | Select a container |
| `-f` | Follow logs |
| `-p` | Previous container logs |
| `--tail` | Last N lines |
| `--since` | Recent logs |
| `--since-time` | Logs after a timestamp |
| `--timestamps` | Include timestamps |
| `--all-containers` | Show all containers |
| `-l` | Select Pods by label |
| `--prefix` | Prefix each log line |

---

## Author

**Justin Rodriguez**  

---

## License

This project is licensed under the **GNU General Public License v3.0 (GPL-3.0)**.

See the `LICENSE` file for the full license text.

