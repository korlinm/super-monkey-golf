# super-monkey-golf

A browser-based mini golf game built from scratch using raw WebGL2 — no game engine, no canvas API, no three.js. The stage tilts in response to mouse movement, rolling the ball forward. Inspired by the feel of Super Monkey Ball.

**[Play it here →](https://korlinm.github.io/super-monkey-golf)**

---

## How to play

- **Move your mouse up** to tilt the stage forward and accelerate
- **Move your mouse down** to flatten or reverse the slope
- **Left / right** to steer
- **Esc** to pause
- Collect the **banana** for a speed boost
- Fall off the edge and you respawn at the start
- Reach the **yellow goal marker** to advance

---

## Architecture

Everything lives in a single `index.html` file — no build step, no bundler, no dependencies beyond [gl-matrix](https://glmatrix.net/) loaded from a CDN. It runs directly from the filesystem or GitHub Pages.

The code is organised as a set of ES5-style classes, each with a single clear responsibility. Reading top to bottom, the file layers up from raw GPU access to gameplay logic:

```
WebGLContext        canvas + WebGL2 handle
Shader              GLSL compile, link, uniform cache
VERT / FRAG         Phong lighting shader with checkerboard grid support
GeometryFactory     buildBox(), buildSphere() — raw Float32Array geometry
VAO                 uploads geometry to GPU, owns VBOs + EBO
Mesh                VAO + world transform (position, rotation, scale)
RigidBody           position, velocity, semi-implicit Euler integration
PhysicsWorld        gravity tilt, sphere-vs-AABB + sphere-vs-plane resolution
Camera              smooth follow-cam, perspective projection
Renderer            Phong forward pass, stage tilt matrix
InputManager        Pointer Lock API, raw mouse delta accumulation
Level1              hole-course stage (two gaps, snake path)
Level2              ramp + banana stage
HUD                 HTML timer overlay
Game                top-level loop, level progression, pause system
```