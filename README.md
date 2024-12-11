
# Star HPC user documentation

Served via https://docs.starhpc.hofstra.io

Copyright (c) 2024
Hofstra University


## License

Text is licensed under [CC-BY 4.0](https://creativecommons.org/licenses/by/4.0/),
code examples are provided under the [MIT](https://opensource.org/licenses/MIT) license.


## Locally building the HTML for testing

### Install build tools and dependencies.

<details> 
	<summary>Liquid 4.0.3</summary>

> [!WARNING]
> If you have the latest dependencies installed, the following does not apply anymore.
> 
> The original `jekyll-rtd-theme` 2.0.10 required `github-pages` 209, which effectively capped the version of Liquid to 4.0.3.
> Due to Liquid 4.0.3 and older not being updated to work with Ruby 3.2.x, Ruby 3.1.x or older was required for Liquid 4.0.3.
> https://talk.jekyllrb.com/t/liquid-4-0-3-tainted/7946/18
> 
> #### With Cygwin
> As of 8/8/2024, Cygwin provided Ruby versions 2.6.4-1 and 3.2.2-2. You would need to make sure to install the former. As the version of bundler supplied with Ruby 2.6 is too old and the version of RubyGems is too new, the correct versions of RubyGems and bundler would need to be installed manually after installing all the other dependencies:
> ```
> gem update --system 3.2.3
> gem install bundler -v 2.1.4
> 
> # confirm the correct version is installed
> bundler -v
> ```

</details>

To allow building of native extensions, install `ruby-devel`, `gcc`, and `make`.

Install `libxml2`, `libxml2-devel`, `libxslt`, `libxslt-devel`, `libiconv`,
and `libiconv-devel`. These are all dependencies of the `nokogiri` gem, which
is built from source.

Also, at the time of this writing, `libffi-devel` is needed to build ffi:
```
  jekyll-rtd-theme was resolved to 2.0.10, which depends on
    github-pages was resolved to 209, which depends on
      github-pages-health-check was resolved to 1.16.1, which depends on
        typhoeus was resolved to 1.4.1, which depends on
          ethon was resolved to 0.16.0, which depends on
            ffi
```

`bundler` is used for dependency management and installation of gems, based on the
content of `Gemfile`. If you run into any issues with gem depencencies, you may
want to try running `bundle update` or removing `Gemfile.lock` and then running
`bundle install` again.

If you run into trouble building `nokogiri`, try building it with the system libraries instead:

```
gem install nokogiri -v '1.17.1' --verbose -- --use-system-libraries
```

### Building the site

```
git clone https://github.com/starhpc/docs.git star-docs
cd star-docs
gem install bundler
bundle config set --local path ~/.bundler  # Optionally specify where to install gems (SO Q&A #8257833).
                                           # Otherwise, bundler may attempt to install gems system-wide,
                                           # e.g. /usr/share/gems, depending on your GEM_HOME
                                           # (see SO Q&A #11635042 and #3408868).
bundle install
bundle exec jekyll serve
```

Then point your browser to `http://localhost:4000`.

Alternatively, run `bundle exec jekyll build` to build the static site without starting a local web server.


## Getting started with Markdown (kramdown), Jekyll, and GitHub Pages

- https://kramdown.gettalong.org/quickref.html
- https://jekyllrb.com/docs/
- https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll
