# Copyright 2016 Google Inc. All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
################################################################################
#

load(
    "//:repositories.bzl",
    "boringssl_repositories",
    "protobuf_repositories",
    "googletest_repositories",
)

boringssl_repositories()

protobuf_repositories()

googletest_repositories()

load(
    "//contrib/endpoints:repositories.bzl",
    "grpc_repositories",
    "servicecontrol_client_repositories",
)

grpc_repositories()

servicecontrol_client_repositories()

load(
    "//src/envoy/mixer:repositories.bzl",
    "mixer_client_repositories",
)

mixer_client_repositories()

# Workaround for Bazel > 0.4.0 since it needs newer protobuf.bzl from:
# https://github.com/google/protobuf/pull/2246
# Do not use this git_repository for anything else than protobuf.bzl
new_git_repository(
    name = "protobuf_bzl",
    # Injecting an empty BUILD file to prevent using any build target
    build_file_content = "",
    commit = "05090726144b6e632c50f47720ff51049bfcbef6",
    remote = "https://github.com/google/protobuf.git",
)

load(
    "@mixerclient_git//:repositories.bzl",
    "mixerapi_repositories",
)

mixerapi_repositories(protobuf_repo="@protobuf_bzl//")

load(
    "//src/envoy:repositories.bzl",
    "envoy_repositories",
)

envoy_repositories()

new_http_archive(
    name = "docker_ubuntu",
    build_file_content = """
load("@bazel_tools//tools/build_defs/docker:docker.bzl", "docker_build")
docker_build(
  name = "xenial",
  tars = ["xenial/ubuntu-xenial-core-cloudimg-amd64-root.tar.gz"],
  visibility = ["//visibility:public"],
)
""",
    sha256 = "de31e6fcb843068965de5945c11a6f86399be5e4208c7299fb7311634fb41943",
    strip_prefix = "docker-brew-ubuntu-core-e406914e5f648003dfe8329b512c30c9ad0d2f9c",
    type = "zip",
    url = "https://codeload.github.com/tianon/docker-brew-ubuntu-core/zip/e406914e5f648003dfe8329b512c30c9ad0d2f9c",
)


DEBUG_BASE_IMAGE_SHA="3f57ae2aceef79e4000fb07ec850bbf4bce811e6f81dc8cfd970e16cdf33e622"

# See github.com/istio/manager/blob/master/docker/debug/build-and-publish-debug-image.sh
# for instructions on how to re-build and publish this base image layer.
http_file(
    name = "ubuntu_xenial_debug",
    url = "https://storage.googleapis.com/istio-build/manager/ubuntu_xenial_debug-" + DEBUG_BASE_IMAGE_SHA + ".tar.gz",
    sha256 = DEBUG_BASE_IMAGE_SHA,
)
