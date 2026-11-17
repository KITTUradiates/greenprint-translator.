# GreenPrint Translator v3.1

A mobile-friendly, GreenPrint-specific Spanish-to-English translator for Minecraft Bedrock add-ons.

## What's included

- `index.html` — the translator website.
- `greenprint_seed_memory.json` — 448 keyed translations, built from the Minecraft-tested GreenPrint English v1.8.8 reference and reviewed baseline text in the original v1.8.7 package.
- `manifest.webmanifest`, `icon-192.png`, `icon-512.png` — Android Chrome install/home-screen metadata and icons.

## What's fixed in this version

- The earlier template-literal/backtick failure is guarded by parser-based edits and a regression test using a nested JavaScript template literal.
- JavaScript is parsed with Acorn. Only selected string literals and template text segments are replaced. Template expressions such as `${result.missing}`, nested backticks, quotes, backslashes, line breaks, formatting codes and protected Minecraft placeholders are preserved.
- The complete known GreenPrint 1.8.7-to-1.8.8 translation set is bundled. Common GreenPrint UI does not need repeated Gemini calls.
- Additional baseline translations cover the minecart interaction label, item/material names, color names, and UI fragments not translated in the earlier working addon.
- The message-filter allowlist is extended with English equivalents while keeping Spanish alternatives. This allows translated player notifications to pass the same filter.
- The website checks JavaScript syntax and AST structure, JSON schema/key structure, `.lang` keys, UUIDs, dependencies, archive file paths, and protected project/structure files before enabling download.
- `projects.js` display-name strings can be translated. `container_contents.js`, `decorations.js`, `water_layers.js`, `.mcstructure` files, textures, models and technical/project data remain untouched.
- Both GreenPrint pack header/module versions are incremented on every export, even when a variant changes only structure/material data and needs no new text translation. The Behavior Pack's dependency on the Resource Pack is updated. The Resource Pack's reverse dependency version is left as in the proven v1.8.8 reference.
- The bundled baseline records your already-working v1.8.8 package as the latest installed version. A first translation from the original v1.8.7 source will therefore export as v1.8.9, avoiding a same-version collision with the working addon. Every subsequent downloaded variant receives a higher pack version, including variants with only structure/material changes. Version history is stored per GreenPrint UUID pair. When you translate multiple source variants that all start at v1.8.7, the first website-generated build will be v1.8.9 (because v1.8.8 is already installed); subsequent downloads will use v1.8.10, v1.8.11, and so on, instead of repeating the same pack version. Version history is saved only when you download the output and is included in the memory backup.
- A Gemini API key is only required if the selected variant contains new strings not already in the bundled/device translation memory. The key is held in page memory and is not saved to localStorage or written into the addon.

## Android setup (one-time)

The site runs in Chrome on Android. Host it over HTTPS so browser file access, Acorn/JSZip libraries, and the Gemini API can work.

### Publish with GitHub Pages

1. Download and extract `GreenPrint_Translator_Website_v3.1.zip` on your phone.
2. In Chrome, sign in to GitHub and create a new **public** repository, for example `greenprint-translator`. The website source and glossary will be public; they contain no addon file or API key.
3. Open the repository and choose **Add file → Upload files**.
4. Upload all six files from the extracted folder: `index.html`, `greenprint_seed_memory.json`, `manifest.webmanifest`, `icon-192.png`, `icon-512.png`, and `README.md`. Keep them in the repository's root (top) folder, not inside another folder.
5. Commit the upload.
6. Open **Settings → Pages**. Under the build/deployment source, choose **Deploy from a branch**, select branch `main` and folder `/ (root)`, then save.
7. After GitHub Pages publishes the site, open the HTTPS link it displays. It will look like `https://YOUR-USERNAME.github.io/greenprint-translator/`.
8. In Chrome, open the three-dot menu and choose **Add to Home screen** or **Install app** if shown. You can then launch it like an app.

You only need to host the website once. You do not need to upload your GreenPrint addon to GitHub.

## Each time you translate a new GreenPrint variant

1. Open GreenPrint Translator in Chrome on your Android phone.
2. Select the new/original Spanish GreenPrint `.mcaddon` from the Android file picker. Prefer the creator's source variant; it is okay if it contains different structure/material data.
3. If the translator finds new player-facing strings, paste a Gemini API key. Create one at <https://aistudio.google.com/app/apikey>. Select `gemini-3.8-flash` (recommended) or `gemini-3.5-flash-lite` (lower-cost option).
4. Tap **Scan, translate and verify**. Known UI translations are reused. Only new/changed strings that the scanner considers candidates are sent to Gemini.
5. Review the new translation rows and validation report. If validation fails, no download is offered.
6. Tap **Download English GreenPrint addon**. On Android, open the downloaded `.mcaddon` and choose Minecraft if prompted. If Android only saves it, use Files → Downloads → tap the `.mcaddon` → Open with Minecraft.
7. Test the new version in a disposable world before using it in your main world or Realm.

For repeated variants with the same UUIDs, use the same browser/device and click Download for every build. The saved version history prevents the pack version from repeating. Export a memory backup before clearing Chrome site data or changing phones.

## Translation memory

- The bundled seed file has 448 translation keys. Many keys are tied to a normalized code context so a short fragment is not reused in an unrelated message.
- New Gemini translations are saved in the browser's localStorage for this site origin.
- **Export memory backup** saves the bundled and device-added translations plus pack version history.
- **Import memory backup** restores those entries and version history on another browser/device.
- **Clear saved additions** removes only device-added entries; the bundled memory remains available.

## API key and privacy

The addon archive is unpacked, scanned, edited and repacked in your browser; it is not uploaded to this site or to GitHub. Only selected candidate text strings and short surrounding code context are sent to Google's Gemini API when a translation is not already in memory.

This is a personal-use client-side website, not a secure multi-user service. The Gemini key is present in the browser's memory and in requests to Google, so someone with access to your active browser/devtools/network traffic could see it. Do not publish the key in GitHub, screenshots or messages. For personal use, create a dedicated key restricted to the Gemini API and monitor its usage. If you ever share the website with other people, use a backend proxy rather than asking users to paste keys into a public page.

Current model IDs used by the selector are `gemini-3.8-flash` and `gemini-3.5-flash-lite`. The request uses the Gemini `generateContent` REST endpoint and omits custom sampling parameters.

## Validation limits

The validation checks archive integrity and code/data structure; it cannot execute Minecraft Bedrock or prove every gameplay interaction. It also cannot guarantee that Gemini's translation is contextually perfect. Review newly translated strings and test the generated addon in-game.
