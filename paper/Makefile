.DEFAUT_GOAL := pdf_draft
PANDOC=pandoc
LATEX2PDF=pdflatex
BIBER=biber

pdf_draft: diagram appendices
	$(PANDOC) draft.yaml paper.md -s -o paper.tex --include-after-body=appendices.tex --filter=pandoc-crossref --filter=pandoc-citeproc --biblatex
	$(LATEX2PDF) paper.tex
	$(BIBER) paper.bcf
	$(LATEX2PDF) paper.tex

appendices: appendices.md
	$(PANDOC) appendices.md -o appendices.tex --filter=pandoc-crossref --filter=pandoc-citeproc --biblatex

diagram: diagram.tex
	$(LATEX2PDF) diagram.tex
	mv diagram.pdf gfx/figure1.pdf

arxiv_tar: diagram appendices
	$(PANDOC) arxiv.yaml paper.md -s -o paper.tex --include-after-body=appendices.tex --filter=pandoc-crossref --filter=pandoc-citeproc --biblatex
	mkdir -p arxiv
	cp -r paper.tex arxiv/.
	cp -r table.tex arxiv/.
	cp -r gfx arxiv/.
	cp -r bibliography.bib arxiv/.
	cp nips_2018.sty arxiv/.
	$(LATEX2PDF) paper.tex
	$(BIBER) paper.bcf
	mv paper.aux arxiv/.
	mv paper.bbl arxiv/.
	tar -czvf arxiv.tar.gz arxiv

arxiv_pdf: arxiv_tar
	cd arxiv; $(LATEX2PDF) paper.tex
