---
title: Your first seed
---

- Using the note title: [[a note about cats]]
- Using the note's filename: [[cats]]
- Using the note's title, with a label: [[A note about cats|link to the note about cats using the note title]]
- Using the note's filename, with a label: [[cats|link to the note about cats using the note's filename]]


Footnotes are also supported and will be treated like internal links.[^1] You can point to other notes in your footnotes.[^2]

[^1]: This is a footnote. For more information about using footnotes, check out the [Markdown Guide](https://www.markdownguide.org/extended-syntax/#footnotes).
[^2]: This is another footnote that links to the note about [[cats]]. You may also point to [[notes that do not exist]] if you wish.

### Site configuration

Some behavior is configurable by tweaking the `_config.yml` file.

**`use_html_extension`**: if you use a static host that doesn't support URLs that don't end with `.html` (such as Neocities), try changing the `use_html_extension` value to `true` in the `_config.yml` file and restart the Jekyll server (or re-build the site). This adds a `.html` extension to note URLs and may resolve issues with links. If you're still having trouble, I recommend using Netlify to host your digital garden: it's free, easy to use, and fully supports this template's features out of the box.

**`open_external_links_in_new_tab`**: when set to `true`, this makes external links open in new tabs. Set to `false` to open all links in the current tab.

**`embed_tweets`**: when set to `true`, tweet URLs on their own lines will be replaced with a Twitter embed. Default value is `false`.

### Automatic bi-directional links

Notice in the "Notes mentioning this note" section that there is another note linking to this note. This is a bi-directional link, and those are automatically created when you create links to other notes.


### Images and other Markdown goodies

Finally, because you have the full power of Markdown in this template, you can use regular Markdown syntax for various formatting options.


If you'd like to quote other people, consider using quote blocks:

> Lorem ipsum dolor sit amet

You can also ==highlight some content== by wrapping it with `==`.

Non-latin languages are supported too: ==你好==, ==안녕하세요==, ==こんにちは==.