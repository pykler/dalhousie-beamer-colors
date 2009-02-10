# set origfile to the name of the main file without the .tex
origfile = example
latexfile = $(origfile)_
TEX = latex

all: pdf

$(latexfile).tex : $(origfile).tex
	cp $< $@

$(latexfile).dvi : $(latexfile).tex
	rm -f $(latexfile).aux $(latexfile).out
	$(TEX) $(latexfile)
	if(grep -q "Citation" $(latexfile).log) then \
	bibtex $(latexfile); fi
	while ($(TEX) $(latexfile) ; \
	grep -q "Rerun to get cross" $(latexfile).log ) do true ; \
	done

# Last Time I Checked ps2pdf on Mac OS X was buggy
pdf : $(origfile).pdf
$(origfile).pdf : $(origfile).ps
	if( echo `uname -a` | grep -i darwin >/dev/null) then \
	dvipdf -dPDFSETTINGS=/prepress $(origfile); fi
	if( ! echo `uname -a` | grep -i darwin >/dev/null) then \
	ps2pdf -dPDFSETTINGS=/prepress $(origfile).ps $(origfile).pdf; fi

ps : $(origfile).ps
$(origfile).ps : $(origfile).dvi
	dvips -o $@ $<

dvi : $(origfile).dvi
$(origfile).dvi : $(latexfile).dvi
	    cp $(latexfile).dvi $(origfile).dvi

clean:
	rm -f $(latexfile).*

distclean: clean
	rm -fr $(origfile).pdf $(origfile).ps $(origfile).dvi
