Check figure number

To compile run

	#pandoc index.md -o index.html
	#pandoc -s -S -c ../isaw-papers-7.css index.md -o index.html
	pandoc --bibliography=/Users/rromanello/Documents/library.bib -s -S -c ../isaw-papers-7.css index.md -o index.html
