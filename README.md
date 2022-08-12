# homebrewdj-launchpad-driver - Launchpad Mini (MK2) Driver for Node from homebrewDJ (TBA)

[![Node.js Package](https://github.com/thallosaurus/homebrewdj-launchpad-driver/actions/workflows/npm-publish.yml/badge.svg)](https://github.com/thallosaurus/homebrewdj-launchpad-driver/actions/workflows/npm-publish.yml)

## About
This Module implements a Interface between a Launchpad Mini MK2 and Javascript Events with node-midi. See the example below on how to use this module


## Installation
### From NPM
```npm i homebrewdj-launchpad-driver```

### From Source
```npm i https://github.com/thallosaurus/homebrewdj-launchpad-driver.git```

## Requirements
Be sure that you have this installed beforehand the installationprocess will fail
- OSX
  - Some version of Xcode (or Command Line Tools)
  - Python (for node-gyp)

- Windows
    -   Microsoft Visual C++ (the Express edition works fine)
    -   Python (for node-gyp)

- Linux
    - A C++ compiler
    - You must have installed and configured ALSA. Without it this module will NOT build.
    - Install the libasound2-dev package.
    - Python (for node-gyp)

If you use Windows, you can also use ```npm install --global windows-build-tools```

## Examples
```javascript
const { hDJMidiRecv, hDJRecvEvent, hDJMidiOutputBuffer, getRandomColor, ButtonId } = require("homebrewdj-launchpad-driver");

let h = new hDJMidiRecv();

//Get all connected MIDI Devices
console.log("available devices:");
console.log(h.enumeratePorts());

//For example, we use input 0 and output 0
h.connect(0, 0);

h.on(hDJRecvEvent.MatrixEvent, (data) => {
    console.log("Someone pressed the matrix at", data.pos);
});

h.on(hDJRecvEvent.ButtonPress, (data) => {
    console.log("Someone pressed the button", data.button);
    console.log(ButtonId[data.button]);
});

//Fill a new Array with some random colors
let data = new Uint8Array(hDJMidiOutputBuffer.width * hDJMidiOutputBuffer.height).map(e => {
    return getRandomColor();
});

for (let y = 0; y < hDJMidiOutputBuffer.height; y++) {
    for (let x = 0; x < hDJMidiOutputBuffer.width; x++) {
        let i = x * hDJMidiOutputBuffer.height + y;

        //write buffer onto transfer buffer
        h.boundBuffer.setXY(data[i], {
            x: x,
            y: y
        });
    }
}

setTimeout(() => {
    h.boundBuffer.flush();
}, 5000);
```

# TODO
- Get Upper Button Row working