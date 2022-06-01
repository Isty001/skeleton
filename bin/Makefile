CC ?= gcc
DEPS = 
CFLAGS += -std=c99 -g -Wall -Wextra -ftrapv -Wshadow -Wundef -Wcast-align -Wunreachable-code -fstack-protector -D_FORTIFY_SOURCE=2
CFLAGS += -O1 -Os
CFLAGS += -I .

SRC = $(shell find src -name "*.c")
TEST_SRC = $(shell find test -name "*.c") $(SRC)

TARGET = 
TEST_TARGET = $(TARGET)-test

INCLUDE_DIR = /usr/local/include/$(TARGET)
BIN_DIR = /usr/local/bin

DIR_BUILD=./build

OBJECTS = $(patsubst %.c, $(DIR_BUILD)/%.o, $(SRC))
TEST_OBJECTS = $(patsubst %.c, $(DIR_BUILD)/%.o, $(TEST_SRC))


default: $(TARGET)

-include $(shell find $(DIR_BUILD) -name "*.d")


$(DIR_BUILD)/%.o: %.c
	mkdir -p $(shell dirname $@)
	$(CC) -MMD $(CFLAGS) -c -fpic $< -o $@

.PRECIOUS: $(TARGET) $(OBJECTS)


$(TARGET): $(OBJECTS)
	$(CC) -o $(TARGET) $(OBJECTS) $(DEPS)

$(TEST_TARGET): $(TEST_OBJECTS)
	$(CC) $(TEST_OBJECTS) $(CFLAGS) -o $@

project-init:
	rm -f ./src/.gitkeep ./test/.gitkeep

clean:
	rm -rf $(DIR_BUILD)/*

install: $(TARGET)
	sudo cp $(TARGET) $(BIN_DIR)

compile-test:
	$(CC) $(TEST_CFLAGS) $(TEST_SRC) -o $(TEST_TARGET) -l$(TARGET)

test: $(TARGET) compile-test
	./$(TEST_TARGET)

valgrind-test: clean compile-test
	valgrind --track-origins=yes --leak-check=full --show-reachable=yes --error-exitcode=1 ./$(TEST_TARGET)
