SOURCE_DIR := sources
IMAGE_DIR := images
DOT := $(shell which dot)
DOT_IMAGE_NAMES := \
	cfg-simple.png \
	cfg-threads.png \
	pipeline-tm.png \
	master-tm.png \
	ndp-sra.png \
	ndp-ura.png \
	ndp-ura-attack1.png \
	ndp-ura-attack2.png \
	ndp-ar-network.png \
	ndp-ar-host.png \
	ndp-ar-attack.png \
	arp-network.png \
	arp-spoof.png

DOT_OUTPUT := $(addprefix $(IMAGE_DIR)/, $(DOT_IMAGE_NAMES))

all: $(DOT_OUTPUT)

$(IMAGE_DIR)/%.png: $(SOURCE_DIR)/%.dot
	dot -Tpng -o $@ $<
