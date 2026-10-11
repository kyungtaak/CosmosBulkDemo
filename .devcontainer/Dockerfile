FROM mcr.microsoft.com/devcontainers/base:bookworm

ARG GITLEAKS_VERSION=8.30.1

RUN set -eu; \
    architecture="$(dpkg --print-architecture)"; \
    case "$architecture" in \
        amd64) architecture=x64 ;; \
        arm64) architecture=arm64 ;; \
        *) echo "Unsupported gitleaks architecture: $architecture" >&2; exit 1 ;; \
    esac; \
    archive="gitleaks_${GITLEAKS_VERSION}_linux_${architecture}.tar.gz"; \
    release="https://github.com/gitleaks/gitleaks/releases/download/v${GITLEAKS_VERSION}"; \
    mkdir /tmp/gitleaks-download; \
    cd /tmp/gitleaks-download; \
    curl --fail --show-error --location --retry 3 "$release/$archive" --output "$archive"; \
    curl --fail --show-error --location --retry 3 "$release/gitleaks_${GITLEAKS_VERSION}_checksums.txt" --output checksums.txt; \
    grep -F "  $archive" checksums.txt | sha256sum --check --status -; \
    tar -xzf "$archive" -C /usr/local/bin gitleaks; \
    mkdir -p /usr/local/share/doc/gitleaks; \
    tar -xzf "$archive" -C /usr/local/share/doc/gitleaks LICENSE; \
    chmod 755 /usr/local/bin/gitleaks; \
    gitleaks version; \
    rm "$archive" checksums.txt; \
    cd /; \
    rmdir /tmp/gitleaks-download
