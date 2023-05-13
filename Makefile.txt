CC := gcc
CFLAGS := -c -Wall -Werror
LFLAGS := -I thirdparty -I src -c 
EXE := bin/canalize
EXE_TEST := bin/canalize-test

all: $(EXE) $(EXE_TEST)

$(EXE): build/src/main.o build/src/process.o build/src/strings.o
		$(CC) build/src/main.o build/src/process.o build/src/strings.o -o $@

build/src/main.o: src/main.c
		$(CC) -I src $(CFLAGS) src/main.c -o $@   

build/src/strings.o: src/strings.c
		$(CC) -I src $(CFLAGS) src/strings.c -o $@ 

build/src/process.o: src/process.c
		$(CC) -I src $(CFLAGS) src/process.c -o $@

$(EXE_TEST): build/test/process_test.o build/test/strings_test.o build/test/main.o build/src/process.o build/src/strings.o
		$(CC) build/test/process_test.o build/test/strings_test.o build/test/main.o build/src/process.o build/src/strings.o -o $@

build/test/process_test.o: test/process_test.c       
		$(CC) $(LFLAGS) test/process_test.c -o $@   

build/test/strings_test.o: test/strings_test.c   
		$(CC) $(LFLAGS) test/strings_test.c -o $@

build/test/main.o: test/main.c 
		$(CC) -I thirdparty -c test/main.c -o $@


.PHONY: all clean
clean:	
		rm -rf build/src/*.o
		rm -rf build/test/*.o
		rm -rf bin/canalize
		rm -rf bin/canalize-test
