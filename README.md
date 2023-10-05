# playback-audio-controller

`playback-audio-controller` is a JavaScript library that provides a simple interface for controlling audio playback and analyzing audio frequencies in the browser. It utilizes the Web Audio API to manage audio playback and perform frequency analysis.

## Installation

You can install the library via npm:

```bash
npm install playback-audio-controller
```

## Usage

### Importing the Library

You can import the `AudioController` class as follows:

```javascript
const { AudioController } = require("playback-audio-controller");
```

### Loading an Audio File

To load an audio file for playback, create an instance of `AudioController` and use the `load` method:

```javascript
const audioController = new AudioController();
const audioFileUrl = "your_audio_file.mp3";

audioController.load(audioFileUrl).then(() => {
    // The audio file is loaded and ready to be played or analyzed.
});
```

### Initializing the Audio Context

Before playing or analyzing audio, you need to initialize the audio context. You can use the `initialize` method:

```javascript
audioController.initialize().then(() => {
    // The audio context is now initialized and ready to use.
});
```

### Playing Audio

To start playing the loaded audio file, use the `start` method:

```javascript
audioController.start();
```

You can also toggle between play and stop states using the `toggle` method:

```javascript
audioController.toggle(); // Toggles between play and stop.
```

### Stopping Audio

To stop the audio playback, use the `stop` method:

```javascript
audioController.stop();
```

### Analyzing Audio Frequency

You can analyze the frequency data of the audio using the `analyzeFrequency` method. Here's an example of how to use it:

```javascript
const options = {
    fftSize: 2048, // FFT size for frequency analysis (default: 2048)
    minFrequency: 20, // Minimum frequency to analyze in Hz (default: 0)
    maxFrequency: 20000, // Maximum frequency to analyze in Hz (default: half the sample rate)
};

const frequencyData = audioController.analyzeFrequency(options);

// `frequencyData` is a Uint8Array containing frequency data within the specified range.
```

## Example

Here's a complete example of how to use `playback-audio-controller`:

```html
<button id="toggle-play" type="button">Toggle</button>

<script>
    const { AudioController } = require("playback-audio-controller");

    const audioController = new AudioController();
    const audioFileUrl = "your_audio_file.mp3";

    audioController.load(audioFileUrl).then(() => {
        audioController.initialize().then(() => {
            // Audio is loaded and context is initialized.

            document
                .getElementById("toggle-play")
                .addEventListener("click", () => {
                    audioController.toggle();
                });

            const logFrequencies = () => {
                setTimeout(() => {
                    // Analyze audio frequency in the range of 20Hz to 20000Hz.
                    const frequencyData = audioController.analyzeFrequency({
                        minFrequency: 20,
                        maxFrequency: 20000,
                    });

                    console.log("Frequency Data:", frequencyData);

                    logFrequencies();
                }, 100);
            };

            logFrequencies();
        });
    });
</script>
```
