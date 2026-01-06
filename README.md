# COMP 127 Course Web Site

Source for [this live site](https://comp127.innig.net).

(Note that the domain `comp127.innig.net` redirects to the current semester. You can use this to create long-term external permalinks to the site, e.g. from assignment repositories.)


## Building the site

### Local build / development

Install a Ruby manager if you don’t already have one. Either [rbenv](https://github.com/rbenv/rbenv) or [rvm](https://rvm.io) should (eventually) work. (As of this writing (2025), rvm is somewhat more straightforward to install and use, but has trouble building recent Ruby versions on macOS machines with Apple Silicon chips.)

Inspect `.ruby-version` in this repository, then install the appropriate version of Ruby using rbenv/rvm.

Then, in your local clone of this repository:

    bin/run-dev

This will spin up a local web server at [http://localhost:3000](http://localhost:3000). The server will automatically rebuild the site when you save changes, and your browser should automatically reload those changes as you leave the server running.

> Note that the data transformers register several possible problems (such as assignments that are never assigned) while still allowing the site to build. These warnings, which appear in the lower right of all the pages, only show up when the server running in dev mode; they _not_ be visible to students on the live site.

Assignments can be “locked” with a `locked: true` in the Markdown front matter. Students can see the titles of locked assignments, but there are no links to them and no assignment pages in the output. To work on those locked assignments, use:

    bin/run-unlock-all-assignments

To see what students will see on a future date, e.g. to preview what assignments will be available on the next day of class, use:

    bin/run-with-fake-today 2063-04-05   # preview specific date
    bin/run-with-fake-today +2           # n days in the future

#### Building/running on Ubuntu Linux

Remember to get the `rbenv` shell function defined: ensure that

```
eval "$(rbenv init - --no-rehash zsh)"
```

gets run in your shell. You'll want to install the Ruby version matching
`.ruby-version`; or, if you use a different version with `rbenv install
<version>`, edit that file to match your version and hope for the best.

Then do the `bundle install` stuff.

### Deploying to production

Push to the `main` branch on Codeberg to deploy to production. (The Codeberg repo is configured to call a webhook that triggers a build on Paul’s personal Linode. A push should update the live site within ~10 seconds.)

The prod server also automatically rebuilds the site periodically (currenly every half an hour) in order to pick up new Google Calendar events.

There is currently no way to view live site build errors without admin access to the Linode. Sorry!


## Project structure

Course content lives in `src/data/`. Presentation and style for the data live in `src/presentation/`.

Documentation TODOs:

- quick overview of Superfluous architecture
- notes on structure of course data
- special markdown processing
- GDoc importer
