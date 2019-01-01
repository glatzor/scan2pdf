This tool scans pages and converts them to PDF files. It automatically
tries to improve text readability by post-processing and to recognize
the scanned text using tessercat OCR.

It was written for private use having my Fujitsu ScanSnap S510 device in
mind.

Scan2pdf depends on the following tools:
 - convert
 - img2pdf
 - parallel
 - pdftk
 - scanimage
 - tesseract
 - unpaper

The following steps are performed:

 1. scanning (scanimage)
 2. deskwing, contrast corrections (convert)
 3. page detection and further post-processing (unpaper)
 4. optionally skipping blank pages (convert)
 6. loseless tiff image compression (convert)
 5. text recogniation (tessercat)
 7. PDF creation (tessercat or img2pdf)
 8. writing PDF metadata (pdftk)
 9. previewing (xdg-open)

By default the script will ask you for keywords. The final PDF file is
named after the current date and the keywords separated by spaces,
e.g. '2018-12-01_bill_tax_fujitsu_scansnap_s510.pdf'.

I took inspiration from Fred Weinhaus (textcleaner) and Florian Knodt
(adlerweb).

If you are looking for a graphical user interface the gscan2pdf
application is worth a try, see http://gscan2pdf.sourceforge.net/
It basically uses the same toolchain if you add convert as a custom
command.

General Options:
 -v, --verbose		Show additional info
 -h, --help		Show this help message

Scaning Options:
 -c, --color		Colored scan (default is grayscale)
 -d, --duplex		Duplex scan if supported by your scanner
 -s, --scanner SCANNER	Specify the scanner. Run 'scanimage -L' to get a list
			of supported scanners. Not required if only one is
			available

Post-Processing Options:
 --no-cleanup		Keep temporary files
 -l, --lang LANG	The languages that should be used for text
			recogniation. Run 'tesseract --list-langs' to get a
			list of supported languges
 --no-ocr		Skip text recogniation
 --rotate DEGREE	Rotate the page by the given degree
 --size SIZE		The size of the page: a5, a4, a3, letter, legal.
			All size names can also be applied in rotated
			landscape orientation, use a4-landscape,
			letter-landscape etc.
 --skip-blank		Detect and skip empty pages
 --layout single|double Layout of the scanned pages per sheet

PDF File Options:
 -a, --author NAME	Specify the author
 -t, --title NAME	Specify the subject

CONFIGURATION

Scan2pdf optionally sources the file ~/.scan2pdfrc. You can set
configuration options there. The file has to contain valid bash syntax.
Alternatively you can set the following options as environment
variables before executing scan2pdf e.g. 'export VERBOSE=1'.

General Options:

VERBOSE			If set show additional information
CREATOR			The name of the creator of the pdf file.
			Defaults to 'YOURNAME (scan2pdf)'

Scanning Options:

SCANMODE		Can be 'color', 'lineart' or 'gray'. Defaults
			to 'gray'
SCANNER			Specify the scanner that should be used
RESOLUTION		The resolution (dpi) of the scan. Defaults to 300
SOURCE			The source of your scanner e.g. 'ADF Duplex' if
			you always want to perform duplex scans and your
			scanner supports it
SCANIMAGE_OPTIONS	Extra options for the scanimage command
SIZE 			The size of the page: a5, a4, a3, letter, legal.
			All size names can also be applied in rotated
			landscape orientation, use a4-landscape,
			letter-landscape etc.


Post-Processing Options:

BLANK_TRESHOLD		Treshold for detecting blank pages. Default value if
			enabled is 100. An higher treshold value requires less
			pixels to declare a page as blank
NO_CLEANUP		Don't cleanup temporary files
NO_OCR			Sip text recogination
NO_KEYWORDS		Don't ask for keywords. The keywords are stored
			in the filename and pdf metadata
NO_PREVIEW		Don't preview the final pdf
PDF_VIEWER		Alternative pdf viewer. Defaults to 'xdg-open'
UNPAPER_OPTIONS 	Extra options for the unpaper command. E.g.
			'--layout double --skip-noisefilter'. See
			'unpaper --help' for more information.
TESSERACT_OPTIONS	Extra options for the tesseract command. See
			'tesseract --help' for more information.
CONVERT_DESKEW_TRESHOLD	The treshold for the 'convert -deskew' command.
			The default value is 40%
CONVERT_CURVE_LEVEL	The levels for 'convert -level' command.
			Defaults to 21%,73%. You can use the curve tool
			of gimp to adapt the values to your scanner
