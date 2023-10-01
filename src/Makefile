# Update these paths to point to the correct locations on your system
SAMTOOLS_PATH = ../lib/samtools
HTSLIB_PATH = ../lib/htslib
LIBGAB_PATH = ../lib/libgab

CXX = g++ -std=c++11
CXXFLAGS = -Wall -Wno-char-subscripts -lm -O3 -lz -I$(LIBGAB_PATH) -I$(LIBGAB_PATH)/gzstream/ -I$(SAMTOOLS_PATH) -I$(HTSLIB_PATH) -c
LDFLAGS = -lgsl -lcurl -lgslcblas -llzma -lbz2 -lz -lpthread
LDLIBS = $(SAMTOOLS_PATH)/bedidx.o $(HTSLIB_PATH)/libhts.a $(SAMTOOLS_PATH)/libst.a $(SAMTOOLS_PATH)/libst.a

all: libgab htslib samtools bam2prof diffprof

libgab:
	git clone --recursive https://github.com/grenaud/libgab.git $(LIBGAB_PATH)
	cd $(LIBGAB_PATH) && make libgab.a && make -C gzstream/ && cd ../../

htslib:
	if [ ! -d "$(HTSLIB_PATH)" ]; then \
		git clone --recursive https://github.com/samtools/htslib.git $(HTSLIB_PATH); \
	fi
	cd $(HTSLIB_PATH) && git submodule update --init --recursive && make && cd ../../

samtools:
	if [ ! -d "$(SAMTOOLS_PATH)" ]; then \
		git clone --recursive https://github.com/samtools/samtools.git $(SAMTOOLS_PATH); \
	fi
	cd $(SAMTOOLS_PATH) && make && cd ../../

%.o: %.cpp
	$(CXX) $(CXXFLAGS) $^ -o $@

bam2prof: bam2prof.o ReconsReferenceHTSLIB.o $(LIBGAB_PATH)/libgab.a $(LIBGAB_PATH)/gzstream/libgzstream.a
	$(CXX) -o $@ $^ $(LDLIBS) $(LDFLAGS)

diffprof: diffprof.o ReconsReferenceHTSLIB.o $(LIBGAB_PATH)/libgab.a $(LIBGAB_PATH)/gzstream/libgzstream.a
	$(CXX) -o $@ $^ $(LDLIBS) $(LDFLAGS)

clean:
	rm -f *.o bam2prof diffprof

