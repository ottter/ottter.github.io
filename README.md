# My Personal Website

I figured that blogging some of my personal projects and generally what I get up to would be a
 good motivator to be more productive in my free time and to improve at what I love. There is
 no set theme, purpose, or timeframe for updates.

## Getting Started

```terminal
git clone https://github.com/ottter/ottter.github.io.git
sudo apt install hugo
```

## Make a New Post

1. Create the file: `hugo new posts/example-post-name.md`
2. Edit the post: `vi content/posts/example-post-name.md`
3. Check locally: `hugo server -D` or just publish: `hugo`
4. Publish to github: `git push`

## Troubleshooting

Debug by running: `hugo -v --debug -D`

Issue:\
`WARN found no layout file for "HTML" for kind "page":`\
Solution:\
`git submodule init && git submodule update`

## Tools used

* [Hugo](https://gohugo.io/) framework (with [cactus](https://themes.gohugo.io/hugo-theme-cactus/) theme)
* Github Pages && Actions
