PDF Boilerplate
===============

This is a boilerplate for PDF compilation from markdown.

Simply create your pdf by editing markdown files in `src/main/markdown`.

You may theme your book by placing scss files in `src/main/scss`, and including them in your markdown header/foot using Maruku's meta-data syntax.

Requirements
============

This suite uses `wkhtmltopdf` and `Maruku`. `bundle install` to get everything but `wkhtmltopdf`.

Get `wkhtmltopdf` [here](http://wkhtmltopdf.org/). In order for the script to work, make sure that `which` can find `wkhtmltopdf`.
