# waa-signer-build

Prebuilt binaries of `waa-signer`, the WAA proof signing service from
[DeconstructedCube/aistudio-api](https://github.com/DeconstructedCube/aistudio-api).
[GPROXY](https://github.com/LeenHawk/gproxy)'s AI Studio Playground channel
calls this service through its `proof_server_url` setting.

This repository contains no signer source. Its workflow checks out a fixed
upstream commit, builds it with the upstream build scripts and publishes the
result under [Releases](https://github.com/LeenHawk/waa-signer-build/releases).

## Download

| Platform | File |
| --- | --- |
| Linux x86-64 | `waa-signer-<version>-linux-amd64.tar.gz` |
| Linux ARM64 | `waa-signer-<version>-linux-arm64.tar.gz` |
| macOS Apple silicon | `waa-signer-<version>-darwin-arm64.tar.gz` |
| Windows x86-64 | `waa-signer-<version>-windows-amd64.zip` |

The Linux binaries are statically linked and do not depend on the system libc.
Each release also carries `SHA256SUMS`:

```sh
sha256sum -c SHA256SUMS --ignore-missing
```

## Run

```sh
tar -xzf waa-signer-<version>-linux-amd64.tar.gz
cd waa-signer-<version>-linux-amd64
./waa-signer -listen 127.0.0.1:8081 -proxy http://127.0.0.1:7890
curl http://127.0.0.1:8081/health
```

A running signer answers
`{"engine":"spidermonkey-152","ok":true,"status":"ok","workers":0}`.

| Flag | Environment variable | Default | Meaning |
| --- | --- | --- | --- |
| `-listen` | `LISTEN_ADDR` | `127.0.0.1:8081` | Listen address. If only `PORT` is set, the signer listens on that port on `0.0.0.0`. |
| `-proxy` | `AISTUDIO_PROXY`, `HTTPS_PROXY`, `HTTP_PROXY` | See meaning | Proxy used to reach Google, HTTP or SOCKS5. When the flag is absent the signer reads the environment variables, then probes local ports `7890` and `10808`, and connects directly if it finds no proxy. |
| `-state-dir` | `WAA_STATE_DIR` | `aistudio_waa_workers` under the system temporary directory | Cache directory. |
| `-timezone` | `AISTUDIO_TIMEZONE` | `America/Los_Angeles` | Time zone of the emulated browser. |
| `-locale` | `WAA_LOCALE` | `en-US` | Language of the emulated browser. |

The signer has no access control and receives Google session cookies and
request contents. Listen on a loopback or private address only.

## Which source is built

`upstream.env` names the upstream repository and commit, the SpiderMonkey
release, and the Go and Zig versions. The commit is the last one whose signer
serves HTTP (`/health`, `/v1/proof/mint`); the following upstream commit
replaced the HTTP server with a stdio IPC that GPROXY does not use.

`checksums.txt` holds the SHA-256 of the SpiderMonkey archives and Zig
toolchains the workflow downloads. A download that does not match fails the
build.

To publish a new build, edit `upstream.env` and `checksums.txt` as needed and
push a tag. A release is named `<upstream short commit>-<build number>`, for
example `fcdc53e-1`.

## Licenses

See [NOTICE.md](NOTICE.md). Each archive includes the notice, the license
texts and a `SOURCE.txt` with links to the exact sources.
