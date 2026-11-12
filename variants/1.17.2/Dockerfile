FROM alpine:3.24
ARG TARGETPLATFORM
ARG BUILDPLATFORM
RUN echo "I am running on $BUILDPLATFORM, building for $TARGETPLATFORM"

RUN apk add --no-cache ca-certificates

# Disable terraform checkpoints. See: https://developer.hashicorp.com/terraform/cli/commands#upgrade-and-security-bulletin-checks
ENV CHECKPOINT_DISABLE=1

# Install terraform
RUN set -eux; \
    TERRAFORM_VERSION=1.17.2; \
    case "$( uname -m )" in \
        'x86') \
            URL="https://releases.hashicorp.com/terraform/1.17.2/terraform_1.17.2_linux_386.zip"; \
            SHA256=382939c9d2ded54649e1e062501fcaa1c2076a70bf4632d4838748a039b6c5e9; \
            ;; \
        'x86_64') \
            URL="https://releases.hashicorp.com/terraform/1.17.2/terraform_1.17.2_linux_amd64.zip"; \
            SHA256=e6f16b2f0b137ee5729caa8c4e79a3bae4ee261acd198a1fcfa8584a6296605b; \
            ;; \
        'armhf') \
            URL="https://releases.hashicorp.com/terraform/1.17.2/terraform_1.17.2_linux_arm.zip"; \
            SHA256=cd4dfed6a2c310f736e33b9e2f3ddcd86c81b392e5d31b51cb55462e6f19e452; \
            ;; \
        'armv7l') \
            URL="https://releases.hashicorp.com/terraform/1.17.2/terraform_1.17.2_linux_arm.zip"; \
            SHA256=cd4dfed6a2c310f736e33b9e2f3ddcd86c81b392e5d31b51cb55462e6f19e452; \
            ;; \
        'aarch64') \
            URL="https://releases.hashicorp.com/terraform/1.17.2/terraform_1.17.2_linux_arm64.zip"; \
            SHA256=858e08c181a76b6b223e8c767a8e983922eebe2d062db949a2de66ce040c368c; \
            ;; \
        's390x') \
            URL="https://releases.hashicorp.com/terraform/1.17.2/terraform_1.17.2_linux_s390x.zip"; \
            SHA256=435bdac1694936aaeed4a23394fecb3f0ebfc6a03184cb51bdb787f81be7e308; \
            ;; \
        *) \
            echo "Architecture not supported"; \
            exit 1; \
            ;; \
    esac; \
    FILE=terraform.zip; \
    wget -q "$URL" -O "$FILE"; \
    echo "$SHA256  $FILE" | sha256sum -c -; \
    unzip "$FILE" -d extract; \
    mkdir -pv /usr/local/bin; \
    BIN=$( find extract -type f -name "terraform" | head -n1 ); \
    mv -v "$BIN" /usr/local/bin/terraform; \
    chmod +x /usr/local/bin/terraform; \
    CHECKPOINT_DISABLE=1 terraform version; \
    rm -rf extract; \
    rm -f "$FILE"; \
    :

CMD [ "terraform" ]
