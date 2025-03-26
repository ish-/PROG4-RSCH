# RSCH

## Introduction

In this project, I will explore post-processing feedback FXs in Unreal Engine, focusing initially on creating a simple trails effect and gradually extending it with more advanced features like displacement driven by various render buffers, datamosh and glitches. The end goal is to produce a module or pack of tools that can be comfortably and efficiently reused in different Unreal Engine scenes. This research aims to investigate existing methods and experiment with new approaches.

<img src="docs/imgs/Exmpl-0.png" width="49%"/>

## State of the Art

Post-processing feedback effects have been used in realtime audio-visual application, artistic renders and many games to achieve distinctive looks ranging from ghostly trails to psychedelic distortions. Unreal Engine’s Post Process Material allows developers to inject custom logic into the rendering pipeline, leveraging existing buffers (SceneColor, SceneDepth, etc) and external textures to create feedback loops utilizing Render Targets to store the previous frame’s image and feed it back into the current frame, generating a trail-like or echo effect.

For now, I haven't found any specific research or tutorials on this topic, so I've started implementing it based on my own understanding.  

## Approach  

### Modular Structure  
- **`M_PP_Feedback`** – A material that blends the last frame with the current one. It is designed to be easily copied and extended within TouchDesigner.  
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