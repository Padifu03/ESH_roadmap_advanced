FROM debian:bookworm-slim

LABEL org.opencontainers.image.description="Bare-metal ARM cross-compilation environment for STM32C031C6"
LABEL org.opencontainers.image.authors="Padifu03 <jfpadifu03@gmail.com>"

RUN apt-get update && apt-get install -y --no-install-recommends \
    gcc-arm-none-eabi \
    gdb-multiarch \
    binutils-arm-none-eabi \
    make \
    openocd && \
    rm -rf /var/lib/apt/lists/*

ARG UID=1000
RUN useradd --uid ${UID} --create-home dev

USER dev
WORKDIR /workspace

CMD ["echo", "STM32 build environment ready. Usage: docker run --rm -v <project_path>:/workspace <image> make all or similar commands."]
