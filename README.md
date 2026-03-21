# My Website

My website is deployed using [Hugo](https://gohugo.io/) and uses the [Gokarna](https://gokarna-hugo.netlify.app/) theme.
The [deployment script](.github/workflows/hugo.yaml) was obtained [here](https://gohugo.io/host-and-deploy/host-on-github-pages/#step-4).

## How To Update

### Homepage

The text in the main page is edited in the `hugo.toml` file.

### Pages and Posts

For this project, all the content exists in the `content` folder. Any pages are in `content/pages` and any posts are in `content/posts`.
The directory structure is as follows:

```bash
content
├── pages
│   └── about.md
└── posts
    ├── 2024-05-03-hello-world.md
    └── 2026-03-20-hello-world2.md
```
