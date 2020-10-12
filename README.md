# CASAdocs
Common Astronomy Software Applications Documentation


## Editing Regular Content
Most of the casadocs content is written in markdown format using the Google
Colab web service to edit Jupyter notebooks of text cells.  Jupyter notebook
pages have a link at the top to "Open in Colab" for editing.  Modified pages
can be saved back in to the casadocs repository from the Colab -> File menu.  

## Editing API Content
API content is generally created by combining xml from the CASA source code
repository with ReStructuredText (rst) files held in the casadocs repository.
The xml can only be updated through development branches of the source code,
while the rst files can be edited directly in the Github repository browser
window.


## Generating from Scratch
This should not be necessary and his here only for reference on how
to regenerate the original content from Plone.

1. Scrape the latest Plone CASAdocs (creates html folder):
   ```
   $: scrapy crawl sitemap
   $: docker run -p 8050:8050 scrapinghub/splash
   $: scrapy crawl full
   ```

2. Generate content pages (creates markdown folder)
   ```
   $: python scripts/convert_html.py
   ``` 

3. Generate notebook files (creates docs/notebooks folder)
   ```
   $: python scripts/build_notebooks.py
   ```

4. Locally build pages to verify (this will also call scripts/parse_task_xml.py)
   ```
   $: cd docs
   $: sphinx-build -a -E -b html . ./build
   ```



