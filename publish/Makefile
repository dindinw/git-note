all: md-guide.html md-guide-single.html md-guide-s5.html md-guide-docbook.db md-guide.pdf

md-guide.html: ../gfm-syntax-guide.md
	pandoc --offline -s -t slidy -f markdown -o $@ $<

md-guide-single.html: ../gfm-syntax-guide.md
	pandoc --offline -s -t html -o $@ $<

md-guide-s5.html: ../gfm-syntax-guide.md
	pandoc --offline -s -t s5 -o $@ $<

md-guide-docbook.db: ../gfm-syntax-guide.md
	pandoc -s -S -t docbook $< -o $@

md-guide.pdf: ../gfm-syntax-guide.md
	markdown2pdf $< -o $@

clean:
	-rm -f md-guide.html md-guide-single.html md-guide-s5.html md-guide-docbook.db md-guide.pdf

