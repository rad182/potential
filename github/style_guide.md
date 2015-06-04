### Style Guide

We do not currently have a strictly enforced 'Git Style' but trying to learn and implement a consistent
approach can and will make things easier for yourself and your fellow developers. Following are some
suggestions pulled from popular practices among Artsy developers, the community and Agis Anastasopoulos' [Git Style Guide](https://github.com/agis-/git-style-guide
).

#### Branches

* Choose short and descriptive names.
* Use identifiers from Github ( issue numbers )
* Use dashes to separate words

````
# good
2673-fair-header-links-to-fair-fix

# bad
fair-link
````

#### Commits

* A commit should be a single, logical change. If you were adding a feature to an
application the commit would include its tests and anything else needed to make applying the commit
introduce the feature completely.
* Commits should be ordered logically. If you want to implement a new feature but have to fix a bug
to add the feature, commit the bug fix first and then the feature.
* If while working you find yourself with several messy commits attempt to ````rebase```` them into
a single, logical commit before attempting to push upstream. Example:
````
# Before rebase
08877527 Add Email Input Field to Artworks Page
4044a23f Spelling Errors Bug Fix
e45ca5p3 Writing Tests
355297a0 c0d3 and things so tired

# After rebase
08877527 Add Email Input Field to Artworks Page
````
#### Pull Requests

* When possible, try to include screenshots of the feature being introduced / changed. If it requires
some level of interaction, try to include a gif of it being used - [Licecap](http://www.cockos.com/licecap/)
is excellent for quickly recording gifs of your screen.
* You may notice that when you run ````git diff```` you'll see '[No Newline at End of File](https://robots.thoughtbot.com/no-newline-at-end-of-file).' Maintaing new lines at the end of files is not vital but will help reduce noise on
Github.
* The Github blog has a nice article on [writing quality pull requests](https://github.com/blog/1943-how-to-write-the-perfect-pull-request).


> If you use sublime text, you can add  ````"ensure_newline_at_eof_on_save": true```` to your user configruation
file and this will be done automatically when you save.
