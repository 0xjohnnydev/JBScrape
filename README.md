# JBScrape

Find jailbreakable iPhones on eBay and Swappa.

Searches for devices running **iOS 16.0-16.6.1** and **iOS 17.0-17.3.1**.

## Features

- Searches eBay and Swappa for iPhones with jailbreakable iOS versions
- Filters out impossible device/iOS combinations (e.g., iPhone 4 claiming iOS 16)
- Only checks seller descriptions on Swappa (ignores buyer comments asking about iOS)
- **OCR mode**: downloads listing photos and reads the iOS version off
  Settings > About screenshots when the seller didn't put it in the text
- Organizes results by device model and price
- Exports to JSON and optionally creates a note in macOS Notes app

## Requirements

- Python 3.8+
- Google Chrome browser
- (OCR only) [Tesseract](https://github.com/tesseract-ocr/tesseract) — `brew install tesseract`

## Installation

```bash
# Clone the repo and enter the directory
git clone https://github.com/zeroxjf/JBScrape.git
cd JBScrape

# Create and activate virtual environment (recommended)
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# For OCR mode, also install the tesseract engine:
brew install tesseract   # macOS (Linux: apt install tesseract-ocr)
```

## Usage

```bash
python JBScrape.py                    # Search eBay (~10 min, default)
python JBScrape.py --ocr              # Also OCR listing photos for the iOS version
python JBScrape.py --sites swappa     # Swappa only (slow ~20 min)
python JBScrape.py --sites ebay swappa  # Both sites (slow ~30 min)
python JBScrape.py --no-headless      # Show the browser window
python JBScrape.py --note             # Create a note in the macOS Notes app
```

## How It Works

1. **eBay**: Searches for listings mentioning specific iOS versions in titles
2. **Swappa**: Visits individual listings and checks seller descriptions only (not buyer Q&A comments)
3. **OCR** (with `--ocr`): For listings where the iOS version isn't in the
   text, downloads the gallery photos and runs OCR to read the version off a
   Settings > About screenshot. Results found this way are tagged `[OCR]`.
4. **Validation**: Filters out impossible combinations (e.g., old devices claiming new iOS)
5. **Output**: Groups results by device model, sorted by price

## License

MIT License - see [LICENSE](LICENSE)
