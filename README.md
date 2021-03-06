# Bulgarian Constitutional Court decisions

## Using Tesseract to Convert Scanned PDFs to Text Data

### Prerequisites

The following packages need to be installed:

- Tesseract - An open source text recognition (OCR) engine. A guide for installing Tesseract can be found [here](https://tesseract-ocr.github.io/tessdoc/Home.html), and documentation can be found [here](https://github.com/tesseract-ocr/tesseract).
- Xpdf - Xpdf offers a selection of command line tools for handling PDF files, including converting PDFs to image files. Xpdf can be downloaded and installed [here](https://www.xpdfreader.com/download.html).

### Process

The following guide is carried out using the command line.

#### 1. Turn pdf files to png

```bash
pdftoppm file.pdf img -png
```

#### 2. If images were low-quality scans (with annotations, black marks etc.), crop image so that only the main body text is included

#### 3. Process png files using tesseract

- If carried out on just one image:

```bash
tesseract img.png newFile -l bul
```

- If carried out on a batch of images:

```bash
for file in *.png  ; do tesseract "$file" "${file%%.*}" -l bul; done
```

#### 4. Combine all text documents

```bash
for file in *.txt; do (cat "${file}"; echo) >> bulgarian_combined.txt; done
```

## Converting DOCX Files to Text Data

The following guide is carried out using the command line.

### 1. Identify DOCX files that will be converted

```bash
find . -name "*.docx" | while read file; do
unzip -p $file word/document.xml |
sed -e 's/<[^>]\{1,\}>//g; s/[^[:print:]]\{1,\}//g' > "${file/docx/txt}"
done
```

### 2. Convert DOCX files to TXT

```bash
grep -r "." --include "*.txt" .
```

## TO-DO

- [x] Write initial guide using tesseract in bash terminal
- [x] Write guide using tesseract in windows/macOS
- [x] Write guide for converting DOCX files to TXT
- [ ] Test pdftools (R) and pdfminer.six (Python) and consider alternative approaches
- [x] Test process for batch editing files using pytesseract
- [x] Think about the best approach for batch translating txt files from Bulgarian to English
