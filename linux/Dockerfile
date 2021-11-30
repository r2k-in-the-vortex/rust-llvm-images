FROM rust:1.56.1-bullseye

# Fixed rust version to 1.56
ARG RUST_VER=1.56.1
# Fixed rust version to 12
ARG LLVM_VER=12

# Avoid warnings by switching to noninteractive
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
		&& apt-get install \
			wget gnupg \
			build-essential \
			-y

# Setup llvm sources
RUN echo "deb http://apt.llvm.org/bullseye/ llvm-toolchain-bullseye-$LLVM_VER  main" >> /etc/apt/sources.list.d/llvm.list
RUN wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key | apt-key add -

RUN apt-get update
#Install Clang dependencies
RUN apt-get install -y zip clang-$LLVM_VER lldb-$LLVM_VER lld-$LLVM_VER clangd-$LLVM_VER liblld-$LLVM_VER-dev llvm-$LLVM_VER-dev

RUN rustup toolchain install nightly \
		&& rustup component add clippy rustfmt llvm-tools-preview  \
		&& rustup component add --toolchain nightly llvm-tools-preview  \
	 	#Install documentation and coverage tools \
		&& cargo install mdbook grcov

WORKDIR /build
ENTRYPOINT ["bash"]

# Switch back to dialog for any ad-hoc use of apt-get
ENV DEBIAN_FRONTEND=dialog
