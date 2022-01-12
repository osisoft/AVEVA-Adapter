FROM ubuntu
WORKDIR /
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y ca-certificates libicu60 libssl1.1 curl
COPY <script_name.sh> /
RUN chmod +x /<script_name.sh>
ADD ./<installer_name>_<symantec_version>-arm64_.tar.gz .
ENTRYPOINT ["/<script_name.sh>"]