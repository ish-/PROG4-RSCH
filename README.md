# RSCH

## Introduction

In this project, I will explore ways for custom post-processing in Unreal and  how its Post Process Material allows developers to inject custom logic into the rendering pipeline, leveraging existing buffers (SceneColor, SceneDepth, Velocity, etc) and external textures to create feedback loops utilizing Render Targets to store the previous frame’s image and feed it back into the current frame, focusing initially on creating a simple trails effect and gradually extending it with more advanced features like displacement driven by various render buffers, datamosh and glitches.

The end goal is to produce a module or pack of tools that can be comfortably and efficiently reused in different Unreal Engine scenes. This research aims to investigate existing methods and experiment with new approaches.

![](docs/imgs/Exmpl-0.png)

## State of the Art

![](docs/videos/2025-04-21-11-08-59.png)

The video feedback effect—where a video camera captures its own output displayed on a monitor—has evolved from an unintended technical glitch to a significant artistic and cultural phenomenon.

Discovered shortly after the invention of the first video recorder by Charlie Ginsburg in 1956, video feedback was initially seen as a problem. Technicians avoided it because pointing a camera at its own monitor could overload early video pickups, potentially damaging equipment and causing screen burn-in.

In the 1960s, artists began to explore video feedback's creative potential.By the 1970s and 1980s, video feedback found its way into mainstream media. Notably, the opening title sequence of the British TV series Doctor Who (1963–1973) utilized a technique known as "howl-around," a form of video feedback. Music videos like Queen's "Bohemian Rhapsody" (1975) and Earth, Wind & Fire's "September" (1978) also employed feedback effects to create visually captivating experiences.

Analgue
[How to make Video Feedback Art](https://www.youtube.com/watch?v=hAdLe841qYw)

Cellular automata (CAs) are mathematical models used to simulate complex systems using simple, local rules. Just like video feedback loops create intricate visual patterns from a recursion, generating emergent patterns.

<img src="https://i.ytimg.com/vi/xP5-iIeKXE8/maxresdefault.jpg"/>
[![Watch the video](https://img.youtube.com/vi/nTQUwghvy5Q/default.jpg)](https://youtu.be/nTQUwghvy5Q)
## Approach

### Modular Structure
- **`M_PP_Feedback`** – A material that blends the last frame with the current one.
- **`AC_CameraCapture`** – The main component for the pawn that dynamically creates and renders RenderTargets while adjusting the material of the post-process volume.

### User Project Structure
- **PostProcess Volume** with an empty slot for the `M_PP_Feedback` material in the pipeline.
- **SceneCapture2D** as a child of the main camera, precisely aligned with its parent.

### Initialization

#### `AC_CameraCapture` Parameters
<img src="docs/imgs/AC_CameraCapture-0.png" width="49%"/>

- **Cache Size** – The number of RenderTargets to be created.
- **Cache Offset** – Defines the delay in frames.
- **Highest Dimension** – Sets a resolution limit to avoid excessive processing with 2K textures. By default, the component creates scaled `1280` textures, or full-size if set to `0`.
- **Copy to Render Target** *(optional)* – Assign a RenderTarget if you want to use it as a model texture, for example.

#### `SceneCapture2D` Parameters
<img src="docs/imgs/SceneCapture2D-0.png" width="49%"/>

Since all operations are handled manually, any automatic actions should be disabled. It’s not immediately obvious, but "Persist State" is not enabled by default—unlike the standard camera settings. I'll experiment with other "Final Color" buffers in the future.

## Install
Migrate **ThirdPerson**, **Characters**, **StarterContent**, **LevelPrototyping** from another project or use symlinks. Keep it immutable!

## WIP

<img src="docs/imgs/Exmpl-1.png"/>
<img src="docs/imgs/Exmpl-2.png"/>