gonzalo-bulnes.github.io
========================

A simple blog powered by [Jekyll](http://jekyllrb.com/) and [Github Pages](https://pages.github.com).

Getting started locally
-----------------------

Install both [Bundler][bundler] and [Bower][bower], and proceed to install all dependencies:

```bash
# install Jekyll
bundle install

# install the vendored assets
bower install
```

Then start you local server: `jekyll serve`

About the vendored assets
-------------------------

The vendored assets are under version control to allow them to be imported from `css/main.scss`. (The Github Pages won't `bower install` before attempting to build.)

Be sure to update them each time you update the bower manifest (`bower.json`). If you don't, versions will be inconsistent. Also, be sure to update them in a single, indpeendent commit so they don't mask any significant change in you code.

About HTTPS
-----------

Full HTTPS [cannot be enabled][full-https] with [custom domains][cname], that's why I disabled the `CNAME`.

  [cname]: https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages/
  [full-https]: https://github.com/isaacs/github/issues/156#issuecomment-110490178

License
-------

Copyright &copy; 2015 Gonzalo Bulnes Guilpain. [All rights reserved](LICENSE).
