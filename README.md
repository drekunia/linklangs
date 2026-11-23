# linklangs

A multilingual personal homepage template for Jekyll. Give your bio and links their own copy in each language, with a shareable URL for each version.

[Live demo](https://drekunia.github.io/linklangs/)

## Create your site

1. Click **Use this template → Create a new repository** on GitHub. A public repository works with GitHub Pages on GitHub Free.
2. Edit `_config.yml` to set your title, profile, and links. Update `_data/languages.yml` for the languages you want to offer. Replace the sample images in `assets/img`.
3. In your repository, go to **Settings → Pages** and select **GitHub Actions** as the source. Then open **Actions → Build and Deploy → Run workflow**.

## Development

Use Ruby 3.4, as specified in [mise.toml](mise.toml).

```sh
bundle install
bundle exec jekyll serve
```

Open [localhost:4000](http://localhost:4000). Restart the server after editing `_config.yml` to load the new configuration.

To preview a site served under a repository path, run `bundle exec jekyll serve --baseurl /my-repo` and open `http://localhost:4000/my-repo/`.

To build the site into `_site` without starting a server:

```sh
bundle exec jekyll build
```

## Site configuration

Edit [_config.yml](_config.yml) for the site title, description, profile, and links. The site requires a non-empty `title` string or at least one non-empty title in a language map. The description, author block, individual author fields, and links list are optional.

```yaml
default_language: en

title:
  en: "Alex’s homepage"
  ja: "アレックスのホームページ"

author:
  name: Alex Morgan
  title:
    en: Writer and developer
    ja: ライター・開発者
  avatar: /assets/img/avatar.webp

links:
  - name:
      en: Blog
      ja: ブログ
    url: https://example.com/blog
  - url: "https://example.com/guide.html?from=profile"
```

`title`, `description`, `author.name`, `author.title`, `author.bio`, and each link's `name` accept either a string or a map keyed by language code.

- A plain string or a one-entry map is shared across modes.
- For a map with multiple entries, the site tries the selected language, including compatible language tags, then English.
- If neither matches, the site title uses the first non-empty title. Optional profile fields and descriptions are omitted. The copyright line uses the site title if the author name is unavailable.
- A link with no usable name gets a label from its URL. For example, the last link above displays as `example.com/guide`. The generated label omits query parameters, leading and trailing slashes, and common file extensions. The link still opens the full URL.

The site skips link entries with a blank or missing `url`. A link section with no visible entries takes up no space. Links open in a new tab by default. Set `same_tab: true` to open in the current tab, `bold: true` to emphasize the label, or `hidden: true` to keep the link out of the visible list.

## Languages

Add or reorder language codes in [_data/languages.yml](_data/languages.yml). The dropdown follows the file order and displays each code as written. An entry can be as small as `en: {}`.

| Optional field | Use |
| --- | --- |
| `name` | Accessible name for the language option |
| `switcher` | Accessible label for the dropdown |
| `locale` | Open Graph locale, such as `en_US` or `ja_JP` |
| `error_heading` | 404 page heading |
| `error_message` | 404 page message |
| `return_home` | Label for the 404 page’s home link |
| `rights` | Extra notice after the copyright line |

Missing 404 and switcher text falls back to English. An empty `rights` field leaves just the copyright line. The root homepage uses its default language’s rights notice, while a localized homepage or the 404 page uses its own language’s notice.

- With no entries, the site uses English. With one entry, it uses that language. Both cases use a single homepage without a dropdown.
- With at least two valid language codes, Jekyll generates static homepages such as `/en/`, `/ja/`, and `/id/` from the same template. Each has localized metadata, a canonical URL, and reciprocal `hreflang` links. The canonical language URLs appear in the sitemap, and `/` is the `x-default` URL.
- A direct visit to a language URL stays in that language. On `/` and the 404 page, the site checks the visitor’s saved choice, then their browser preferences, then `default_language`. If the configured default is unavailable, it uses the first listed language.
- The homepage dropdown navigates to the selected language URL and saves the choice. The 404 dropdown changes the error page in place. The 404 template uses `noindex, follow`.

## Deployment

The [Build and Deploy workflow](.github/workflows/pages-deploy.yml) builds the site in production mode with the custom Jekyll plugins and publishes the result to GitHub Pages. It runs on site changes pushed to `main` and can also be triggered manually. GitHub Pages provides the published origin and base path, including when you configure a custom domain through Pages.

For another host, set `url` to the origin (for example, `https://example.com`) and `baseurl` to the hosting path (for example, `/homepage` or an empty string for the root). Build with `JEKYLL_ENV=production bundle exec jekyll build` and upload the contents of `_site`.

## License

See [LICENSE](LICENSE).
