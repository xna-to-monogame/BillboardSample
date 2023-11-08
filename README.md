> ⚠ This sample is currently not working in MonoGame 3.8.1  See [Issue #1](https://github.com/xna-to-monogame/BillboardSample/issues/1) for more information on why and if you would like solve it.

# Billboard Sample

This sample shows how to efficiently render large numbers of billboard sprites by using a vertex shader to perform the billboard computations entirely on the GPU.

## Sample Overview

A billboard is a way of faking complex objects without bothering to render a full 3D model. The idea is that where the 3D object would have been, you simply render a 2D sprite with a picture of the object textured onto it. If the player moves around to look at the object from a different direction, you rotate the sprite so that it will always face toward the camera, thus preventing them from ever seeing it "edge on" and being able to tell that it is just a 2D image.

This technique is obviously not as convincing as rendering a true 3D model would be, but it can be much faster, especially for large numbers of objects such as the field of grass shown in this sample. By moving the camera around, you can get an idea of what situations the billboards look good in, and also where the illusion starts to break down.

Because this billboard implementation runs entirely in the vertex shader, there is no additional CPU load from rendering billboards compared to normal static geometry. This makes it feasible to render very large numbers of billboards at a good frame rate.

## Sample Controls

This sample uses the following keyboard and gamepad controls.

| Action            | Keyboard control                                  | Gamepad control         |
| ----------------- | ------------------------------------------------- | ----------------------- |
| Rotate the camera | UP ARROW, DOWN ARROW, LEFT ARROW, and RIGHT ARROW | Right thumb stick       |
| Move camera       | **W** (forward), **S** (backward)                 | Left thumb stick        |
| Strafe            | **A** and **D**                                   |                         |
| Reset the camera  | **R**                                             | Right thumb stick press |
| Exit              | ESC or ALT+F4                                     | **BACK**                |

## How the Sample Works

The **VegetationProcessor** takes in a simple landscape mesh and randomly creates billboard polygons scattered across it. These new polygons are set to render using the Billboard.fx effect, and a number of different billboard textures and sizes are randomly selected.

In the vertex shader, the billboard effect calculates vertex positions by examining the view matrix to ensure the billboard sprite will always face toward the camera. It also uses a per-billboard random number to make each billboard a slightly different shape, and to randomly mirror the textures on half of the billboards, adding variety to the results. Finally, it uses a sine wave pattern to make the top two vertices of each billboard sway, simulating the effect of wind on the vegetation. The amount of this swaying is controlled by an effect parameter, so some types of billboard can be set to sway more than others, or to remain entirely still.

Billboard lighting is calculated using the normal of the underlying landscape geometry. This means that vegetation will be lighter or darker depending on the angle of the hill that it is growing on, and will always be lit consistently with the underlying surface. In a more sophisticated landscape engine, it would be possible to make each billboard sample into the landscape texture to automatically pick up an appropriate color from the ground below it.

The billboards are rendered in two passes by using different alpha test and depth buffer settings to achieve almost correct depth sorting of alpha blended sprites without requiring them to be depth sorted on the CPU first. The comments in the **BillboardGame.Draw** method explain this technique in detail.

Note that although the billboard effect is included as part of the Game Studio project file, it is not part of the content project. This effect will be built automatically because it is referenced by the **VegetationProcessor**, so there is no need for it to be duplicated in the content project. It was only included here so you can easily locate the file for viewing or editing the effect code.

© 2010 Microsoft Corporation. All rights reserved.