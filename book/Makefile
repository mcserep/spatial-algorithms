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

_JUPYTER_HTML=$(patsubst %, %.html, $(SRC))

.PHONY: all clean
all: $(_JUPYTER_HTML) index.html

%.html: %.ipynb
	jupyter nbconvert --to html $<
	sed -i 's/.ipynb"/.html"/' $@
	sed -i 's/.ipynb#/.html#/' $@

index.html: README.md
	pandoc -s README.md -t html -o index.html -T "Spatial algorithms with Python"
	sed -i 's/.ipynb/.html/' $@

clean:
	$(RM) *.html
