all: md-guide.html md-guide-single.html md-guide-s5.html md-guide.pdf

md-guide.html: ../gfm-syntax-guide.md
	pandoc --offline -s -t slidy -o $@ $<

md-guide-single.html: ../gfm-syntax-guide.md
	pandoc --offline -s -t html -o $@ $<

md-guide-s5.html: ../gfm-syntax-guide.md
	pandoc -s -m -i -t s5 -o $@ $<

md-guide.pdf: ../gfm-syntax-guide.md
	markdown2pdf $< -o $@
clean:
	-rm -f md-guide.html md-guide-single.html md-guide-s5.html md-guide.pdf

