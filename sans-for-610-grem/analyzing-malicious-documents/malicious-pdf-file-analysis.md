# Malicious PDF File Analysis

## PDF Structure

The structure and contents of a PDF file are defined using objects

1. Header - Contains information about the version of PDF specifications
2. Objects (one or more) - Specify how to render the document and could include text, graphics, and dynamic components such as Javascript
3. Cross-reference table (xref) - Specifies offsets where the files objects are located. This allows for easy modification of the file without rewriting the whole thing.
4. Trailer - Containes offsets of the xref table and number of objects

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption><p>pdf structure</p></figcaption></figure>

PDF opened in a normal text editor

* Objects start with 'obj' and end with 'endobj'
* The first number is the object number and the second number is the version number
* Objects can also reference other objects
* /AA is automatic action
* /O is open document
  * 43  0 references another object

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

### Streams

PDF objects use streams to store data, such as text. Malicious PDFs sometimes conceal Javascript inside streams. Streams are delineated by "stream" and "endstream"

## Analysis

1. Open PDF in VisualStudio Code
   1. This provides a quick look at the PDF, nothing too interesting, any other text editor would probably do just fine
2. Open pdfid.py - Performs basic triage and displays keywords and number of instances, 0 means "no instances of keyword"
   1. Some malicious documents might use OpenAction or URI (among others)
   2. <img src="../../.gitbook/assets/image (41).png" alt="" data-size="original">
3. Open pdfparser.py
   1. pdf-parser.py \<filename> -a _\[shows statistics]_
   2. pdf-parser.py \<filename> -s /URI | more _\[searches document]_
   3. pdf-parser.py \<filename> -k /URI \[_shows just values for the key, "URI" in this example]_
   4. pdf-parser.py \<filename> -o \<object number> \[examine specific object]
   5. pdf-parser.py \<filename> -r \<object number> \[finds references to the specified object]
   6.

