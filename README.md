# ⚠️ Stalled ⚠️ This project is not under active developmet

## Cleanstream - Live Speech Filter using AI

CleanStream is an OBS plugin that cleans live audio streams from unwanted words and utterances using real-time local AI.

<div align="center">

[![GitHub](https://img.shields.io/github/license/locaal-ai/obs-cleanstream)](https://github.com/locaal-ai/obs-cleanstream/blob/main/LICENSE)
[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/locaal-ai/obs-cleanstream/push.yaml)](https://github.com/locaal-ai/obs-cleanstream/actions/workflows/push.yaml)
[![Total downloads](https://img.shields.io/github/downloads/locaal-ai/obs-cleanstream/total)](https://github.com/locaal-ai/obs-cleanstream/releases)
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/locaal-ai/obs-cleanstream)](https://github.com/locaal-ai/obs-cleanstream/releases)

</div>

## Usage

- Add the plugin to any audio-generating source
- Adjust the settings

<div align="center">

<a href="https://youtu.be/vgcK3K654FU"><img src="https://github.com/locaal-ai/obs-cleanstream/assets/441170/59f0038a-4e0e-47ed-afb8-110acd463146" width="40%" /></a>

</div>

## Download
Check out the [latest releases](https://github.com/locaal-ai/obs-cleanstream/releases) for downloads and install instructions.

## Method
This video walkthrough (YouTube) will explain various parts of the code if you're looking to learn from what I've discovered.

<div align="center">
    <a href="https://youtu.be/HdSI3sUKwsY" target="_blank">
        <img width="480" src="https://img.youtube.com/vi/HdSI3sUKwsY/maxresdefault.jpg" />
    </a>
</div>

### Audio processing

The filter is running Whisper in real-time to detect words in small chunks of the incoming audio. For each chunck it produces a decision which then determines if the audio rendering will play the original audio or e.g. a beep or silence. The processing happens in a separate thread and therefore there's a built-in lag/delay mechanism to make sure the audio decision (play, beep, silence) is in-sync with the actual audio playback based on the timestamp. The built-in delay is adaptive since some systems (e.g. with CUDA) can make faster decisions.

Here is an illustration of the process:

![alt text](docs/image.png)

## Requirements
- OBS version 30+ for plugin versions 0.0.4+
- OBS version 29 for plugin versions 0.0.2+
- OBS version 28 for plugin versions 0.0.1

We do not support older versions of OBS since the plugin is using newer APIs.

## Introduction
CleanStream is an OBS plugin that cleans live audio streams from unwanted words and utterances, such as "uh"s and "um"s, and other words that you can configure, like profanity.

See our [resource on the OBS Forums](https://obsproject.com/forum/resources/cleanstream-remove-uhs-ums-profanity-in-your-live-stream-or-recording-with-ai.1732/) for additional information.

It is using a neural network ([OpenAI Whisper](https://github.com/openai/whisper)) to predict in real time the speech and remove the unwanted words.

It's using the [Whisper.cpp](https://github.com/ggerganov/whisper.cpp) project from [ggerganov](https://github.com/ggerganov) to run the Whisper network in a very efficient way.

But it is working and you can try it out. *Please report any issues you find.* 🙏 ([submit an issue](https://github.com/locaal-ai/obs-cleanstream/issues) or meet us on https://discord.gg/KbjGU2vvUz)

We're working on improving the plugin and adding more features. If you have any ideas or suggestions, please open an issue.

## Building

The plugin was built and tested on Mac OSX, Windows and Ubuntu Linux. Help is appreciated in building on other OSs and packages.

The building pipelines in CI take care of the heavy lifting. Use them in order to build the plugin locally.

Start by cloning this repo to a directory of your choice.

### Mac OSX

Using the CI pipeline scripts, locally you would just call the zsh script.

```sh
$ ./.github/scripts/build-macos.zsh -c Release -t macos-x86_64
```

#### Install
The above script should succeed and the plugin files will reside in the `./release` folder off of the root. Copy the files to the OBS directory e.g. `/Users/you/Library/Application Support/obs-studio/obs-plugins`.

To get `.pkg` installer file, run
```sh
$ ./.github/scripts/package-macos.zsh -c Release -t macos-x86_64
```
(Note that maybe the outputs in the e.g. `build_x86_64` will be in the `Release` folder and not the `install` folder like `pakage-macos.zsh` expects, so you will need to rename the folder from `build_x86_64/Release` to `build_x86_64/install`)

### Linux (Ubuntu-ish)

Use the CI scripts again
```sh
$ ./.github/scripts/build-linux.sh
```

### Windows

Use the CI scripts again, for example:

```powershell
> .github/scripts/Build-Windows.ps1 -Configuration Release
```

The build should exist in the `./release` folder off the root. You can manually install the files in the OBS directory.

```powershell
> Copy-Item -Recurse -Force "release\Release\*" -Destination "C:\Program Files\obs-studio\"
```

#### Building with CUDA support on Windows

CleanStream will now build with CUDA support automatically through a prebuilt binary of Whisper.cpp from https://github.com/occ-ai/occ-ai-dep-whispercpp. The CMake scripts will download all necessary files.

To build with cuda add `CPU_OR_CUDA` as an environment variable (with `cpu`, `12.2.0` or `11.8.0`) and build regularly

```powershell
> $env:CPU_OR_CUDA="12.2.0"
> .github/scripts/Build-Windows.ps1 -Configuration Release
```
## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=locaal-ai/obs-cleanstream&type=Date)](https://star-history.com/#locaal-ai/obs-cleanstream&Date)
