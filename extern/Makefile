
ifeq ($(NNG_WITH_MBEDTLS),ON)
	DOMBEDTLS=mk-mbedtls
	NNGFLAGS=-D NNG_ENABLE_TLS=ON -D MBEDTLS_ROOT=$(abspath mbedtls/build/target) -D MBEDTLS_ROOT_DIR=$(abspath mbedtls/build/target) -D FORCE_MBED_OK=TRUE  -D CMAKE_FIND_PACKAGE_PREFER_CONFIG=TRUE  -D CMAKE_INSTALL_PREFIX:PATH=.
else
	DOMBEDTLS=
	NNGFLAGS=-D CMAKE_INSTALL_PREFIX:PATH=.
endif

.ONESHELL:

all: $(DOMBEDTLS) mk-nng

clean: clean-mbedtls clean-nng

mk-mbedtls:
	mkdir -p mbedtls/build/target
	cd mbedtls/build
	cmake -D CMAKE_INSTALL_PREFIX:PATH=target ..
	cmake --build .
	cmake --install .
mk-nng:
	mkdir -p nng/build
	cd nng
	patch -N -p2 < ../_patch/nng.cmake.mbedtls.patch
	cd build
	cmake $(NNGFLAGS) ..
	cmake --build . --target install --config Release

clean-nng:
	cd nng
	git checkout cmake/FindMbedTLS.cmake
	cd ..
	rm -rf nng/build

clean-mbedtls:
	rm -rf mbedtls/build

.PHONY: mk-mbedtls mk-nng

