SOURCE_DIR := sources
IMAGE_DIR := images
DOT := $(shell which dot)
DOT_OUTPUT := $(addprefix $(IMAGE_DIR)/, cfg-simple.png cfg-threads.png)

all: $(DOT_OUTPUT)

$(IMAGE_DIR)/%.png: $(SOURCE_DIR)/%.dot
	dot -Tpng -o $@ $<
