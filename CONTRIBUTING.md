# Contributing

Keep this catalogue small, useful, and supported by sources. A clear limitation is more helpful than a broad claim.

## Local setup

Use Node.js 22 or newer. There are no runtime dependencies to install.

~~~sh
npm test
npm run validate
npm run docs
npm run docs:check
~~~

## Add or revise an entry

Edit data/tools.json. Each entry needs a unique, stable id; a name and project URL; categories and interfaces; English and Traditional Chinese summaries, best-fit descriptions, and limitations; and at least one source with its checkedAt date.

Use the project's own README, documentation, source code, release notes, or issue tracker. Describe a concrete use case and a meaningful limitation. Do not treat a missing search result as evidence that competitors do not exist. Record only capabilities the cited material supports; distinguish a proposed feature from a released one.

Use the existing category and interface values when they fit. A new value should describe a useful distinction rather than a single product. Write Traditional Chinese in clear language, keeping product names and command names unchanged.

Set checkedAt to the date you actually review that source. Update the catalogue's reviewedAt when completing a catalogue review. Do not refresh dates without reviewing the material.

Catalogue entries describe source review. If you execute a third-party tool, explain the version, platform, command, and observed result in the pull request; do not imply the whole catalogue was tested.

## Generate the guides

Both README files are generated. Edit the catalogue for entry changes, or scripts/build-readme.mjs for shared guide text, then run npm run docs. Commit the generated English and Traditional Chinese files together.

Run the local checks above before opening a pull request. CI uses Windows and Linux with Node.js 22 and 24, and checks that regenerated documentation matches the committed files.

Live URL checks are optional and separate from CI because servers, rate limits, and network access can vary:

~~~sh
node bin/agent-tool-map.mjs check-links --json
~~~

## Scope

Prefer small fixes and evidence-backed catalogue improvements. Keep the CLI dependency-free and usable without API keys or a model connection. Discuss larger changes in an issue before adding integrations or a new runtime service.
