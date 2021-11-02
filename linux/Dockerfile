FROM ubuntu:20.04

# Fixed rust version to 1.56
ARG RUST_VER=1.56.0
# Fixed rust version to 12
ARG LLVM_VER=12

ENV RUSTUP_HOME=/usr/local/rustup \
    CARGO_HOME=/usr/local/cargo \
    PATH=/usr/local/cargo/bin:$PATH \
    RUST_VERSION=$RUST_VER

# Avoid warnings by switching to noninteractive
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
		&& apt-get install \
			wget gnupg \
			build-essential \
			libz-dev \
			-y

# Setup llvm sources
RUN echo "deb http://apt.llvm.org/focal/ llvm-toolchain-focal-$LLVM_VER  main" >> /etc/apt/sources.list.d/llvm.list
RUN wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key | apt-key add -

RUN apt-get update
#Install Clang dependencies
RUN apt-get install -y zip clang-$LLVM_VER lldb-$LLVM_VER lld-$LLVM_VER clangd-$LLVM_VER liblld-$LLVM_VER-dev llvm-$LLVM_VER-dev

#Install rust stable and nightly
ADD https://sh.rustup.rs /temp/rustup-init.sh
RUN chmod +x /temp/rustup-init.sh \
		&& /temp/rustup-init.sh \
			--default-toolchain $RUST_VERSION \
			-y --no-modify-path

RUN rustup toolchain install nightly \
		&& rustup component add llvm-tools-preview  \
		&& rustup component add --toolchain nightly llvm-tools-preview  \
	 	#Install documentation and coverage tools \
		&& cargo install mdbook 
		&& cargo install --frozen grcov


WORKDIR /build
ENTRYPOINT ["bash"]

# Switch back to dialog for any ad-hoc use of apt-get
ENV DEBIAN_FRONTEND=dialog
