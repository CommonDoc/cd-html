# CommonHTML

[![Build Status](https://travis-ci.org/CommonDoc/common-html.svg)](https://travis-ci.org/CommonDoc/common-html)

[![Coverage Status](https://coveralls.io/repos/CommonDoc/common-html/badge.svg?branch=master)](https://coveralls.io/r/CommonDoc/common-html?branch=master)

An HTML parser/emitter for [CommonDoc](https://github.com/CommonDoc/common-doc).

# Usage

```lisp
(defvar node
  (doc
   (document
    (:title "My Document"
     :creator "me"
     :keywords (list "test" "test1"))
    (paragraph
     ()
     (text-node
      (:text "test"))))))

(common-html.emitter:node-to-html-string node) ;; => "<p>test</p>"
```

# Multi-file emission

Normally, a document is emitted into HTML as a single file. You can also perform
Texinfo/Sphinx style emission, where a document is broken up into sections, and
each section (Up to a certain depth, or any depth) is emitted as a different
file.

To emit a document into multiple files, simply do:

```lisp
(common-html.multi-emit:multi-emit doc #p"output-directory/")
```

An optional keyword argument, `:max-depth`, can be provided to choose at what
section depth to stop emitting each section in a different file. For instance,
if you have a document that looks like this:

1. Intro
  1. Overview
  2. History
    1. Motivation
2. Tutorial

Emitting it with the default `:max-depth` of `nil` will produce 5 files, while
emitting it with a `:max-depth` of 2 will produce four files: One for each of
the Intro, Overview and Tutorial subsections, and another for both the History
section and its Motivation subsection.

## How it Works

Multi-part file emission can be complicated.

First, some obvious choices, and how CommonHTML chooses:

1. Should the directory structure of the HTML output mirror that of the
   sections? Or should all HTML files be emitted within the same directory?
   Answer: For simplicity (Users might not expect or want nested files), all
   HTML files are emitted into the same directory.

2. What should the name of the resulting HTML files be? The pure name of the
   section? The result of calling `common-doc.util:string-to-slug` on the
   section? Or an autogenerated ID? Answer: The first option might produce
   invalid pathnames, and the last option is an inconceivable abomination, so we
   just go with slugifying the section text.

# License

Copyright (c) 2014-2015 Fernando Borretti

Licensed under the MIT License.
