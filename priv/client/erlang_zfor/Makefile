ifndef ROOT
	ROOT = $(shell pwd)
endif

VSN=1.0.5

COMMON_TEST_LIB = $(shell erl -noshell -eval 'io:format("~s~n", [code:lib_dir(common_test)]).' -s init stop)
TEST_SERVER_LIB = $(shell erl -noshell -eval 'io:format("~s~n", [code:lib_dir(test_server)]).' -s init stop)
RUN_TEST_CMD = $(COMMON_TEST_LIB)/priv/bin/run_test

INCLUDE = $(PWD)/include

all: subdirs

subdirs:
	cd src; VSN=$(VSN) ROOT=$(ROOT) make

test: test.spec test_do

test_compile: subdirs
	cd test; \
		ROOT=$(ROOT) TEST_SERVER=$(TEST_SERVER_LIB) RUN_TEST=$(COMMON_TEST_LIB) make

test_do: test_compile
	mkdir -p log
	${RUN_TEST_CMD} -I$(INCLUDE) -pa $(ROOT)/ebin -logdir log -spec test.spec -cover test/zfor_client.coverspec

test.spec: test.spec.in
	cat test.spec.in | sed -e "s,@PATH@,$(PWD)," > $(PWD)/test.spec

dialyze:
	cd src; make dialyze

clean:
	rm -f *~ ebin/* *.boot *.script *.tar.gz *.rel test.spec src/*.rel.in
	rm -rf log
	cd src; ROOT=$(ROOT) make clean
	cd test; ROOT=$(ROOT) make clean

.PHONY: all

