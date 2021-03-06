package(default_visibility = ["//visibility:public"])

load("@org_pubref_rules_protobuf//cpp:rules.bzl", "cc_proto_library")
load("@org_pubref_rules_protobuf//cpp:rules.bzl", "cc_proto_compile")
load("@org_pubref_rules_protobuf//java:rules.bzl", "java_proto_compile")

cc_binary(
    name = "words",
    srcs = ["words.cpp"],
    data = ["//data:model"],
    deps = [
        ":knearest",
        ":recogniser",
        ":scrabble",
        "@opencv//:opencv_imgcodecs",
    ],
)

cc_library(
    name = "recogniser",
    srcs = ["recogniser.cpp"],
    hdrs = ["recogniser.h"],
    deps = [
        ":knearest",
    ],
)

cc_test(
    name = "recogniser_test",
    srcs = ["recogniser_test.cpp"],
    data = [
        "testdata/words2.png",
        "//data:model",
    ],
    deps = [
        ":knearest",
        ":recogniser",
        "@gtest//:gtest",
        "@opencv//:opencv_imgcodecs",
    ],
)

cc_library(
    name = "scrabble",
    srcs = [
        "scrabble.cpp",
        "scrabble.h",
    ],
    copts = [
        "--std=c++11",
    ],
    deps = [
        "@dawgdic//:dawgdic",
    ],
)

cc_library(
    name = "knearest",
    srcs = [
        "knearest.cpp",
        "knearest.h",
    ],
    copts = [
        "--std=c++11",
    ],
    deps = [
        "@opencv//:opencv_core",
        "@opencv//:opencv_imgproc",
        "@opencv//:opencv_ml",
    ],
)

cc_binary(
    name = "train_model",
    srcs = ["train_model.cpp"],
    deps = [
        ":knearest",
        "//external:gflags",
        "@opencv//:opencv_highgui",
    ],
)

cc_binary(
    name = "server",
    srcs = ["server.cpp"],
    data = ["//data:model"],
    deps = [
        ":knearest",
        ":recogniser",
        ":scrabble",
        ":service",
        "//external:gflags",
        "@opencv//:opencv_imgcodecs",
    ],
)

cc_binary(
    name = "client",
    srcs = ["client.cpp"],
    deps = [
        ":service",
        "//external:gflags",
    ],
)

cc_proto_library(
    name = "service",
    protos = [
        "messages.proto",
        "service.proto",
    ],
    with_grpc = True,
)

cc_proto_compile(
    name = "messages",
    protos = ["messages.proto"],
    with_grpc = False,
)

cc_library(
    name = "messages_lib",
    srcs = [":messages"],
    deps = [
        "//external:protobuf_clib",
    ],
)

java_proto_compile(
    name = "service_java",
    protos = [
        "messages.proto",
        "service.proto",
    ],
    visibility = ["//visibility:public"],
    with_grpc = True,
)
