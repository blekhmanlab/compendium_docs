![Human Microbiome Compendium logo](http://blekhmanlab.org/images/compendium.png "Human Microbiome Compendium")

# Documentation website

Sphinx documentation for the [Human Microbiome Compendium](https://microbiomap.org)

To generate a local copy of the docs in the "html" directory:

```sh
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r docs/requirements.txt
python -m sphinx -T -W --keep-going -b html -d _build/doctrees -D language=en docs/source ./html
```
