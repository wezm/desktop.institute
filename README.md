# Desktop Institute

This is the source of [desktop.institute], a blog of sorts run by [Wesley Moore].
If you've found a typo, broken link, or anything else that needs updating, feel
free to let me know or send a PR.

## Contributing

desktop.institute is built with [Zola]. To build the site, [install Zola], then
run `zola serve`.

## License

All rights reserved.

## Random Notes

To generate a front-matter date field:

    :r! ruby -rtime -e "puts File.mtime('%:r.md').xmlschema"

[desktop.institute]: https://desktop.institute/
[Wesley Moore]: https://www.wezm.net/about/
[install Zola]: https://www.getzola.org/documentation/getting-started/installation/
[Zola]: https://www.getzola.org/
