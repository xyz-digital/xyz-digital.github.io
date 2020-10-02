---
layout: page
title:  Exploring Mozilla Text To Speech
author: Adam Berg
---

This post explores some of the options for converting text to speech. It focuses primarily on Mozilla's TTS library and how to get it up and running on your computer.

<!--more-->

> Nothing is impossible, the word itself says, ‘I'm possible!' -- Audrey Hepburn

<audio id="mozilla-tts-quote" controls="controls">
  <source type="audio/mp3" src="{{"/assets/audio/impossible-quote.mp3" | relative_url}}"></source>
  <p>Your browser does not support the audio element.</p>
</audio>

### Google Cloud Text-to-Speech API

Google offers a [Text-to-Speech API](https://cloud.google.com/text-to-speech) that is priced at $4 / 1 million characters or $16 / 1 million characters for the [advanced WaveNet voices](https://cloud.google.com/text-to-speech/docs/voices).  The variety of voices and languages is impressive, but I was more interested in finding an offline solution, even if it produces lower quality audio.

### Amazon Polly

Unsurprisingly, Amazon offers an API as well called [Amazon Polly](https://aws.amazon.com/polly/).  The variety of voices appears to be a bit less than Google's offering and the number of supported languages is also behind.  The pricing is the same as Google's with a cheaper standard model and a more expensive "neural" model.

### eSpeak

I figured there would be a fairly simple command line program that would do fairly basic and robotic speech-to-text.  [eSpeak](http://espeak.sourceforge.net/) emerged as an option and I gave it a try.  `sudo apt-get install espeak`.

```
espeak "Nothing is impossible, the word itself says, I'm possible! Audrey Hepburn" -w output.wav
```

<audio controls="controls">
  <source type="audio/mp3" src="{{"/assets/audio/impossible-quote-espeak.mp3" | relative_url}}"></source>
  <p>Your browser does not support the audio element.</p>
</audio>

eSpeak scores some major points for simplicity.  Unfortunately, the speech is quite robotic and annoying to listen to.  I continued my search until I found Mozilla TTS.


## Mozilla TTS

I finally stumbled upon [https://github.com/mozilla/TTS](https://github.com/mozilla/TTS), a project that is a part of [Mozilla Common Voice](https://commonvoice.mozilla.org/en).  Mozilla's mission, "Keep the internet open and accessible to all", captures what they do well.  It also exemplifies a belief likely held by most software engineers.  With Common Voice, Mozilla is attempting to collect a large publically available dataset of labelled speech.  [ImageNet](http://www.image-net.org/) was a similar project [ignited the development of computer vision models](https://qz.com/1034972/the-data-that-changed-the-direction-of-ai-research-and-possibly-the-world/) by providing a massive list of labeled images that researchers could freely access.  With a public dataset, it was possible to design these models outside of the major companies (like Google and Amazon) that already had private access to this kind of data.  The Common Voice website makes it easy to either donate your voice or validate others. 

### How to Run Mozilla TTS

I was surprised to find that the main Readme didn't really have straightforward instructions for how to run, or even what the end product was when you did finally run it.  Their wiki included some [Build Instructions](https://github.com/mozilla/TTS/wiki/Build-instructions-for-server), but after following them I was unable to get things running because the config file provided did not have all the values set.  After diving through some issues, I could tell there were quite a few parameters that even if I could identify them wouldn't know what to set them to.  Thankfully, below the models and configs you were supposed to download was a [simpler route](https://github.com/mozilla/TTS/wiki/Released-Models#simple-packaging---self-contained-package-that-runs-an-http-api-for-a-pre-trained-tts-model):

```bash
# Create a fresh virtual environment with Python 3.6
apt-get install espeak libsndfile1
pip install https://github.com/reuben/TTS/releases/download/ljspeech-fwd-attn-pwgan/TTS-0.0.1+92aea2a-py3-none-any.whl
python -m TTS.server.server
# Open http://localhost:5002
```

![Mozilla TTS]({{"/assets/images/Mozilla-TTS.png" | relative_url}})

The output was pretty good overall (this is the one used at the [top of the article](#mozilla-tts-quote)).  However, if I were to use this in any kind of a project, I would not want to have to manually enter it in to the browser and manually download outputs.  I dug a bit deeper to see if I could extract a fairly simple CLI based on the web application.

I was able to modify the included [server.py](https://github.com/mozilla/TTS/blob/master/server/server.py) file to run on the command line instead of the Flask application it really was.  I added a text argument so the input file could be passed in and updated the export to write the bytes to a user specified .wav file.

```python
import argparse
import os

from TTS.server.synthesizer import Synthesizer


def create_argparser():
    def convert_boolean(x):
        return x.lower() in ['true', '1', 'yes']

    parser = argparse.ArgumentParser()
    parser.add_argument('--tts_checkpoint', type=str, help='path to TTS checkpoint file')
    parser.add_argument('--tts_config', type=str, help='path to TTS config.json file')
    parser.add_argument('--tts_speakers', type=str, help='path to JSON file containing speaker ids, if speaker ids are used in the model')
    parser.add_argument('--wavernn_lib_path', type=str, default=None, help='path to WaveRNN project folder to be imported. If this is not passed, model uses Griffin-Lim for synthesis.')
    parser.add_argument('--wavernn_file', type=str, default=None, help='path to WaveRNN checkpoint file.')
    parser.add_argument('--wavernn_config', type=str, default=None, help='path to WaveRNN config file.')
    parser.add_argument('--is_wavernn_batched', type=convert_boolean, default=False, help='true to use batched WaveRNN.')
    parser.add_argument('--pwgan_lib_path', type=str, default=None, help='path to ParallelWaveGAN project folder to be imported. If this is not passed, model uses Griffin-Lim for synthesis.')
    parser.add_argument('--pwgan_file', type=str, default=None, help='path to ParallelWaveGAN checkpoint file.')
    parser.add_argument('--pwgan_config', type=str, default=None, help='path to ParallelWaveGAN config file.')
    parser.add_argument('--use_cuda', type=convert_boolean, default=True, help='true to use CUDA.')
    parser.add_argument('--text', type=str, help='input file to read text from')
    parser.add_argument('--out', type=str, default="output.wav", help='output file to save speech as')
    return parser


synthesizer = None

embedded_models_folder = os.path.join(os.path.dirname(os.path.realpath(__file__)), 'model')

embedded_tts_folder = os.path.join(embedded_models_folder, 'tts')
tts_checkpoint_file = os.path.join(embedded_tts_folder, 'checkpoint.pth.tar')
tts_config_file = os.path.join(embedded_tts_folder, 'config.json')

embedded_wavernn_folder = os.path.join(embedded_models_folder, 'wavernn')
wavernn_checkpoint_file = os.path.join(embedded_wavernn_folder, 'checkpoint.pth.tar')
wavernn_config_file = os.path.join(embedded_wavernn_folder, 'config.json')

embedded_pwgan_folder = os.path.join(embedded_models_folder, 'pwgan')
pwgan_checkpoint_file = os.path.join(embedded_pwgan_folder, 'checkpoint.pkl')
pwgan_config_file = os.path.join(embedded_pwgan_folder, 'config.yml')

args = create_argparser().parse_args()

# If these were not specified in the CLI args, use default values with embedded model files
if not args.tts_checkpoint and os.path.isfile(tts_checkpoint_file):
    args.tts_checkpoint = tts_checkpoint_file
if not args.tts_config and os.path.isfile(tts_config_file):
    args.tts_config = tts_config_file
if not args.wavernn_file and os.path.isfile(wavernn_checkpoint_file):
    args.wavernn_file = wavernn_checkpoint_file
if not args.wavernn_config and os.path.isfile(wavernn_config_file):
    args.wavernn_config = wavernn_config_file
if not args.pwgan_file and os.path.isfile(pwgan_checkpoint_file):
    args.pwgan_file = pwgan_checkpoint_file
if not args.pwgan_config and os.path.isfile(pwgan_config_file):
    args.pwgan_config = pwgan_config_file

synthesizer = Synthesizer(args)

with open(args.text, "r") as f:
    text = f.read()

data = synthesizer.tts(text)

with open(args.out, "wb") as f:
    f.write(data.getbuffer())
```

### How Does Mozilla TTS Stack Up?

The first test I ran was on a random short story I found [here](https://www.classicshorts.com/stories/aos.html).  At about 16 seconds in to the clip below, the output appears to choke on the word pasture.  It manages to recover quickly, but not before letting out a ghoulish "ttteeeeaaarrr" at around 26 seconds.  When I first heard the stuttering I was laughing.  Now I think I may need to sleep with the lights on. 

> They even executed a few innocent people to prove that they knew how to kill, and in roaming through virgin fields still belonging to the Prussians they shot stray dogs, cows chewing the cud in peace or sick horses put out to pasture.

<audio id="mozilla-tts-quote" controls="controls">
  <source type="audio/mp3" src="{{"/assets/audio/mozilla-tts-stutter.mp3" | relative_url}}"></source>
  <p>Your browser does not support the audio element.</p>
</audio>

This was the only "bug" I really ran in to while using.  Otherwise, I was quite impressed with the performance.  While it doesn't quite sound as "human-like" as the paid alternatives, it is a huge step up from the eSpeak option.  The project will likely see more improvements as time goes on and their dataset improves.

## What's Next?

At some point I will likely want to explore the opposite direction: speech-to-text.  Mozilla apparently has things covered there with [DeepSpeech](https://github.com/mozilla/DeepSpeech), a project that appears to have significantly more support at the moment.