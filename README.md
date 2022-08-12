# Description

This node.js library can **merge multiple PDF documents**, or parts of them, to one new PDF document. It's only dependency is [pdf-lib](https://pdf-lib.js.org/) so it can run in any javascript-only environnement **without any external dependencies**.

> If you are searching for the legacy version based on 
[pdfjs](https://www.npmjs.com/package/pdfjs) please install a [v3 release](https://github.com/nbesli/pdf-merger-js/releases?q=v3&expanded=true). Since [v4](https://github.com/nbesli/pdf-merger-js/releases?q=v4&expanded=true) we use [pdf-lib](https://pdf-lib.js.org/) instead.

This library is inspired by the [PHP library PDFMerger](https://github.com/myokyawhtun/PDFMerger) and has a very similar API.

## Installation

`npm install --save pdf-merger-js`

## Code sample

```js
const PDFMerger = require('pdf-merger-js');

var merger = new PDFMerger();

(async () => {
  await merger.add('pdf1.pdf');  //merge all pages. parameter is the path to file and filename.
  await merger.add('pdf2.pdf', [2]); // merge only page 2
  await merger.add('pdf2.pdf', [1, 3]); // merge the pages 1 and 3
  await merger.add('pdf2.pdf', '4, 7, 8'); // merge the pages 4, 7 and 8
  await merger.add('pdf3.pdf', '1 to 2'); //merge pages 1 to 2
  await merger.add('pdf3.pdf', '3-4'); //merge pages 3 to 4

  await merger.save('merged.pdf'); //save under given name and reset the internal document
})();
```

### Browser Sample - React

```jsx
import PDFMerger from 'pdf-merger-js/browser';
import React, { useEffect, useState } from 'react';

// files: Array of PDF File or Blob objects
const Merger = (files) => {
  const [mergedPdfUrl, setMergedPdfUrl] = useState();

  useEffect(() => {
    const render = async () => {
      const merger = new PDFMerger();

      for(const file of files) {
        await merger.add(file)
      }

      const mergedPdf = await merger.saveAsBlob();
      const url = URL.createObjectURL(mergedPdf);

      return setMergedPdfUrl(url);
    };

    render().catch((err) => {
      throw err;
    });

    () => setMergedPdfUrl({});
  }, [files, setMergedPdfUrl]);

  return !data ? (
    <>Loading</>
  ) : (
    <iframe
      height={1000}
      src={`${mergedPdfUrl}`}
      title='pdf-viewer'
      width='100%s'
    ></iframe>
  );
};
```

## Similar libraries

* [pdf-merge](https://www.npmjs.com/package/pdf-merge) has a dependency on [PDFtk](https://www.pdflabs.com/tools/pdftk-the-pdf-toolkit/).
* [easy-pdf-merge](https://www.npmjs.com/package/easy-pdf-merge) has a dependency on the [Apache PDFBox® - A Java PDF Library](https://pdfbox.apache.org/).
* [pdfmerge](https://www.npmjs.com/package/pdfmerge) has a dependency on python and [PyPDF2](https://pythonhosted.org/PyPDF2/).
