DEPS = 
CFLAGS += -std=c99 -g -Wall -Wextra -ftrapv -Wshadow -Wundef -Wcast-align -Wunreachable-code -fstack-protector
CFLAGS += -O1 -Os
CFLAGS += -I .

SRC = $(shell find src -name "*.c")
TEST_SRC = $(shell find test -name "*.c")

TARGET = 
TEST_TARGET = $(TARGET)-test

VERSION_MAJOR = 0
VERSION_MINOR = 0
VERSION_PATCH = 0

LIB_OBJ = lib$(TARGET).so
SONAME = $(LIB_OBJ).$(VERSION_MAJOR)
FULL_NAME = $(SONAME).$(VERSION_MINOR).$(VERSION_PATCH)

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


$(TARGET): clean $(OBJECTS)
	$(CC) -shared -fpic -Wl,-soname,$(SONAME) -o $(FULL_NAME) $(OBJECTS) $(DEPS)

$(TEST_TARGET): $(TEST_OBJECTS)
	$(CC) $(TEST_OBJECTS) $(CFLAGS) -o $@

project-init:
	rm -f ./src/.gitkeep ./test/.gitkeep ./include/.gitkeep

clean:
	rm -rf $(DIR_BUILD)/*

install: $(TARGET)
	sudo mv $(FULL_NAME) $(LIB_DIR)
	ln -sf $(LIB_DIR)/$(FULL_NAME) $(LIB_DIR)/$(SONAME)
	ln -sf $(LIB_DIR)/$(SONAME) $(LIB_DIR)/$(LIB_OBJ)
	sudo mkdir -p $(INCLUDE_DIR)
	sudo cp include/*.h $(INCLUDE_DIR)

compile-test:
	$(CC) $(CFLAGS) $(TEST_SRC) -o $(TEST_TARGET) -l$(TARGET)

test: $(TARGET) install compile-test
	./$(TEST_TARGET)

