# How to Generate the HTML Version of the Documentation

If you want to generate the HTML version of the documentation on your local machine, you need to have Sphinx and some Sphinx-contrib packages installed. To do that, go to a subdirectory of `docs` (e.g. `docs/protocol`) and do:
```bash
pip install -r requirements.txt
```

You can then generate the HTML documentation _in that subdirectory_ by doing:
```bash
make html
```

It should tell you where the generated documentation (HTML files) can be found. You can view it in your web browser.
