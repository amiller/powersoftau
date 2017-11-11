FROM ubuntu:trusty@sha256:f6eed4def93a3b54da920737f0abf1a8cae2e480bb368280c898265fcaf910a3
RUN apt-get update --fix-missing
RUN apt-get install -yq git
RUN apt-get install -yq curl
RUN apt-get install -yq build-essential
RUN apt-get install -yq musl-tools
# install toolchain
RUN curl https://sh.rustup.rs -sSf > rustup.sh && sha256sum rustup.sh \
     | awk '{print $1}' \
     |xargs -I{} test "22aa1f7f4c4b9be99a9d7e13ad45b2aec6714165a0578dd5ef81ca11f55ea24e" = {} \
     && cat rustup.sh \
     | sh -s -- --default-toolchain nightly-2017-11-08 -y

RUN git clone https://github.com/ebfull/powersoftau/
WORKDIR powersoftau
RUN git checkout 9e1553c437183540392a7231d0788318a19b18a3

ENV PATH=/root/.cargo/bin:$PATH

RUN rustup target add x86_64-unknown-linux-musl
RUN cargo build --target=x86_64-unknown-linux-musl --release --features=u128-support --bin=compute
RUN sha256sum target/x86_64-unknown-linux-musl/release/compute
CMD bash