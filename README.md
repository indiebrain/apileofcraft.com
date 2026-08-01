# A Pile of Craft

A static blog about the things made by the people on one street. Built with
[Hugo](https://gohugo.io/) using Org-mode content, deployed to GitHub Pages at
[apileofcraft.com](https://apileofcraft.com/).

## Running locally

Requires Hugo **extended** (0.164 or newer):

```bash
hugo server --buildDrafts
```

Then open <http://localhost:1313/>.

## Writing a post

Each post is a *page bundle* — a folder holding the writing and its media
together. Create one with:

```bash
hugo new posts/the-thing-you-made/index.org
```

That folder is where the post's images and video live, right next to the text.
Reference them by filename (no path needed).

### Front matter

Front matter uses Org keywords at the top of the file:

```org
#+TITLE: The Corner Birdhouse
#+DATE: 2026-07-28
#+DRAFT: false
#+DESCRIPTION: One sentence for search results and social cards.
#+MAKERS[]: Rosa_Delgado
#+TAGS[]: woodworking outdoors
#+COVER: cover.jpg
#+COVERALT: A plain-language description of the cover image, for screen readers.
#+COVERCAPTION: An optional visible caption under the cover.
```

Notes:

- **`#+MAKERS[]:` and `#+TAGS[]:`** are *arrays*. The `[]` matters — values are
  split on spaces. Join a multi-word maker name with underscores
  (`Rosa_Delgado`); the site shows it with spaces ("Rosa Delgado") and gives
  each maker their own page at `/makers/rosa_delgado/`.
- **`#+COVERALT:`** is required whenever you set a cover. Every meaningful image
  needs alternative text — it is both an accessibility requirement and good
  manners.

### Adding images and video in the body

Images use ordinary Org links, with alt text via an attribute:

```org
#+CAPTION: A visible caption.
#+ATTR_HTML: :alt Screen-reader description of the image.
[[file:detail.jpg]]
```

Video needs a small HTML block (Org has no native video syntax). Always give a
fallback for browsers that can't play it:

```org
#+BEGIN_EXPORT html
<figure>
  <video controls preload="none" width="720" poster="cover.jpg">
    <source src="clip.mp4" type="video/mp4">
    <p>Your browser can't play this video.
       <a href="clip.mp4">Download the clip</a> instead.</p>
  </video>
  <figcaption>What the clip shows.</figcaption>
</figure>
#+END_EXPORT
```

Keep video files small and web-encoded:

```bash
ffmpeg -i source.mov -c:v libx264 -pix_fmt yuv420p -movflags +faststart clip.mp4
```

## Deploying

Push to `main`. The workflow in `.github/workflows/deploy.yml` builds the site
with Hugo and publishes it to GitHub Pages. The custom domain is set by
`static/CNAME`.

One-time GitHub setup: in the repository, go to **Settings → Pages** and set the
source to **GitHub Actions**, then add `apileofcraft.com` as the custom domain
and point the domain's DNS at GitHub Pages.

## Accessibility

The site targets **WCAG 2.1 AAA**. Practically, that means: describe every
image, don't rely on color alone, keep contrast high, and preserve visible
keyboard focus. The templates handle the structure; authors are responsible for
alt text and honest captions.
