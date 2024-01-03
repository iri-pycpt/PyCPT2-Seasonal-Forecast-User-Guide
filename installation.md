# Installing PyCPT 2.5

PyCPT 2.5 can be installed on Windows, Linux, and Mac computers by following [these instructions](https://iri-pycpt.github.io/installation/). 
The main steps are: 
1. Download and '''install''' the free [Miniconda](https://docs.conda.io/projects/miniconda/en/latest/) software package for your platform 
2. Download the [notebook and python environment files needed for Windows/Mac/linux](https://iri-pycpt.github.io/installation/#pycpt-quickstart) from the IRI
3. Move the two files (notebook and environment file) to your working directory.

The final two steps will be run from the command line, e.g., using the Anaconda App prompt on Windows.   

4. Create and activate the pycpt environment. Type these in and hit return after each one.  For Windows this looks like:

```python
conda create -n pycpt_environment --file conda-win-64.lock
conda activate pycpt_environment
```

5. Finally, type `Jupyter Notebook` and hit return, which brings up the PyCPT Jupyter Notebook web browser PyCPT interface

```python
jupyter notebook
```

The full software documentation can be found here: [https://iri-pycpt.github.io](https://iri-pycpt.github.io).











     