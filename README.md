gonzalobulnes.com
=================

Powered by [Jekyll](http://jekyllrb.com/) and [Github](https://github.com)!

Usage
-----

This site uses [Jekyll Assets][jekyll-assets], therefore [can't be built automatically by the Github Pages engine](https://help.github.com/articles/pages-don-t-build-unable-to-run-jekyll#unsafe-plugins). The publication flow I prefer consits in creating a branch dedicated to publish the new post and push it to `master`.

```bash
# Publish the contents of add-article-using-git-to-go-forward

# checkout the post content
git checkout add-article-using-git-to-go-forward
# create a publication branch
git checkout -b publish-article-using-git-to-go-forward
# add the compiled site to the publiction branch
jekyll serve
git add _site
git commit -m "Publish article Using Git To Go Forward"

# push the branch to origin/master
git push --force origin publish-article-using-git-to-go-forward:master
```

This site also uses [Jekyll SASS][jekyll-sass], but `jekyll serve` knows how to deal with it.

  [jekyll-assets]: https://github.com/ixti/jekyll-assets
  [jekyll-sass]: https://github.com/noct/jekyll-sass
