# Introduction

There are different ways of generating and serving PDFs with Seaside. This should provide you with the questions you need to answer to be able to decide how to proceed.

# What kind of possibilities are there?

- SPDF, a low-level, not very complete smalltalk library
- LibHaru, a low-level, more complete c library
- pdflatex, a complex typesetting system based on LaTeX
- Apache fop, an xml-based layouting engine
- Rome/Cairo, a graphical library with pdf export
- PDFReactor, a xhtml/css-to-PDF rendering engine (commercial)
- Artefact, a low-level (but growing) smalltalk library (Pharo)

# What kind of document do you want to create?

## Do you need automatic line-breaking?

For longer documents, you probably want to be able to automatically wrap longer lines of dynamic text.

- SPDF doesn’t provide this function.
- LibHaru doesn’t provide this function.
- LaTeX uses an advanced line-breaking algorithm.
- Apache fop automatically breaks lines.
- Cairo has Pango, which provides line-breaking.
- Plain Rome doesn’t provide line breaking.
- PDFReactor is quite capable of laying out the content based on provided stylesheets and page configuration (commercial license required, demo available).

## Do you need paragraph breaking over multiple pages?

- SPDF doesn’t provide this function.
- LibHaru doesn’t provide this function.
- LaTeX uses an page-breaking algorithm, taking in account widows and orphans.
- Apache fop automatically breaks paragraphs over multiple pages.
- Cairo itself has no concept of pages.
- Pango provides the basics for breaking text over multiple boxes, but doesn’t have the notion of pages.
- PDFReactor does break paragraphs over multiple pages.

## Do you need absolute positioning of graphical elements?

- LaTeX and FOP are difficult to use with absolute positioning. They insist on using their own layouting engine. This can be an advantage.
- SPDF provides absolute positioning of elements.

## Do you need international font support?

- LaTeX is good at that, FOP is completely unicode based.
- Cairo is unicode based, Pango is built to handle difficult support things like right-to left mixed with left-to right.
- SPDF uses iso-latin1.
- PDFReactor fully supports unicode rendering.

## Do you need images?

- SPDF only works with monochrome tiffs.
- LibHaru can use different kinds of raster images.
- LaTeX can include different kind of raster images.
- Apache fop can use diferent kind of raster images and SVG.
- Cairo can use different kind of raster images.
- PDFReactor will render raster images as well as SVG content for high quality print output.

## Do you want to use other fonts than the standard PDF fonts?

PDF has build-in support for 13 fonts, but allows other ones to be included.

- It is difficult to integrate new fonts into LaTeX, but possible. LaTeX has the best support for mathematics. LaTeX has an ascii encoding of some characters.
- SPDF can only handle the standard fonts.
- HaruPDF can load truetype and postscript fonts.
- Cairo with FreeType can use (nearly) all platform fonts.
- PDFReactor can use all platform fonts; TrueType and OpenType fonts are supported.

## Do you want to create interactive content?

- With LaTeX you can use the hyperref package to create links and forms.
- SPDF doesn’t have links.
- With PDFReactor you can ember flash, hyperlinks, bookmarks, barcodes, form elements etc.

# When do you want to generate your PDFs?

- SPDF can streamfiles directly to the socket, so that people can start to download and read documents while they are generated.
- HaruPDF is integrated and fast.
- LaTeX and FOP are rather slow when generating PDF. For complicated documents it is a good idea to cache created files.
- Cairo is integrated and fast.
- PDFReactor is a webservice which will return PDF bytes in response to a request feeding it xhtml/css.
Calling external programs is done through FFI (for dll’s) or OSProcess (for executables).

# How do you want to serve PDFs?

PDF files can be generated and served through Seaside, but that consumes a lot of CPU with increasing size of the document. Saving the documents on the file-system or using a separate image to upload the generated file is often more efficent.

# Which solution works on what platform?

- Squeak: SPDF, libharu, pdflatex, fop, cairo, pdfreactor
- VW: SPDF, cairo, pdfreactor

PDFReactor can be used from any dialect with support for HTTP requests, but it is especially trivial in dialects capable of generating a web interface from WSDL.

# Speed and Scalability

Large documents should be generated, cached and served externally.

# Examples

## SPDF

