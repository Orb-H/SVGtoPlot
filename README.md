# SVGtoPlot
#### Make paths-only-SVG to a parametric function with Fourier Transformation.

Development will be very slow 🤔🤔
<hr/>
 
### Plan to implement SVGtoPlot
  1. For given path, give range of parameter `t` from `k/n` to `(k+1)/n`.
  2. Make parametric equation for each path.
  3. Separate x-function and y-function.
  4. For each component, combine and execute FT with given accuracy and the number of terms of the result of FT.
 


### Description of commands in `<path d="">`
 - M
   - Usage: `M x y` or `m dx dy`
   - Action: **Move** the starting point to given position
 - L
   - Usage: `L x y` or `l dx dy`
   - Action: Draws a **line** from the last point to given position.
 - H
   - Usage: `H x` or `h dx`
   - Action: Draws a **horizontal line** from the last point to the point with given x-coordinate.
 - V
   - Usage: `V y` or `v dy`
   - Action: Draws a **vertical line** from the last point to the point with given y-coordinate.
 - Z
   - Usage: `Z` or `z`
   - Action: **Closes** the path. It means this draws a line from the last point to the starting point.
 - C
   - Usage: `C x1 y1, x2 y2, x y` or `c dx1 dy1, dx2 dy2, dx dy`
   - Action: Draws a **cubic bezier curve** from the last point to the given point(`x y`), with given two control points(`x1 y1, x2 y2`).
 - S
   - Usage: `S x2 y2, x y` or `s dx2 dy2, dx dy`
   - Action: Draws a **continued cubic bezier curve** from the last point to the given point(`x y`), with one control point used before and new given control point(`x2 y2`).
 - Q
   - Usage: `Q x1 y1, x y` or `q dx1 dy1, dx dy`
   - Action: Draws a **quadratic bezier curve** from the last point to the given point(`x y`), with given control point(`x1 y1`).
 - T
   - Usage: `T x y` or `t dx dy`
   - Action: Draws a **continued quadratic bezier curve** from the last point to the given point, with the control point used before.
 - A
   - Usage: `A rx ry x-rotate-angle large-arc-flag sweep-flag x y` or `a rx ry x-rotate-angle large-arc-flag sweep-flag dx dy`
   - Action: Draws a **arc** on an oval with given radius, angle, and flag from the last point to the given point(`x y`).
   
### Building equations of commands
  - M
  
    <img src="https://latex.codecogs.com/gif.latex?\inline&space;h(0)=(f(0),g(0))=(x,y)" title="equation" />
  - L
  
    <img src="https://latex.codecogs.com/gif.latex?\inline&space;h(t)=(f(t),g(t))=(u(k+1-nt)+x(nt-k),v(k+1-nt)+y(nt-k))" title="equation not loaded" />
  - H
    - Using the equation of L, equation for H and V is easily derivable.
    
    <img src="https://latex.codecogs.com/gif.latex?\inline&space;h(t)=(f(t),g(t))=(u(k+1-nt)+x(nt-k),v)" title="equation not loaded" />
  - V
  
    <img src="https://latex.codecogs.com/gif.latex?\inline&space;h(t)=(f(t),g(t))=(u,v(k+1-nt)+y(nt-k))" title="equation not loaded" />
  - Z
  
    <img src="https://latex.codecogs.com/gif.latex?\inline&space;h(t)=(f(t),g(t))=(u(k+1-nt)+u_{0}(nt-k),v(k+1-nt)+v_{0}(nt-k))" title="equation not loaded" />
  - Q
  
    <img src="https://latex.codecogs.com/gif.latex?\inline&space;h(t)=(f(t),g(t))=(u(k+1-nt)^{2}+2x_{1}(nt-k)(k+1-nt)+x(nt-k)^{2},v(k+1-nt)^{2}+2y_{1}(nt-k)(k+1-nt)+y(nt-k)^{2})" title="equation not loaded" />
  - T
  
    <img src="https://latex.codecogs.com/gif.latex?\inline&space;h(t)=(f(t),g(t))=(u(k+1-nt)^{2}+2u_{1}(nt-k)(k+1-nt)+x(nt-k)^{2},v(k+1-nt)^{2}+2v_{1}(nt-k)(k+1-nt)+y(nt-k)^{2})" title="equation not loaded" />
  - C
  
    <img src="https://latex.codecogs.com/gif.latex?\inline&space;h(t)=(f(t),g(t))=(u(k+1-nt)^{3}+3x_{1}(nt-k)(k+1-nt)^{2}+3x_{2}(nt-k)^{2}(k+1-nt)+x(nt-k)^{3},v(k+1-nt)^{3}+3y_{1}(nt-k)(k+1-nt)^{2}+3y_{2}(nt-k)^{2}(k+1-nt)+v(nt-k)^{3})" title="equation not loaded" />
  - S
  
    <img src="https://latex.codecogs.com/gif.latex?\inline&space;h(t)=(f(t),g(t))=(u(k+1-nt)^{3}+3u_{1}(nt-k)(k+1-nt)^{2}+3x_{2}(nt-k)^{2}(k+1-nt)+x(nt-k)^{3},v(k+1-nt)^{3}+3v_{1}(nt-k)(k+1-nt)^{2}+3y_{2}(nt-k)^{2}(k+1-nt)+v(nt-k)^{3})" title="equation not loaded" />
  - A
  
    TODO.
