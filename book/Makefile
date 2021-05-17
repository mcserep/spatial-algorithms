SRC=01_python_intro \
	02_operations_conditions \
	03_iterations_lists \
	04_functions \
	05_basic_algorithms \
	06_sorting \
	07_collections \
	08_oop 09_tabular \
	10_plotting \
	11_spatial_vector \
	12_spatial_raster \
	13_graph_representation \
	14_graph_shortest_path \
	15_graph_spanning_tree \
	16_spatial_indexing \
	17_convexhull \
	18_clustering \
	AX01_strings \
	AX02_math \
	exercise_01_solutions \
	exercise_02_solutions \
	exercise_03_solutions \
	exercise_04_solutions \
	exercise_05_solutions

_WEB_HTML=$(patsubst %, web/%.html, $(SRC))
_PDF_HTML=$(patsubst %, pdf/%.html, $(SRC))
_PDF=$(patsubst %, pdf/%.pdf, $(SRC))

ATHENAPDF=docker run --rm -v $(CURDIR)/../:/converted/ --security-opt seccomp:unconfined \
	arachnysdocker/athenapdf athenapdf -D 1000

## RULES: all, web, book, chapters, clean

.PHONY: all
all: book web

.PHONY: book
book: pdf/book.pdf

.PHONY: chapters
chapters: $(_PDF)

.PHONY: web
web: $(_WEB_HTML) web/index.html

.PHONY: clean
clean:
	$(RM) *.html
	$(RM) *.pdf
	$(RM) web/
	$(RM) pdf/

.SECONDARY:
web/%.html: %.ipynb
	mkdir -p web
	jupyter nbconvert --to html --template classic --output-dir="./web" $<
	sed -i 's/\.ipynb\"/.html"/' $@
	sed -i 's/\.ipynb\#/.html#/' $@
	sed -i 's|\.\./images/|../../images/|' $@

.SECONDARY:
pdf/%.html: %.ipynb
	mkdir -p pdf
	jupyter nbconvert --to html --template classic --output-dir="./pdf" $<
	sed -i 's/\.ipynb\"/.pdf"/' $@
	sed -i 's/\.ipynb\#/.pdf#/' $@
	sed -i 's|\.\./images/|../../images/|' $@

.SECONDARY:
pdf/%.pdf: pdf/%.html
	mkdir -p pdf
	$(ATHENAPDF) book/$< book/$@

web/index.html: README.md
	pandoc -s $< -t html -o $@ -T "Spatial algorithms with Python"
	sed -i 's/\.ipynb/.html/' $@

pdf/index.html: README.md
	pandoc -s $< -t html -o $@ -T "Spatial algorithms with Python"
	sed -i 's/\.ipynb/.pdf/' $@

pdf/00_toc.pdf: pdf/index.html
	mkdir -p pdf
	$(ATHENAPDF) book/$< book/$@

pdf/00_cover.pdf: 00_cover.tex
	pdflatex -output-directory pdf $< 

pdf/book.pdf: pdf/00_cover.pdf pdf/00_toc.pdf $(_PDF)
	pdfunite $^ $@
