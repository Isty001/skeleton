CC ?= gcc
DEPS = 
CFLAGS += -std=c99 -g -Wall -Wextra -ftrapv -Wshadow -Wundef -Wcast-align -Wunreachable-code -fstack-protector -D_FORTIFY_SOURCE=2
CFLAGS += -O1 -Os
CFLAGS += -I .

SRC = $(shell find src -name "*.c")
TEST_SRC = $(shell find test -name "*.c") $(SRC)

TARGET = 
TEST_TARGET = $(TARGET)-test

VERSION_MAJOR = 0
VERSION_MINOR = 0
VERSION_PATCH = 0

LIB_OBJ = lib$(TARGET).so
SONAME = $(LIB_OBJ).$(VERSION_MAJOR)
MINOR_NAME = $(SONAME).$(VERSION_MINOR)
FULL_NAME = $(MINOR_NAME).$(VERSION_PATCH)

INCLUDE_DIR = /usr/local/include/$(TARGET)
LIB_DIR = /usr/local/lib

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
	$(CC) -shared -fpic -Wl,-soname,$(SONAME) -o $(FULL_NAME) $(OBJECTS) $(DEPS)

$(TEST_TARGET): $(TEST_OBJECTS)
	$(CC) $(TEST_OBJECTS) $(CFLAGS) -o $@

project-init:
	rm -f ./src/.gitkeep ./test/.gitkeep ./include/.gitkeep

clean:
	rm -rf $(DIR_BUILD)/*

install: $(TARGET)
	sudo mv $(FULL_NAME) $(LIB_DIR)
	ln -sf $(LIB_DIR)/$(FULL_NAME) $(LIB_DIR)/$(MINOR_NAME)
	ln -sf $(LIB_DIR)/$(MINOR_NAME) $(LIB_DIR)/$(SONAME)
	ln -sf $(LIB_DIR)/$(SONAME) $(LIB_DIR)/$(LIB_OBJ)
	sudo mkdir -p $(INCLUDE_DIR)
	sudo cp include/*.h $(INCLUDE_DIR)

compile-test:
	$(CC) $(TEST_CFLAGS) $(TEST_SRC) -o $(TEST_TARGET) -l$(TARGET)

test: $(TARGET) install compile-test
	./$(TEST_TARGET)

valgrind-test: clean compile-test
	valgrind --track-origins=yes --leak-check=full --show-reachable=yes --error-exitcode=1 ./$(TEST_TARGET)
