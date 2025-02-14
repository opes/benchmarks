include ../common/commands.mk
.DEFAULT_GOAL := build

executables := \
	target/primes_cpp_gcc \
	target/primes_cpp_clang \
	target/primes_rs \
	target/primes_cr \
	target/primes_zig

artifacts := $(executables) \
	target/primes_scala.jar \
	target/Primes.class

all_runners := $(patsubst %,run[%], $(artifacts)) \
	run[pypy][primes.py] \
	run[primes.py] \
	run[primes.js] \
	run[primes.rb] \
	run[jit][primes.rb] \
	run[truby-jvm][primes.rb] \
	run[truby-native][primes.rb] \
	run[jruby][primes.rb] \
	run[primes.lua] \
	run[primes_jit.lua]


# Build

.PHONY: build
build: $(artifacts)

target/primes_scala.jar: primes.scala | target
	$(SCALAC_BUILD)

target/Primes.class: Primes.java | target
	$(JAVAC_BUILD)

target/primes_rs: primes.rs | $(rs_fmt)
	$(RUST_CLIPPY)
	$(RUSTC_BUILD)

target/primes_cpp_gcc: primes.cpp | target libnotify
	$(GCC_CPP_BUILD)

target/primes_cpp_clang: primes.cpp | target libnotify
	$(CLANG_CPP_BUILD)

target/primes_cr: primes.cr | target
	$(CRYSTAL_BUILD)

target/primes_zig: primes.zig | target
	$(ZIG_BUILD)

# Run

.PHONY: run
run: $(all_runners)

## Common recipe for all runners
.PHONY: $(all_runners)
$(all_runners)::
	$(ECHO_RUN)

## Runners
executable_runners := $(patsubst %,run[%], $(executables))
$(executable_runners):: run[%] : %
	$(EXECUTABLE_RUN)

run[target/primes_scala.jar]:: run[%]: %
	$(SCALA_RUN) Primes

run[target/Primes.class]:: run[%]: %
	$(JAVA_CLASS_RUN)

run[pypy][primes.py]:: run[pypy][%]: % | $(py_fmt)
	$(PYPY3_RUN)

run[primes.py]:: run[%]: % | $(py_fmt)
	$(PYTHON3_RUN)

run[primes.js]:: run[%]: %
	$(NODE_RUN)

run[primes.rb]:: run[%]: % | $(rubocop)
	$(RUBY_RUN)

run[jit][primes.rb]:: run[jit][%]: % | $(rubocop)
	$(RUBY_JIT_RUN)

run[truby-jvm][primes.rb]:: run[truby-jvm][%]: % | $(rubocop)
	$(TRUBY_JVM_RUN)

run[truby-native][primes.rb]:: run[truby-native][%]: % | $(rubocop)
	$(TRUBY_NATIVE_RUN)

run[jruby][primes.rb]:: run[jruby][%]: % | $(rubocop)
	$(JRUBY_RUN)

run[primes.lua]:: run[%]: % | libnotify
	$(LUA_RUN)

run[primes_jit.lua]:: run[%]: %
	$(LUA_JIT_RUN)

# Utilities

.PHONY: clean
clean:
	-rm -rf target
	-rm -rf zig-cache
