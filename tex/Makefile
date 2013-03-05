LATEX = pdflatex
TEXFILES = $(wildcard *.tex)
BIBFILES = $(wildcard *.bib)

MAINFILE = hotmobile13-report

all: $(MAINFILE).pdf

%.pdf: %.tex %.bib %.blg *.sty  $(TEXFILES) $(BIBFILES)
	pdflatex $*.tex 
	bibtex --min-crossrefs=20 $*
	pdflatex $*.tex
	pdflatex $*.tex

%.blg: %.bib
	pdflatex $*.tex
	bibtex --min-crossrefs=20 $*
	pdflatex $*.tex

%.toc: %.tex
	pdflatex $*.tex

clean:
	/bin/rm -f $(MAINFILE).pdf *.dvi *.aux *.ps *~ *.log
	/bin/rm -f *.lot *.lof *.toc *.blg *.bbl url.sty *.out

view: 
	pdflatex $(MAINFILE).tex
	evince $(MAINFILE).pdf

osx:
	pdflatex $(MAINFILE).tex
	open $(MAINFILE).pdf

check:
	pdflatex $(MAINFILE).tex | grep -s -e "multiply" -e "undefined"

update:
	cat revision.tex | perl -ane \'\$r = \`git log --oneline | wc -l\`;\$r++;s/\d+/\$r/;print \$_\' > _r; mv _r revision.tex
upload:
	git pull
	pdflatex $(MAINFILE).tex
	scp $(MAINFILE).pdf martins@ssh.cs.brown.edu:www/files/$(MAINFILE)-draft.pdf
