SOURCE_DIR := sources
IMAGE_DIR := images
DOT := $(shell which dot)
DOT_IMAGE_NAMES := \
	cfg-simple.png \
	cfg-threads.png \
	pipeline-tm.png \
	master-tm.png
DOT_OUTPUT := $(addprefix $(IMAGE_DIR)/, $(DOT_IMAGE_NAMES))

all: $(DOT_OUTPUT)

$(IMAGE_DIR)/%.png: $(SOURCE_DIR)/%.dot
	dot -Tpng -o $@ $<
