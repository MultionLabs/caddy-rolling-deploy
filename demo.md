# caddy-rolling-deploy

**1. Enable in your Caddyfile**

```caddyfile
deploy.example.com {
	rolling_deploy {
		secret {$ROLLING_DEPLOY_SECRET}
		docker_hosts unix:///var/run/docker.sock
		# or: tcp://host-a:2375 tcp://host-b:2375
	}
}
```

**2. Trigger a rolling deploy** (e.g. from CI after pushing an image)

```bash
curl -si \
  "https://deploy.example.com/webhooks/rolling-deploy/${ROLLING_DEPLOY_SECRET}/api/ghcr.io/acme/api:1.2.3"
```

Updates container `api` to image `ghcr.io/acme/api:1.2.3` on each configured Docker host — one at a time, with rollback on failure.
