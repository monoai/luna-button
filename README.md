# Luna Button
[![Last Commit](https://img.shields.io/github/last-commit/monoai/luna-button)]()
[![Website](https://img.shields.io/website?url=https%3A%2F%2Fnanora.moe)](https://nanora.moe)
[![Twitter Follow](https://img.shields.io/twitter/follow/monoAI_)](https://twitter.com/monoAI_)

![Luna-Button Cover](https://user-images.githubusercontent.com/46586720/108640159-10942d80-74d3-11eb-917b-da15b979a233.png)

A Voice Button Website dedicated to Hololive 4th Generation VTuber Himemori Luna. んなあああああ！！！！

[Click here to visit https://nanora.moe](https://nanora.moe)

## Related Links:

* [Himemori Luna's Youtube channel](https://www.youtube.com/channel/UCa9Y57gfeY0Zro_noHRVrnw)

* [Himemori Luna's Twitter](https://twitter.com/himemoriluna)

## Contributing

Please fork this project for modification, and after completing the modification, initiate a Pull Request in this project.

### Add or modify voice

**Description**: All voice meta information is stored in [src/voices.json](src/voices.json). To add or modify these voices, you need to modify this file accordingly.

Voice is always in mp3 format and stored in [public/voices](public/voices). The corresponding URL is `voices/`.

For new voice, please use software such as MP3GainGUI for volume standardization. Currently the volume standardized value is 80.

Please note that for this project, we specifically categorize the voices by this pattern: `CATEGORY_VOICENAME.mp3`. Example for a proper filename is `EN_ICookTheSubaru.mp3` where `EN` represents the `Nanolish` Category while `ICookTheSubaru` represents the voice name. It doesn't have to fully transcribe the line as long as it describes it for easy locating. Please check [voices.json](src/voices.json) for more information.

Because this site uses a strong cache strategy, except for `index.html`, files with the same filename, even if modified, will **NEVER** be refreshed by the client. Therefore, the filename of the newly voice, whatever it is new or modified, **MUST** be different from any previous filename.

If you are modifying voice, delete the original file after modification.

### Participate in translation

Please help us translate to the available languages! (EN/JP/TW/MY)

The language files for the main program are in three .js files named the language name in [src/locales](src/locales).

The language files for voices are in [src/voices.json](src/voices.json).

The country flags are located in [src/App.vue](src/App.vue) just in case a new language is implemented.

The corresponding modification can be recognized by the program as a valid translation.

## Deploying a local development environment

This site is developed using Vue + jQuery + Bootstrap 3.

To deploy a local development environment, first install the latest version of Node. Then follow these steps:

1. Clone the code.

2. Go to the code directory and run `npm install`.

3. Run `npm run serve`. During the code modification process, this local development server can immediately reflect the results of the modification.

4. To compile the files for deployment, run `npm run build`, which will generate the `dist` directory. This site is completely static, you can directly deploy the entire `dist` directory.

> To contribute your code to this project, you don't have to compile locally. After passing the test in the development server and pushing it to Github, you can directly require a Pull Request to this project.

## LICENSE
[![LICENSE](https://img.shields.io/github/license/monoai/luna-button)](LICENSE)

Program: MIT

Audio: According to the [Hololive secondary creation licence](https://www.hololive.tv/terms).

This project is a work of fans and is not related to the official Hololive.

## Special Thanks

This project is modified based on zyzsdy's [Aqua button](https://github.com/zyzsdy/aqua-button).

The creator of this project would like to thank the contributors for their translation/voice work and the Luna Candy Kingdom Discord server for their help and support in developing this site.
