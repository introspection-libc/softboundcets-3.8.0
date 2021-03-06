all: softboundcets_rt softboundmpx_rt softboundcetsmpx_rt

SBCETS_MPX_FLAGS=-Wall -pedantic -O3 -D__SOFTBOUNDCETSMPX_SPATIAL_TEMPORAL

MPX_FLAGS=-Wall -pedantic -O3 -D__SOFTBOUNDMPX_SPATIAL 
CFLAGS=-Wall -pedantic  -O3 -D__SOFTBOUNDCETS_SPATIAL_TEMPORAL
ARFLAGS=-rcs

# If LLVM_GOLD is set, also build a library for use with LTO
#
# Note that the name of the library is hardcoded in the compiler. Thus, we call
# it the same, but put it in the lto/ subdirectory. To use it, pass
# -L/path/to/softboundcets-lib/lto to the compiler, and use -flto during
# compilation and linking
ifneq ($(LLVM_GOLD),)
all: softboundcets_rt_lto softboundmpx_rt_lto softboundcetsmpx_rt_lto
endif


softboundcetsmpx_rt: softboundcetsmpx.h softboundcetsmpx-checks.c softboundcetsmpx.c softboundcetsmpx-wrappers.c
	clang $(SBCETS_MPX_FLAGS) -c softboundcetsmpx-checks.c -o softboundcetsmpx-checks.o
	clang $(SBCETS_MPX_FLAGS) -c softboundcetsmpx.c -o softboundcetsmpx.o
	clang $(SBCETS_MPX_FLAGS) -c softboundcetsmpx-wrappers.c -o softboundcetsmpx-wrappers.o
	ar $(ARFLAGS) libsoftboundcetsmpx_rt.a softboundcetsmpx.o softboundcetsmpx-checks.o softboundcetsmpx-wrappers.o

softboundmpx_rt: softboundmpx.h softboundmpx-checks.c softboundmpx.c softboundmpx-wrappers.c
	clang $(MPX_FLAGS) -c softboundmpx-checks.c -o softboundmpx-checks.o
	clang $(MPX_FLAGS) -c softboundmpx.c -o softboundmpx.o
	clang $(MPX_FLAGS) -c softboundmpx-wrappers.c -o softboundmpx-wrappers.o
	ar $(ARFLAGS) libsoftboundmpx_rt.a softboundmpx.o softboundmpx-checks.o softboundmpx-wrappers.o

softboundcets_rt: softboundcets.h softboundcets-checks.c softboundcets.c softboundcets-wrappers.c 
	clang $(CFLAGS) -c softboundcets-checks.c -o softboundcets-checks.o
	clang $(CFLAGS) -c softboundcets.c -o softboundcets.o
	clang $(CFLAGS) -c softboundcets-wrappers.c -o softboundcets-wrappers.o
	ar $(ARFLAGS) libsoftboundcets_rt.a softboundcets.o softboundcets-checks.o softboundcets-wrappers.o

softboundcets_rt_lto: softboundcets.h softboundcets-checks.c softboundcets.c softboundcets-wrappers.c 
	mkdir lto
	clang $(CFLAGS) -flto -c softboundcets-checks.c -o lto/softboundcets-checks.lto.o
	clang $(CFLAGS) -flto -c softboundcets.c -o lto/softboundcets.lto.o
	clang $(CFLAGS) -flto -c softboundcets-wrappers.c -o lto/softboundcets-wrappers.lto.o
	ar --plugin=$(LLVM_GOLD) $(ARFLAGS) lto/libsoftboundcets_rt.a lto/softboundcets.lto.o lto/softboundcets-checks.lto.o lto/softboundcets-wrappers.lto.o


softboundmpx_rt_lto: softboundmpx.h softboundmpx-checks.c softboundmpx.c softboundmpx-wrappers.c
	clang $(MPX_FLAGS) -flto -c softboundmpx-checks.c -o lto/softboundmpx-checks.lto.o
	clang $(MPX_FLAGS) -flto -c softboundmpx.c -o lto/softboundmpx.lto.o
	clang $(MPX_FLAGS) -flto -c softboundmpx-wrappers.c -o lto/softboundmpx-wrappers.lto.o
	ar --plugin=$(LLVM_GOLD) $(ARFLAGS) lto/libsoftboundmpx_rt.a lto/softboundmpx.lto.o lto/softboundmpx-checks.lto.o lto/softboundmpx-wrappers.lto.o

softboundcetsmpx_rt_lto: softboundcetsmpx.h softboundcetsmpx-checks.c softboundcetsmpx.c softboundcetsmpx-wrappers.c
	clang $(SBCETS_MPX_FLAGS) -flto -c softboundcetsmpx-checks.c -o lto/softboundcetsmpx-checks.lto.o
	clang $(SBCETS_MPX_FLAGS) -flto -c softboundcetsmpx.c -o lto/softboundcetsmpx.lto.o
	clang $(SBCETS_MPX_FLAGS) -flto -c softboundcetsmpx-wrappers.c -o lto/softboundcetsmpx-wrappers.lto.o
	ar --plugin=$(LLVM_GOLD) $(ARFLAGS) lto/libsoftboundcetsmpx_rt.a lto/softboundcetsmpx.lto.o lto/softboundcetsmpx-checks.lto.o lto/softboundcetsmpx-wrappers.lto.o


clean:
	rm -rf *.o *.a *~ lto/

