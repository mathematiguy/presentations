# This Makefile automates routine tasks for this Singularity-based project.
IMAGE ?= container.sif
RUN ?= singularity exec $(FLAGS) $(IMAGE)
SINGULARITY_ARGS ?=
FLAGS ?=  -B $$(pwd):/code --pwd /code
VENV_PATH ?= venv

.PHONY: run container shell sandbox sandbox-shell

report: presentation/presentation.pdf
presentation/presentation.pdf: presentation/presentation.tex
	$(RUN) bash -c "(cd $(dir $@) && xelatex $(notdir $<) && xelatex $(notdir $<) && xelatex $(notdir $<))"

clean:
	rm presentation/*.aux presentation/*.nav presentation/*.pdf presentation/*.log presentation/*.out presentation/*.snm presentation/*.toc texput.log

# Builds a Singularity container from the Singularity definition file.
# Note: This command requires sudo privileges.
container: $(IMAGE)
$(IMAGE): Singularity
	sudo singularity build --force $(IMAGE) $(SINGULARITY_ARGS) Singularity

# Starts a shell within the Singularity container, with the virtual environment activated.
shell:
	singularity shell $(FLAGS) $(IMAGE) $(SINGULARITY_ARGS) bash

sandbox: sandbox.sif
sandbox.sif: $(IMAGE)
	sudo singularity build --force --sandbox sandbox.sif $(IMAGE)

sandbox-shell: sandbox.sif
	sudo singularity shell --writable sandbox.sif