The script creates a pdf ’hello world’ document named spdf02.pdf.

```smalltalk
| myStream pdfWriter |
myStream := (self testFileNamed: 'spdf02.pdf') writeStream.
pdfWriter := PDFWriter on: myStream.
pdfWriter compressionOff.
pdfWriter write: 'Hello, World (From PDFTester)'.
pdfWriter close.
myStream close
```

## LibHARU

The script creates a pdf document in the squeak directory containing a single page with a 2 by 2 inch image 1 inch from the lower left corner.

```smalltalk
| document page image |
document := PDFDocument new.
page := document addPage.
image := document loadPNGImage: 'Image.png'.
page drawImage: image rectangle: (Rectangle origin: 72@72 extent: 144@144).
document saveToFile: 'documentWithImage.pdf'
```

## LaTeX

```smalltalk
generateLaTeX: aFileName
        | aFile |
        aFile := CrLfFileStream fileNamed: aFileName, '.tex'.
        aFile lineEndConvention: #crlf.
        self startOn: aFile.
        self usecases do: [:each| self printUseCase: each on: aFile].
        self endOn: aFile.
        aFile close

startOn: aStream
        aStream nextPutAll:'\documentclass[12pt,a4paper]{article}'; cr.
        aStream nextPutAll:'\usepackage[english,dutch]{babel}'; cr.
        aStream nextPutAll:'\begin{document}'; cr.
        aStream nextPutAll: '\begin{tabular}{|l|l|}'; cr.
        aStream nextPutAll: '\hline ';cr

endOn: aStream
        aStream nextPutAll:'\end{tabular}'; cr.
        aStream cr; cr.
        aStream nextPutAll: '\end{document}'; cr

printUseCase: aUseCase on: aStream
        aStream nextPutAll: (aUseCase usecaseName) ,' & ',(aUseCase usecaseDescription), ' \\ hline '; cr
```

You can use that script to write the file usecase.tex to the same directory the squeak image is in (that is not secure). Also think about the situation where multiple people try to run this at the same time.

```smalltalk
runLaTeXOnUsecase
        OSProcess waitForCommand:'pdflatex usecase.tex'
```

Generates usecase.pdf in the same directory on Unix.

## PDFReactor

```smalltalk
self
  downloadBytes: (AccountingReportComponent on: self statement) asPDF
  mime: 'application/pdf'
  filename: 'accountingreport.pdf'
```

where,

```smalltalk
asPDF
  | handler response |
  handler := WARenderContinuation root: (PDFRoot for: self).
  response := WAResponse new.
  handler context: (WARenderingContext new session: self session; actionUrl: WAUrl new; yourself).
  handler processRendering: response.
  ^ (PDFReactorClient default convertXHTML: response contents contents) getPDF
```

in Seaside 3.0, this might look like:

```smalltalk
Component>>asPDF
| xhtml |
xhtml := WAHtmlCanvas builder
		fullDocument: true;
		rootBlock: 
			[:root |
			root style add: (Styles default documentForFile: 'print.css').
			root style add: 'svg {-ro-replacedelement: ==''com.realobjects.xml.formatters.SVGFormatter'';}'];
		render: [:html | html render: self].
^(PDFreactorClient default convertXHTML: xhtml) bytes
```

uploading the file from squeak (should probably be done from Apache) in a WAComponent

```smalltalk
renderContentOn: html
  html submitButton on: #export of:self
export
  | aFile contents|
  aFile := CrLfFileStream readOnlyFileNamed: 'usecase.pdf'.
  aFile binary.
  contents := aFile contents.
  aFile close.
  self session returnResponse: (WAResponse
    document: contents
    mimeType: 'application/pdf'
    fileName: 'usecase.pdf')
```

in Seaside 3.0 the way to create a response changed:

```smalltalk
    self requestContext respond: [ :response |
        response
            contentType:'application/pdf'
            attachmentWithFileName:'usecase.pdf';
            binary;
            nextPutAll: thePdf
```

# Where do you find all this software?

- SPDF: SPDF
- LibHARU: HPDF, LibHARU
- LaTeX: MikTeX LaTeX, TeX Live LaTeX
- Cairo: Sophie, Rome, Cairo, Pango
- Apache FOP
- PDFReactor