# In Lieu of Fun

This repository contains the website for the [In Lieu of Fun](https://inlieuof.fun) webcast.

- If you have a question or want to report a problem, please consider [filing an issue][issues].
- If you are looking for the webcast itself, please visit https://inlieuof.fun.
- This repository: http://repo.inlieuof.fun
    - Site build status: http://build.inlieuof.fun
- The tools repository: http://tools.inlieuof.fun

[issues]: http://issues.inlieuof.fun

## Site Data

Episodes are stored in Markdown files in the `_episodes` directory, with each
file named like `YYYY-MM-DD-NNNN.md`. Here `NNNN` is the episode number, padded
with zeroes on the left (e.g., `0014`, `0143`). Each file has a YAML front
matter section giving episode metadata:

 - `episode`: The episode number [integer; required]
 - `date`: The date when the episode aired [string, "YYYY-MM-DD"; required]
 - `youtube`: The URL of the episode stream on YouTube [string]
 - `crowdcast`: The URL of the episode stream on Crowdcast [string]
 - `summary`: A brief summary of the episode [string, optional]. If this is
   provided, it is shown in the Episode Log.
 - `topics`: A comma-separated list of topics [string, optional]. If this is
   provided and there is not a summary, it is shown in the Episode Log.
 - `links`: A list of related hyperlinks [optional]. Shown on the Episode Log.
 - `special`: If true, cosmetically flag this episode as interesting in the Episode Log.

The body of the episode is arbitrary markdown text that will be displayed on
the episode detail page. Episode detail pages are rendered using the layout
defined in [`_layouts/episode.html`](./_layouts/episode.html). The episode log
table is rendered by the Liquid template in [`episodes.html`](./episodes.html).

To add a new episode, create a new file in the `_episodes` directory following
the format of the existing files, and update the guest list as necessary.

Guests are recorded in [`_data/guests.yaml`](./_data/guests.yaml), and the
guest list is rendered by the Liquid template in [`guests.html`](./guests.html).


## Updates

The site content is generated with [Jekyll](https://jekyllrb.com).  To test
changes locally, run `bundle exec jekyll serve` and visit the local server in
your browser. You can leave this running while you work, it will detect file
changes and rebuild as needed. The only caveat is that if you edit the root
`_config.yml` file, you will need to restart it.

### First-Time Setup

After cloning the repository for the first time, you will need to install the
toolchain, which is not checked in. This is, roughly:

```bash
# One-time setup instructions for a fresh clone.

# Install Jekyll and Bundler. On macOS, you may want to include --user-install
gem install jekyll bundler

# Install vendored gems.
git clone https://github.com/inlieuoffun/inlieuoffun.github.io.git ilof
cd ilof
bundle install
```

### Daily Updates

To simplify the process of setting up new episodes, we have written a
[command-line tool that primes a new episode][epdate] from its announcement on Twitter.
To use it you need to have (or install) [Go](https://golang.org), then run:

[epdate]: https://github.com/inlieuoffun/tools/tree/default/epdate

```shell
go install github.com/inlieuoffun/tools/epdate
```

To use the tool, you will need:

- A [Twitter API v2 bearer token](https://developer.twitter.com/en/portal/dashboard),
  in the environment variable `TWITTER_TOKEN`.

- A [YouTube Data API key](https://console.developers.google.com/apis/credentials),
  in the environment variable `YOUTUBE_API_KEY`.

Once you have these set up, change directory into a clone of this repository
and run:

```shell
epdate -edit
```

If there are new episodes scheduled, this will create one or more new files in
the `_episodes` directory and update `_data/guests.yaml`. Inspect these and
correct any errors or other missing data (the tool is imperfect), then commit
and push them up to GitHub.


## URL Structure

- Episode Log (HTML): https://inlieuof.fun/episodes.html
    - Episode Log (JSON): https://inlieuof.fun/episodes.json
    - Latest episode (JSON): https://inlieuof.fun/latest.json
    - Episode N (JSON), e.g., N=25: https://inlieuof.fun/episode/25.json
- Guest List (HTML): https://inlieuof.fun/guests.html
    - Guest List (JSON): https://inlieuof.fun/guests.json
- Episode N stream redirect (ex. N=25): https://inlieuof.fun/stream/25
    - Latest episode stream redirect: https://inlieuof.fun/stream/latest
- Episode N detail redirect (ex. N=25): https://inlieuof.fun/episode/25
- Merch store redirects: https://inlieuof.fun/merch, https://inlieuof.fun/store

## Peculiarities

Because this site is hosted from a github.io repository, the generated site
data must be published from the branch named `master`. Thus, this repository
uses the `source` branch as its default branch.
