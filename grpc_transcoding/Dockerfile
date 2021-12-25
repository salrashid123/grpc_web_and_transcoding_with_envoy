from debian as build

RUN apt-get update -y && apt-get install -y build-essential wget unzip curl git


RUN curl -OL https://github.com/google/protobuf/releases/download/v3.19.1/protoc-3.19.1-linux-x86_64.zip && \
    unzip protoc-3.19.1-linux-x86_64.zip -d protoc3 && \
    mv protoc3/bin/* /usr/local/bin/ && \
    mv protoc3/include/* /usr/local/include/

ADD . /app
WORKDIR /app
RUN git clone https://github.com/googleapis/googleapis /tmp/googleapis
RUN /usr/local/bin/protoc -I ./echo   -I /tmp/googleapis  --include_imports --include_source_info   --descriptor_set_out=echo/echo.proto.pb     echo/echo.proto

FROM envoyproxy/envoy:v1.20.1
COPY --from=build /app/ /

EXPOSE 18080

WORKDIR /

ENTRYPOINT ["/usr/local/bin/envoy", "-c", "transcoding.yaml"]