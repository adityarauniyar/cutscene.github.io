# cutscene.github.io



---

#### Local Setup (Legacy)

Assuming you have [Ruby](https://www.ruby-lang.org/en/downloads/) and [Bundler](https://bundler.io/) installed on your system (*hint: for ease of managing ruby gems, consider using [rbenv](https://github.com/rbenv/rbenv)*), and also [Python](https://www.python.org/) and [pip](https://pypi.org/project/pip/) (*hint: for ease of managing python packages, consider using a virtual environment, like [venv](https://docs.python.org/pt-br/3/library/venv.html) or [conda](https://docs.conda.io/en/latest/). If you will use only `jupyter`, you can use [pipx](https://pypa.github.io/pipx/)*).

```bash
$ cd cutscene.github.io
$ bundle install
# assuming pip is your Python package manager
$ pip install jupyter
$ bundle exec jekyll serve --lsi
```

Now, feel free to customize the theme however you like (don't forget to change the name!).
After you are done, **commit** your final changes.

---