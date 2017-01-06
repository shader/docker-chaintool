# Chaintool via Docker

A container that provides a pre-built chaintool, as well as some additional tools

## What's Included

This container uses the latest version of hyperledger/fabric-chaintool as its entrypoint

Since this container is based off of znly/protoc, it also contains the following:

-   protobuf 3.1.0
-   gRPC 1.0.1
-   Google Well Known Types are automatically included (via google/)
-   Go related tools compiled with 1.7, gRPC support is built-in:
    -   github.com/golang/protobuf/protoc-gen-go
    -   github.com/gogo/protobuf/protoc-gen-gofast
    -   github.com/gogo/protobuf/protoc-gen-gogo
    -   github.com/gogo/protobuf/protoc-gen-gogofast
    -   github.com/gogo/protobuf/protoc-gen-gogofaster
    -   github.com/gogo/protobuf/protoc-gen-gogoslick
    -   github.com/grpc-ecosystem/grpc-gateway/protoc-gen-swagger
    -   github.com/grpc-ecosystem/grpc-gateway/protoc-gen-grpc-gateway

## Usage

    $ docker run --rm chaintool
    
    chaintool version: v0.10.1-SNAPSHOT
    
    Usage: chaintool [general-options] action [action-options]
    
    General Options:
      -v, --version  Print the version and exit
      -h, --help
    
    Actions:
      build -> Build the chaincode project
      buildcar -> Build the chaincode project from a CAR file
      clean -> Clean the chaincode project
      package -> Package the chaincode into a CAR file for deployment
      unpack -> Unpackage a CAR file
      ls -> List the contents of a CAR file
      proto -> Compiles a CCI file to a .proto
      inspect -> Retrieves metadata from a running instance

Also, to actually run most of the commands (such as build) you will need to mount your local files.

The default working directory is \`/src\`, and /dest is also provided for output files. You can also just specify the working directory via command line options.

    # using defaults
    $ docker run --rm -v $(pwd):/src chaintool build
    
    # manually specifying working directory
    $ docker run --rm -v $(pwd):$(pwd) -w $(pwd) chaintool build

To use protoc directly, either use the znly/protoc container, or specify an alternate endpoint:

    $ docker run --rm --entrypoint protoc -v $(pwd):/src shcv/chaintool --python_out=. -I. myfile.proto

## Image Size

The current image size appears to be about MB.
The breakdown appears to be approximately:

```
| Source                 | Size (MB) |
|------------------------+-----------|
| znly/protoc            |       200 |
| openjdk                |       140 |
| chaintool + clojure    |       130 |
| golang + 'go get' deps |       190 |
|------------------------+-----------|
| Total                  |       660 |
```