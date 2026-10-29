.PHONY: all clean

all: regae-draft-v0.3.pdf

regae-draft-v0.3.pdf: main.tex references.bib cvpr.sty ieeenat_fullname.bst $(wildcard sections/*.tex) $(wildcard figures/*)
	latexmk -pdf -interaction=nonstopmode -halt-on-error main.tex
	cp main.pdf regae-draft-v0.3.pdf

clean:
	latexmk -c main.tex
