# Neural Network Visualizer

A 3D visualization tool for ONNX models. Drop in your model file and see what's actually happening inside.

## What it does

Takes your ONNX model and renders it as an interactive 3D scene. You can see each layer, watch data flow through the network, and actually understand what's going on instead of just staring at logs.

Built this just as a hobby project.

## Features

- **3D layer visualization** - Conv layers, linear layers, pooling, activations, all rendered in 3D
- **Animated data flow** - Watch activations propagate through the network
- **Interactive** - Click layers to inspect them, rotate/pan the camera
- **Convolutional kernel animation** - See the kernels scan across feature maps
- **No server needed** - Runs entirely in your browser

The animation speed adjusts based on layer type. Conv layers are slower so you can actually see what's happening.

## How to use

1. Open `index.html` in a browser (or visit the GitHub Pages site)
2. Drag and drop your `.onnx` file onto the page
3. Wait a second while it parses
4. Done

### Controls

- Left click + drag to rotate
- Right click + drag to pan
- Scroll to zoom
- Arrow buttons on the right to navigate
- Play button starts the animation
- Click any layer to highlight it

## Technical stuff

Uses Three.js for rendering and protobuf.js for parsing ONNX files. The parser tries to decode the protobuf schema first, but if that fails (thanks GitHub Pages CORS issues), there's a fallback that does basic binary pattern matching.

The fallback parser isn't perfect. It looks for operation type strings in the binary data and tries to filter out false positives. Works well enough for most models I've tested.

### Why the fallback parser exists

Originally just used protobuf.js with the external ONNX schema. Worked great locally. Deployed to GitHub Pages and suddenly the external schema URL returns 404. Cool.

So now the protobuf schema is included locally (`onnx.proto`), and there's still a fallback just in case.

## Known issues

- Really large models (500+ layers) get sampled down for performance
- The fallback parser can still detect too many layers on some models, so there's a cap at 300
- Doesn't handle every single ONNX operation type (but covers the common ones)

## Files

- `index.html` - Everything (yeah, it's all one file)
- `onnx.proto` - Local copy of the ONNX protobuf schema

## Models tested

Works with YOLOv8n and various custom models. If your model doesn't parse correctly, check the console. The error messages are actually helpful.

## Development

Just open `index.html`. That's it. No build process, no dependencies to install.

If you want to test locally with file:// protocol, some browsers block that. Python's http.server works fine:

```bash
python -m http.server 8000
```

Then open `http://localhost:8000`

## License

Do whatever you want with it.
