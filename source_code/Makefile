# CANON - Makefile
# Francesco Pedulli, 2026

CC = gcc
CFLAGS = -O3 -Wall -Wextra -std=c11 -march=native
LDFLAGS = -lm

SOURCES = canon_optimal.c
TARGET = canon

.PHONY: all clean test benchmark

all: $(TARGET)

$(TARGET): $(SOURCES)
	$(CC) $(CFLAGS) -o $@ $< $(LDFLAGS)
	@echo "✓ Built: $(TARGET)"
	@echo "  Complexity: Θ(n·r) - provably optimal"

clean:
	rm -f $(TARGET) *.o *.canon test_*.bin
	@echo "✓ Cleaned"

test: $(TARGET)
	@echo "Testing CANON compression..."
	@echo "Creating test file..."
	@dd if=/dev/urandom of=test_random.bin bs=1M count=1 2>/dev/null
	@echo ""
	./$(TARGET) compress test_random.bin test_random.canon
	@echo ""
	@echo "✓ Test complete"

benchmark: $(TARGET)
	@echo "Benchmarking CANON on various data types..."
	@echo ""
	@echo "1. Random data (worst case - high rank):"
	@dd if=/dev/urandom of=test_1mb.bin bs=1M count=1 2>/dev/null
	@./$(TARGET) compress test_1mb.bin
	@echo ""
	@echo "2. Structured data (best case - low rank):"
	@python3 -c "print('A' * 1048576)" > test_structured.bin
	@./$(TARGET) compress test_structured.bin
	@echo ""
	@echo "3. Text data (average case):"
	@cat /usr/share/dict/words /usr/share/dict/words > test_text.bin 2>/dev/null || echo "test text" > test_text.bin
	@./$(TARGET) compress test_text.bin
	@echo ""
	@echo "✓ Benchmark complete"

install: $(TARGET)
	@echo "Installing CANON..."
	@mkdir -p ~/bin
	@cp $(TARGET) ~/bin/
	@echo "✓ Installed to ~/bin/canon"
	@echo "  Add ~/bin to PATH if needed"

.DEFAULT_GOAL := all
