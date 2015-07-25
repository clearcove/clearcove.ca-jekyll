# Migrate from WordPress to Jekyll

I tried two approaches:

* site1: export directly from Wordpress to jekyll using
  https://github.com/benbalter/wordpress-to-jekyll-exporter
  This approach worked great for images and posts, however it didn't get all
  the pages.
    * overall page layout looked better. It brought in links to the existing pages.
    * the directory structure for attachments mimics WP's exactly which makes
      for a messy dir structure.
    * pages are in the root directory
* site2: export Wordpress XML, import with jekyll-import
      ruby -rubygems -e 'require "jekyll-import"; JekyllImport::Importers::WordpressDotCom.run({ "source" => "/Users/johund/development/clearcove.ca-history/clearcovesoftwareinc.wordpress.2015-06-23.xml" })'
    * This one handled a lot of pages, however it wasn't perfect either.
    * I'll have to manually merge the two imports.
    * the rendered pages had broken characters and markup. Pretty much unusable.
    * I'll just use this to capture the missing pages and merge them into site.

## Site development

I develop the final site in the `site` directory.
