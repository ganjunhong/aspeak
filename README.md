# :speaking_head: aspeak

[![GitHub stars](https://img.shields.io/github/stars/kxxt/aspeak)](https://github.com/kxxt/aspeak/stargazers)
[![GitHub issues](https://img.shields.io/github/issues/kxxt/aspeak)](https://github.com/kxxt/aspeak/issues)
[![GitHub forks](https://img.shields.io/github/forks/kxxt/aspeak)](https://github.com/kxxt/aspeak/network)
[![GitHub license](https://img.shields.io/github/license/kxxt/aspeak)](https://github.com/kxxt/aspeak/blob/main/LICENSE)
[![PyPI version](https://badge.fury.io/py/aspeak.svg)](https://badge.fury.io/py/aspeak)

<a href="https://github.com/kxxt/aspeak/graphs/contributors" alt="Contributors">
    <img src="https://img.shields.io/github/contributors/kxxt/aspeak" />
</a>
<a href="https://github.com/kxxt/aspeak/pulse" alt="Activity">
    <img src="https://img.shields.io/github/commit-activity/m/kxxt/aspeak" />
</a>


A simple text-to-speech client using azure TTS API(trial). :laughing:

**TL;DR**: This program uses trial auth token of Azure Cognitive Services to do speech synthesis for you.

You can try the Azure TTS API online: https://azure.microsoft.com/en-us/services/cognitive-services/text-to-speech

## Installation

```sh
$ pip install --upgrade aspeak
```

## Limitations

Since we are using Azure Cognitive Services, there are some limitations:

| Quota | Free (F0)<sup>3</sup> |
|--|--|
| **Max number of transactions per certain time period per Speech service resource** | |
| Real-time API. Prebuilt neural voices and custom neural voices. | 20 transactions per 60 seconds |
| Adjustable | No<sup>4</sup> |
| **HTTP-specific quotas** | |
| Max audio length produced per request | 10 min |
| Max total number of distinct `<voice>` and `<audio>` tags in SSML | 50 |
| **Websocket specific quotas** | |
| Max audio length produced per turn | 10 min |
| Max total number of distinct `<voice>` and `<audio>` tags in SSML | 50 |
| Max SSML message size per turn | 64 KB |

This table is copied
from [Azure Cognitive Services documentation](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/speech-services-quotas-and-limits#general)

And the limitations may be subject to change. The table above might become outdated in the future. Please refer to the
latest [Azure Cognitive Services documentation](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/speech-services-quotas-and-limits#general)
for the latest information.

**Attention**: If the result audio is longer than 10 minutes, the audio will be truncated to 10 minutes and the program
will not report an error.

## Usage

```
usage: usage: aspeak [-h] [-V | -L | -Q | [-t [TEXT] [-p PITCH] [-r RATE] [-S STYLE] [-R ROLE] [-d STYLE_DEGREE] | -s [SSML]]] 
              [-f FILE] [-e ENCODING] [-o OUTPUT_PATH] [-l LOCALE] [-v VOICE]
              [--mp3 [-q QUALITY] | --ogg [-q QUALITY] | --webm [-q QUALITY] | --wav [-q QUALITY] | -F FORMAT] 

This program uses trial auth token of Azure Cognitive Services to do speech synthesis for you

options:
  -h, --help            show this help message and exit
  -V, --version         show program's version number and exit
  -L, --list-voices     list available voices, you can combine this argument with -v and -l
  -Q, --list-qualities-and-formats
                        list available qualities and formats
  -t [TEXT], --text [TEXT]
                        Text to speak. Left blank when reading from file/stdin
  -s [SSML], --ssml [SSML]
                        SSML to speak. Left blank when reading from file/stdin
  -f FILE, --file FILE  Text/SSML file to speak, default to `-`(stdin)
  -e ENCODING, --encoding ENCODING
                        Text/SSML file encoding, default to "utf-8"(Not for stdin!)
  -o OUTPUT_PATH, --output OUTPUT_PATH
                        Output file path, wav format by default
  --mp3                 Use mp3 format for output. (Only works when outputting to a file)
  --ogg                 Use ogg format for output. (Only works when outputting to a file)
  --webm                Use webm format for output. (Only works when outputting to a file)
  --wav                 Use wav format for output
  -F FORMAT, --format FORMAT
                        Set output audio format (experts only)
  -l LOCALE, --locale LOCALE
                        Locale to use, default to en-US
  -v VOICE, --voice VOICE
                        Voice to use
  -q QUALITY, --quality QUALITY
                        Output quality, default to 0

Options for --text:
  -p PITCH, --pitch PITCH
                        Set pitch, default to 0
  -r RATE, --rate RATE  Set speech rate, default to 0
  -S STYLE, --style STYLE
                        Set speech style, default to "general"
  -R ROLE, --role ROLE  Specifies the speaking role-play. This only works for some Chinese voices! Available values are Girl, Boy, YoungAdultFemale, YoungAdultMale, OlderAdultFemale,
                        OlderAdultMale, SeniorFemale, SeniorMale.
  -d STYLE_DEGREE, --style-degree STYLE_DEGREE
                        Specifies the intensity of the speaking style. range: [0.01, 2]. This only works for some Chinese voices!

Attention: If the result audio is longer than 10 minutes, the audio will be truncated to 10 minutes and the program will not report an error. Please refer to the documentation for
other limitations at https://github.com/kxxt/aspeak/blob/main/README.md#limitations
```

- If you don't specify `-o`, we will use your default speaker.
- If you don't specify `-t` or `-s`, we will assume `-t` is provided.
- You must specify voice if you want to use `-p`/`-r`/`-S` option.

### Special Note for Pitch and Rate

- Pitch is a float value.
    - It is usually between -0.5 and 0.5.
    - The default value is 0.
- Rate is also a float value.
    - It is usually between -1 and 2.
    - The default value is 0.
    - Note that this value is different from the speaking speed field on the trial page.

### Examples

#### Speak "Hello, world!" to default speaker.

```sh
$ aspeak -t "Hello, world"
```

#### List all available voices.

```sh
$ aspeak -L
```

#### List all available voices for Chinese.

```sh
$ aspeak -L -l zh-CN
```

#### Get information about a voice.

```sh
$ aspeak -L -v en-US-SaraNeural
```

<details>

<summary>
    Output
</summary>

```
Microsoft Server Speech Text to Speech Voice (en-US, SaraNeural)
Display Name: Sara
Local Name: Sara @ en-US
Locale: English (United States)
Gender: Female
ID: en-US-SaraNeural
Styles: ['cheerful', 'angry', 'sad']
Voice Type: Neural
Status: GA
```

</details>

#### Save synthesized speech to a file.

```sh
$ aspeak -t "Hello, world" -o output.wav
```

If you prefer mp3/ogg/webm, you can use `--mp3`/`--ogg`/`--webm` option.

```sh
$ aspeak -t "Hello, world" -o output.mp3 --mp3
$ aspeak -t "Hello, world" -o output.ogg --ogg
$ aspeak -t "Hello, world" -o output.webm --webm
```

#### List available quality levels and formats

```sh
$ aspeak -Q
```

<details>

<summary>Output</summary>

```
Available qualities:
Qualities for wav:
-2: Riff8Khz16BitMonoPcm
-1: Riff16Khz16BitMonoPcm
 0: Riff24Khz16BitMonoPcm
 1: Riff24Khz16BitMonoPcm
Qualities for mp3:
-3: Audio16Khz32KBitRateMonoMp3
-2: Audio16Khz64KBitRateMonoMp3
-1: Audio16Khz128KBitRateMonoMp3
 0: Audio24Khz48KBitRateMonoMp3
 1: Audio24Khz96KBitRateMonoMp3
 2: Audio24Khz160KBitRateMonoMp3
 3: Audio48Khz96KBitRateMonoMp3
 4: Audio48Khz192KBitRateMonoMp3
Qualities for ogg:
-1: Ogg16Khz16BitMonoOpus
 0: Ogg24Khz16BitMonoOpus
 1: Ogg48Khz16BitMonoOpus
Qualities for webm:
-1: Webm16Khz16BitMonoOpus
 0: Webm24Khz16BitMonoOpus
 1: Webm24Khz16Bit24KbpsMonoOpus

Available formats:
- Riff8Khz16BitMonoPcm
- Riff16Khz16BitMonoPcm
- Audio16Khz128KBitRateMonoMp3
- Raw24Khz16BitMonoPcm
- Raw48Khz16BitMonoPcm
- Raw16Khz16BitMonoPcm
- Audio24Khz160KBitRateMonoMp3
- Ogg24Khz16BitMonoOpus
- Audio16Khz64KBitRateMonoMp3
- Raw8Khz8BitMonoALaw
- Audio24Khz16Bit48KbpsMonoOpus
- Ogg16Khz16BitMonoOpus
- Riff8Khz8BitMonoALaw
- Riff8Khz8BitMonoMULaw
- Audio48Khz192KBitRateMonoMp3
- Raw8Khz16BitMonoPcm
- Audio24Khz48KBitRateMonoMp3
- Raw24Khz16BitMonoTrueSilk
- Audio24Khz16Bit24KbpsMonoOpus
- Audio24Khz96KBitRateMonoMp3
- Webm24Khz16BitMonoOpus
- Ogg48Khz16BitMonoOpus
- Riff48Khz16BitMonoPcm
- Webm24Khz16Bit24KbpsMonoOpus
- Raw8Khz8BitMonoMULaw
- Audio16Khz16Bit32KbpsMonoOpus
- Audio16Khz32KBitRateMonoMp3
- Riff24Khz16BitMonoPcm
- Raw16Khz16BitMonoTrueSilk
- Audio48Khz96KBitRateMonoMp3
- Webm16Khz16BitMonoOpus
```

</details>

#### Increase/Decrease audio qualities

```sh
# Less than default quality.
$ aspeak -t "Hello, world" -o output.mp3 --mp3 -q=-1
# Best quality for mp3
$ aspeak -t "Hello, world" -o output.mp3 --mp3 -q=3
```

#### Read text from file and speak it.

```sh
$ cat input.txt | aspeak
```

or

```sh
$ aspeak -f input.txt
```

with custom encoding:

```sh
$ aspeak -f input.txt -e gbk
```

#### Read from stdin and speak it.

```sh
$ aspeak
```

or (more verbose)

```sh
$ aspeak -f -
```

maybe you prefer:

```sh
$ aspeak -l zh-CN << EOF
我能吞下玻璃而不伤身体。
EOF
```

#### Speak Chinese.

```sh
$ aspeak -t "你好，世界！" -l zh-CN
```

#### Use a custom voice.

```sh
$ aspeak -t "你好，世界！" -v zh-CN-YunjianNeural
```

#### Custom pitch, rate and style

```sh
$ aspeak -t "你好，世界！" -v zh-CN-XiaoxiaoNeural -p 1.5 -r 0.5 -S sad
```

### Advanced Usage

#### Use a custom audio format for output

**Note**: When outputing to default speaker, using a non-wav format may lead to white noises.

```sh
$ aspeak -t "Hello World" -F Riff48Khz16BitMonoPcm -o high-quality.wav
```

#### Custom style degree and role

According to the
[Azure documentation](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/speech-synthesis-markup?tabs=csharp#adjust-speaking-styles)
, style degree specifies the intensity of the speaking style.
It is a floating point number between 0.01 and 2, inclusive.

At the time of writing, style degree adjustments are supported for Chinese (Mandarin, Simplified) neural voices.

According to the
[Azure documentation](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/speech-synthesis-markup?tabs=csharp#adjust-speaking-styles)
, `role` specifies the speaking role-play. The voice acts as a different age and gender, but the voice name isn't
changed.

At the time of writing, role adjustments are supported for these Chinese (Mandarin, Simplified) neural voices:
`zh-CN-XiaomoNeural`, `zh-CN-XiaoxuanNeural`, `zh-CN-YunxiNeural`, and `zh-CN-YunyeNeural`.

## About This Application

- I found Azure TTS can synthesize nearly authentic human voice, which is very interesting :laughing:.
- I wrote this program to learn Azure Cognitive Services.
- And I use this program daily, because `espeak` and `festival` outputs terrible :fearful: audio.
    - But I respect :raised_hands: their maintainers' work, both are good open source software and they can be used
      off-line.
- I hope you like it :heart:.

## Alternative Applications

- https://github.com/skygongque/tts
- https://github.com/LuckyHookin/edge-TTS-record/
