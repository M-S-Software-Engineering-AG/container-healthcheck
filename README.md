# Container Healthcheck

A tiny static binary that performs an HTTP `GET` and exits `0` on a `200`,
`1` on anything else (non-`200`, connection error, missing URL). It lets
shell-less (distroless, scratch) images use a binary-form `HEALTHCHECK`.

## Usage

```
container-healthcheck URL
```

Prebuilt static binaries are published on the releases page for Linux, macOS
and Windows (amd64, arm64; Linux also armv7), named
`container-healthcheck-<os>-<arch>`.

```dockerfile
FROM gcr.io/distroless/static-debian13:nonroot
ARG TARGETARCH
ARG TARGETVARIANT
ARG HEALTHCHECK_VERSION
ADD --chmod=755 https://github.com/M-S-Software-Engineering-AG/container-healthcheck/releases/download/${HEALTHCHECK_VERSION}/container-healthcheck-linux-${TARGETARCH}${TARGETVARIANT} /usr/local/bin/healthcheck
HEALTHCHECK CMD ["/usr/local/bin/healthcheck", "http://localhost:8080/health"]
```

## Releases

Pushes to `release/<major>.<minor>` are tagged automatically
(`v<major>.<minor>.<patch>`); each tag builds, Trivy-scans and publishes the
binaries via the shared
[ci-workflows](https://github.com/M-S-Software-Engineering-AG/ci-workflows).
