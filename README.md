# Shivanandi Devotional Songbook

Shivanandi is a small, browser-based devotional songbook. It displays songs, bajans, and shlokas with lyrics, search, filters, saved songs, copy, and share actions.

The project is a **static website**. It does not use npm, a framework, a build process, or a backend.

## Project Files

```text
Bajan/
|-- index.html   Main page, CSS, HTML layout, and application JavaScript
|-- songs.js     Main song data collection
|-- songs2.js    Additional song data collection, currently including the Kubera shloka
|-- README.md    Project documentation
```

The browser loads the files in this order:

```html
<script src="./songs.js"></script>
<script src="./songs2.js"></script>
```

Both data files add songs to `window.songs`. The application then reads that combined array.

## How To Open The Website

The simplest option is to open `index.html` in a browser.

You can also use the VS Code Live Server extension:

1. Install **Live Server** in VS Code.
2. Right-click `index.html`.
3. Select **Open with Live Server**.

Because this is a static website, no terminal command or package installation is required.

## `index.html` Overview

`index.html` contains three main parts:

1. The `<head>` section
2. The visible `<body>` layout
3. The JavaScript application inside the final `<script>` tag

### Head Section

The head currently contains:

```html
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<meta name="theme-color" content="#f7f4ec" />
<title>Shivanandi | Devotional songs & lyrics</title>
```

These tags define the character encoding, mobile layout behavior, browser theme color, and browser tab title.

The page also loads two Google Fonts:

- `DM Sans` for normal interface text
- `Playfair Display` for headings and devotional titles

The hero image is loaded from Unsplash in the `.intro-image` CSS rule. An internet connection is required for the external fonts and image. The song data and application still work without them.

## Body Layout

### Header

The header contains:

- The Om symbol inside `.brand-mark`
- The `Shivanandi` brand name
- The short header message: `Words for the moments that matter`

Main classes:

- `.site-header`
- `.brand`
- `.brand-mark`
- `.brand-name`
- `.header-note`

### Intro Section

The intro section contains the page title, description, and devotional image.

Important elements:

```html
<section class="intro" aria-labelledby="page-title">
<h1 id="page-title">Let the words<br />bring you home.</h1>
</section>
```

To change the main title, edit the `<h1>` in `index.html`.

To change the introductory paragraph, edit the final `<p>` inside `.intro-copy`.

### Song Library

The `.library` section has two columns on desktop:

- `.browse`: search, filters, song list
- `.song-panel`: selected song details and lyrics

On screens narrower than 940px, the columns become one column and the song panel is placed below the list.

### Search and Filters

These elements are filled by JavaScript:

```html
<input id="search" />
<div id="filters"></div>
<div id="bajan-filters" hidden></div>
<div id="shloka-filters" hidden></div>
<div id="song-list"></div>
```

The main filter names are defined in JavaScript:

```js
const mainCategories = ["All songs", "Bajans", "Shloka", "Stuthi", "Saved"];
```

If you add a new section value to a song, also add that value to `mainCategories` if you want a dedicated filter button.

### Selected Song Panel

The right-side panel is populated from the currently selected song:

- `#song-category`: section and deity
- `#song-title`: song title
- `#song-meta`: language and content type
- `#lyrics`: formatted lyrics
- `#song-details`: optional additional information
- `#panel-foot`: footer text

The action buttons are:

- `#favorite-button`: saves or removes a song from the saved list
- `#copy-button`: copies the title and lyrics
- `#share-button`: uses the browser share API when available, otherwise copies the text

## CSS Guide

All CSS is currently inside the `<style>` block in `index.html`.

### Theme Variables

The colors and fonts are declared in `:root`:

```css
:root {
    --paper: #f7f4ec;
    --paper-deep: #eee9dc;
    --ink: #202d25;
    --muted: #72776e;
    --green: #315a43;
    --green-deep: #213e30;
    --saffron: #c9793e;
    --line: #ded9cc;
    --white: #fffefa;
}
```

Change these variables to update the visual theme consistently across the site.

The font variables are:

```css
--serif: "Playfair Display", Georgia, serif;
--sans: "DM Sans", "Segoe UI", sans-serif;
```

### Main CSS Areas

- Global reset and body styles: `*`, `body`, `button`, `input`
- Header: `.site-header`, `.brand`, `.brand-mark`, `.brand-name`
- Intro area: `.intro`, `.intro-copy`, `.intro-image`
- Song browser: `.library`, `.browse`, `.search`, `.filters`, `.song-list`
- Song rows: `.song-row`, `.song-symbol`, `.song-info`, `.row-arrow`
- Selected panel: `.song-panel`, `.panel-head`, `.lyrics`, `.panel-foot`
- Additional details: `.song-details`, `.song-details-head`, `.song-details-paragraph`
- Buttons: `.filter`, `.action-button`
- Responsive layout: media queries at 940px, 760px, and 520px
- Motion: `@keyframes rise-in` and reduced-motion support

### Additional Song Details Styling

The optional details area appears below the lyrics:

```css
.song-details {
    padding: 0 24px 25px;
}

.song-details-head,
.song-details-paragraph {
    color: var(--muted);
    font-family: var(--sans);
    font-size: 13px;
    line-height: 1.7;
}
```

Change these rules if you want the extra information to be larger, darker, centered, or spaced differently.

## Song Data Format

Every song is a JavaScript object inside `window.songs`.

The standard fields are:

