# JBScrape

Find jailbreakable iPhones on eBay and Swappa.

Searches for devices running **iOS 16.0-16.7.2** and **iOS 17.0-17.3.1**.
With `--legacy`, also finds **iPhone 11 series and older** running any iOS up to
**26.0.1** (checkm8 / older-hardware coverage); iPhone 12 and newer stay limited
to the 16.x/17.x range.

## Features

- Searches eBay and Swappa for iPhones with jailbreakable iOS versions
- **OCR on by default**: downloads listing photos and reads the iOS version off
  Settings > About screenshots when the seller didn't put it in the text
  (disable with `--no-ocr`). Results found this way are tagged `[OCR]`.
- `--legacy` mode: extends the version ceiling to iOS 26.0.1 for iPhone 11
  series and older devices only
- Filters out impossible device/iOS combinations (e.g., iPhone 4 claiming iOS 16)
- Only checks seller descriptions on Swappa (ignores buyer comments asking about iOS)
- Organizes results by device model and price
- Exports to JSON and optionally creates a note in macOS Notes app

## Requirements

- Python 3.8+
- Google Chrome browser
- [Tesseract](https://github.com/tesseract-ocr/tesseract) for OCR (on by default) — `brew install tesseract`

## Installation

```bash
# Clone the repo and enter the directory
git clone https://github.com/zeroxjf/JBScrape.git
cd JBScrape

# Create and activate virtual environment (recommended)
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies (includes Pillow/pytesseract for OCR)
pip install -r requirements.txt

# OCR is on by default, so also install the tesseract engine:
brew install tesseract   # macOS (Linux: apt install tesseract-ocr)
```

## Usage

```bash
python JBScrape.py                    # Search eBay, OCR on (~10 min, default)
python JBScrape.py --legacy           # + iPhone 11 & older up to iOS 26.0.1
python JBScrape.py --no-ocr           # Skip photo OCR (title/description only)
python JBScrape.py --sites swappa     # Swappa only (slow ~20 min)
python JBScrape.py --sites ebay swappa  # Both sites (slow ~30 min)
python JBScrape.py --no-headless      # Show the browser window
python JBScrape.py --note             # Create a note in the macOS Notes app
```

## How It Works

1. **eBay**: Searches for listings mentioning specific iOS versions in titles
2. **Swappa**: Visits individual listings and checks seller descriptions only (not buyer Q&A comments)
3. **OCR** (default on, `--no-ocr` to disable): For listings where the iOS
   version isn't in the text, downloads the gallery photos and runs OCR to read
   the version off a Settings > About screenshot. Results are tagged `[OCR]`.
4. **Legacy mode** (`--legacy`): iPhone 11 series and older are accepted on any
   iOS up to 26.0.1; iPhone 12 and newer remain limited to the 16.x/17.x range.
5. **Validation**: Filters out impossible combinations (e.g., old devices claiming new iOS)
6. **Output**: Groups results by device model, sorted by price

## License

MIT License - see [LICENSE](LICENSE)
