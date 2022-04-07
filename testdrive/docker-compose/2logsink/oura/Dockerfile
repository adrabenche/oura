FROM --platform=linux/amd64 rust:1-buster as builder

ARG ARG_GITREV

RUN git config --global init.defaultBranch main &&  \
    git config --global advice.detachedHead false && \
    git init && \
    git remote add origin https://github.com/txpipe/oura.git && \
    git fetch  && \
    test -z ${ARG_GITREV} &&  gitrev=$(git describe --tags $(git rev-list --tags --max-count=1)) || gitrev=${ARG_GITREV}  && \
    git checkout "$gitrev"

RUN cargo build --release --target x86_64-unknown-linux-gnu --all-features && \
    cp /target/x86_64-unknown-linux-gnu/release/oura /oura

# Run app
FROM debian:buster-slim

COPY --from=builder /oura /usr/local/bin/oura

ENTRYPOINT [ "oura" ]