| Field | Required | Purpose |
| --- | --- | --- |
| `id` | Yes | Unique internal identifier. Use lowercase words separated by hyphens. |
| `title` | Yes | Title displayed in the list and selected panel. |
| `searchTerms` | Recommended | English or alternate spellings used by search. |
| `section` | Yes | Usually `Bajans`, `Shloka`, or `Stuthi`. |
| `deity` | Yes | Used for deity filters and song metadata. |
| `language` | Yes | Language displayed in the song list and panel. |
| `symbol` | Yes | Short symbol shown in the circular song icon. |
| `lyrics` | Yes | Multiline lyrics inside a JavaScript template literal. |
| `detailsHeading` | Optional | Heading for the additional details area. |
| `detailshead` | Optional | First additional paragraph. Keep this exact lowercase field name because the renderer uses it. |
| `detailsParagraph` | Optional | Second additional paragraph. |

Current Kubera example:

```js
{
    id: "om-rajadhirajaya",
    title: "...",
    searchTerms: "Om Rajadhirajaya ...",
    section: "Shloka",
    deity: "Kubera",
    language: "Sanskrit",
    symbol: "ra",
    lyrics: `...`,
    detailsHeading: "About this shloka",
    detailshead: "This shloka is a prayer to Lord Kubera...",
    detailsParagraph: "Add the additional information about the Kubera shloka here."
}
```

The comma after the closing lyrics backtick is important because more fields follow it.

## Adding A New Song

Add a new object to `songs.js` or `songs2.js`.

Example:

```js
{
    id: "new-song-name",
    title: "New Song Title",
    searchTerms: "New Song Title alternate spelling",
    section: "Bajans",
    deity: "Ganesh",
    language: "Kannada",
    symbol: "ga",
    lyrics: `First lyric line
Second lyric line
**********`
},
```

Important rules:

- Put a comma after every object except the last object in the array.
- Make every `id` unique.
- Use backticks around multiline lyrics.
- Keep the `section` value consistent with the available filters.
- Add `searchTerms` for transliterations and alternate spellings.
- Use `**********` at the end when you want the marker centered by the renderer.

## Adding Details To A Song

Details are optional. Add these properties after the lyrics:

```js
lyrics: `Your lyrics here`,
detailsHeading: "About this song",
detailshead: "The first paragraph of information.",
detailsParagraph: "A second paragraph with more context."
```

If a song does not have `detailsHeading`, `detailshead`, or `detailsParagraph`, the details section is hidden automatically.

## JavaScript Flow

### Initial Setup

At the top of the script, the application gets the shared song array and important HTML elements:

```js
const songs = window.songs;
const filters = document.getElementById("filters");
const list = document.getElementById("song-list");
```

It also loads saved song IDs from browser `localStorage` using the key:

```text
saajha-saved
```

### `renderFilters()`

Creates the main filter buttons:

- All songs
- Bajans
- Shloka
- Stuthi
- Saved

Clicking a button changes `activeFilter`, resets the deity filter, and redraws the page.

### `renderBajanFilters()` and `renderShlokaFilters()`

These functions find unique deity names from the song data and create secondary filter buttons. They are visible only when the matching main category is selected.

### `matchingSongs()`

Filters the song array by:

1. Main section
2. Saved status
3. Selected deity
4. Search text

Search checks the title, search terms, ID, section, deity, language, and lyrics.

### `renderSongs()`

Creates the clickable song rows in the left column. Clicking a row updates `selectedId`, redraws the list, and calls `renderSelected()`.

### `renderSelected()`

Finds the selected song and fills the right panel. It also:

- Formats the lyrics
- Centers the `**********` marker
- Shows optional details
- Updates the favorite button state
- Updates the saved label

### Saved Songs

Saved song IDs are stored in browser local storage. They remain after refreshing the page in the same browser, but they are not stored in a server or shared with other users.

### Copy and Share

The copy button uses `navigator.clipboard`.

The share button uses `navigator.share` when supported. If sharing is unavailable, it copies the song text instead.

## Common Changes

### Change The Website Name

Edit these values in `index.html`:

- `<title>...</title>` in the head
- `.brand-name` text in the header
- Footer brand text

### Change The Main Heading

Edit the `<h1 id="page-title">` element in the intro section.

### Change The Hero Image

Find `.intro-image` in the CSS and replace the URL inside `background: ... url("...")`.

### Change The Footer

Edit the two `<span>` elements inside `<footer>`.

### Change Colors

Edit the variables inside `:root`. Prefer changing variables instead of searching for individual color values.

### Add A New Main Category

1. Add the category name to `mainCategories` in `index.html`.
2. Add songs using that exact `section` value.
3. Add a special sub-filter function only if that category needs one.

## Troubleshooting

### A song does not appear

Check that:

- The song object is inside `window.songs`.
- The object has a unique `id`.
- Commas are correct between fields and objects.
- The `section` value is spelled correctly.
- The browser console has no JavaScript errors.

### Details do not appear

Check the exact property names:

```js
detailsHeading
detailshead
detailsParagraph
```

Also check that the details text is not empty.

### Search does not find a song

Add transliterations and alternate spellings to `searchTerms`.

### Saved songs look incorrect after editing IDs

Saved IDs are stored in local storage. If you change a song ID, remove old saved data from the browser or save the song again under its new ID.

### The image or fonts do not load

The fonts and hero image come from external URLs. Check the internet connection. The site still works with fallback fonts and the background fallback color.

## Editing Checklist

Before finishing a change:

1. Check commas and quotation marks in the song data files.
2. Keep IDs unique.
3. Use the exact HTML IDs expected by the JavaScript.
4. Keep CSS class names consistent between HTML and CSS.
5. Open or refresh `index.html` in the browser.
6. Test search, filters, song selection, saved songs, copy, and share if those areas changed.
7. Check the browser console for errors.

## Technology Summary

- HTML5
- CSS3 with custom properties and responsive media queries
- Vanilla JavaScript
- Browser `localStorage`
- Clipboard API
- Web Share API when available
- Google Fonts
- Unsplash image URL

There is no framework, database, server-side code, or package manager in the current project.
