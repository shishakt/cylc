#!/usr/bin/make -f

#C: THIS FILE IS PART OF THE CYLC SUITE ENGINE.
#C: Copyright (C) 2008-2012 Hilary Oliver, NIWA
#C: 
#C: This program is free software: you can redistribute it and/or modify
#C: it under the terms of the GNU General Public License as published by
#C: the Free Software Foundation, either version 3 of the License, or
#C: (at your option) any later version.
#C:
#C: This program is distributed in the hope that it will be useful,
#C: but WITHOUT ANY WARRANTY; without even the implied warranty of
#C: MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#C: GNU General Public License for more details.
#C:
#C: You should have received a copy of the GNU General Public License
#C: along with this program.  If not, see <http://www.gnu.org/licenses/>.

CYLC=$(dir $(abspath $(lastword $(MAKEFILE_LIST))))../bin/cylc

.PHONY: all clean html html-multi html-single pdf

all: index.html

clean:
	rm -rf *.aux\
        command-usage\
        commands.tex\
        cug.html\
        cug1.html\
        cug-html.{4ct,4tc,aux,css,dvi,idv,lg,log,tmp,toc,xref}\
        cug-html*.html\
        cug-pdf.{lof,log,out,pdf,toc}\
        cug.pdf\
        cylc-version.txt\
        index.html\
        single

commands.tex: cylc-version.txt command-usage/help.txt
	{\
        echo "\label{help}";\
        echo "\lstinputlisting{command-usage/help.txt}";\
        echo "\subsection{Command Categories}";\
        for CAT in `$(CYLC) categories`; do\
        	echo "o $$CAT";\
        	$(CYLC) help $$CAT >> command-usage/$$CAT.txt;\
        	echo "\subsubsection{$$CAT}";\
        	echo "\label{$$CAT}";\
        	echo "\lstinputlisting{command-usage/$$CAT.txt}";\
        done;\
        echo "\subsection{Commands}";\
        for COMMAND in `$(CYLC) commands`; do\
        	echo "+ $$COMMAND";\
        	$(CYLC) $$COMMAND --help\
                | egrep -v '^DONE$$' >command-usage/$$COMMAND.txt;\
        	echo "\subsubsection{$$COMMAND}";\
        	echo "\label{$$COMMAND}";\
        	echo "\lstinputlisting{command-usage/$$COMMAND.txt}";\
        done\
    } >$@

command-usage:
	mkdir -p $@

command-usage/help.txt: command-usage
	$(CYLC) help >$@

cug.html: commands.tex
	htlatex cug-html.tex "cug-html.cfg,xhtml,3,next";
	mv cug-html.html $@
	rm -f cug-html.{4ct,4tc,aux,dvi,idv,lg,log,tmp,toc,xref}

cug1.html: commands.tex
	htlatex cug-html.tex "cug-html.cfg,xhtml,fn-in" ""
	mv cug-html.html $@
	rm -f cug-html.{4ct,4tc,aux,dvi,idv,lg,log,tmp,toc,xref}

cug.pdf: commands.tex
	pdflatex cug-pdf.tex;
	pdflatex cug-pdf.tex;
	pdflatex cug-pdf.tex;
	rm -f cug-pdf.{lof,log,out,toc};
	mv cug-pdf.pdf $@

cylc-version.txt:
	$(CYLC) -v >$@

index.html: cug.html cug1.html cug.pdf
	{\
        echo '<html>';\
        echo '<head><title>Cylc Documentation</title></head>';\
        echo '<body>';\
        echo '<h1>Cylc Documentation</h1>';\
        echo '<p><a href="cug.html">Multi-page HTML</a></p>';\
        echo '<p><a href="cug1.html">Single-page HTML</a></p>';\
        echo '<p><a href="cug.pdf">PDF</a></p>';\
        echo '</body>';\
        echo '</html>';\
    } >$@

html: html-multi html-single

html-multi: cug.html

html-single: cug1.html

pdf: cug.pdf