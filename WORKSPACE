# -*- python -*-

# Copyright 2018-2020 Josh Pieper, jjp@pobox.com.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

workspace(name = "com_github_mjbots_moteus",
          managed_directories = {
              "@npm": ["node_modules"],
          })

BAZEL_VERSION = "3.4.1"
BAZEL_VERSION_SHA = "1a64c807716e10c872f1618852d95f4893d81667fe6e691ef696489103c9b460"

load("//tools/workspace:default.bzl", "add_default_repositories")

add_default_repositories()

# Do the Javascript part of our initialization.
load("//tools/workspace:npm_stage1.bzl", "setup_npm_stage1")
setup_npm_stage1()

load("//tools/workspace:npm_stage2.bzl", "setup_npm_stage2")
setup_npm_stage2()

load("//tools/workspace:npm_stage3.bzl", "setup_npm_stage3")
setup_npm_stage3()


# Now bazel-toolchain
load("@com_github_mjbots_bazel_toolchain//toolchain:deps.bzl", "bazel_toolchain_dependencies")
bazel_toolchain_dependencies()

load("@com_github_mjbots_bazel_toolchain//toolchain:rules.bzl", "llvm_toolchain")
llvm_toolchain(
    name = "llvm_toolchain",
    llvm_version = "10.0.0",
    urls = {
        "windows" : ["https://github.com/mjbots/bazel-toolchain/releases/download/0.5.6-mj20201011/LLVM-10.0.0-win64.tar.xz"],
    },
    sha256 = {
        "windows" : "2851441d3993c032f98124a05e2aeb43010b7a85f0f7441103d36ae8d00afc18",
    },
    strip_prefix = {
        "windows" : "LLVM",
    }
)

load("@llvm_toolchain//:toolchains.bzl", "llvm_register_toolchains")
llvm_register_toolchains()



# Now rules_mbed
load("@com_github_mjbots_rules_mbed//:rules.bzl", mbed_register = "mbed_register")
load("@com_github_mjbots_rules_mbed//tools/workspace/mbed:repository.bzl", "mbed_repository")

mbed_register(
    # Pick a random target to test against.
    config = {
        "mbed_target": "targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F446xE/TARGET_NUCLEO_F446ZE",
        "mbed_config": {
        },
    }
)

# And finally, bazel_deps
load("@com_github_mjbots_bazel_deps//tools/workspace:default.bzl",
     bazel_deps_add = "add_default_repositories")
bazel_deps_add()
