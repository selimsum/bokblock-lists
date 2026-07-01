# bokblock-lists

Crowdsourced filter lists for the [Bokblock](https://github.com/selimsum/bokblock) browser
extension. These lists block AI-generated slop, content farms, and other low-quality YouTube
content.

## Available Lists

| List | Description | URL |
|------|-------------|-----|
| `ai-slop.txt` | English-language AI slop channels and title patterns | [Subscribe](#subscribing) |

## Subscribing

In the BlockTube extension options → **Filter Lists** tab, click **Add list** and enter:

```
https://cdn.jsdelivr.net/gh/selimsum/bokblock-lists@latest/ai-slop.txt
```

The extension checks for updates once daily and applies them automatically.

## Contributing

If you find an AI slop channel on YouTube, right-click the channel and select
**Report as AI Slop** in the BlockTube context menu. Your submission will be reviewed
by a maintainer and added to the appropriate list.

See [FORMAT.md](FORMAT.md) for the full rule format specification.

## Adding a Regional List

To host a regional list (e.g. for Turkish, Spanish, or other language content):

1. Fork this repository (or create a new one following the same structure).
2. Add your `ai-slop-XX.txt` file following the format in [FORMAT.md](FORMAT.md).
3. The GitHub Actions workflow will publish daily releases automatically.
4. Users subscribe by entering your jsDelivr URL in the Filter Lists tab.

## License

[GPLv3](LICENSE)
