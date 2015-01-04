TW5-sitemap
===========

TiddlyWiki5 plugin to generate sitemap.xml (http://www.sitemaps.org/protocol.html).

It works in the browser but its primary use case is for generating a sitemap.xml for static sites
using something similar to the following when running TiddlyWiki under node.js:

```
tiddlywiki --rendertiddlers [!is[system]] statictiddler.html static text/plain  --rendertiddler sitemap sitemap.xml text/plain
```

The rest of this document presumes familiarity with TiddlyWiki5 (TW) and node.js (node).

## Setup

The following presumes you are running TW under node:

1. In the directory for your TW, if there is not already a directory named *plugins*, create one.
It should be a sibling of the *tiddlers* directory.
2. Clone or download the files in this project into a subdirectory of *plugins* called *sitemap*.
3. Change the URL in *sitemap/sitemapserver.tid* from `http://dullroar.com/` to your own site's URL. Note that
the ending `/` is required.
4. Change the *sitemap/tiddlywiki.info* file so that the `plugins` array has `dullroar/sitemap` in it, e.g.,
`"plugins": ["dullroar/sitemap"],`.

## Working With Tiddlers

`sitemap` uses the following TW filter to select tiddlers for the feed:

```
[!is[system]!tag[static]!title[Table of Contents]!sort[modifed]]
```

This basically says:

1. Do not include "system" tiddlers.
2. Do not include tiddlers with a tag of "static" (I use tiddlers tagged "static" to hold assets for
my static web site).
3. Do not include a tiddler entitled "Table of Contents".
4. Sort by modified date in descending order (currently there is a bug where this doesn't seem to be
happening).

So, the net of all that is that any tiddlers you create (and not tagged "static") will be placed in the
sitemap.

## Generating a sitemap.xml

To generate the sitemap for a static site, I use a command similar to (all on one line):

```
tiddlywiki --rendertiddlers [!is[system]!tag[static]] statictiddler.html static text/plain --rendertiddler $:/core/templates/static.template.css static/static.css text/plain --rendertiddler sitemap sitemap.xml text/plain 
```

If you want to use this in-browser, you can build a TW using the following command under node:

```
tiddlywiki --build index
```

The resulting *index.html* file in the *output* directory will have the plugin installed. You can navigate to
the *sitemap* tiddler under Shadows, or once you've opened the file with `index.html#sitemap`.

Or you can simply run the TW under node:

```
tiddlywiki --verbose --server 8080 $:/core/save/lazy-images text/plain text/html Jim
```

And then navigate directly to the feed tiddler using http://localhost:8080/#sitemap.

## Acknowledgments

Built for TiddlyWiki5 by Jeremy Ruston [@Jermolene], https://github.com/Jermolene/TiddlyWiki5, possibly
the coolest thing you can do with a browser.