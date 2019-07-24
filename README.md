# SVGtoPlot

#### Make paths-only-SVG to a parametric function with Fourier Transformation.

Development will be very slow 🤔🤔

<hr/>

### Plan to implement SVGtoPlot

1.  For given path, give range of parameter `t` from `k/n` to `(k+1)/n` (or divide range by their lengths).
2.  Make parametric equation for each path.
3.  Separate x-function and y-function.
4.  For each component, combine and execute FT with given accuracy and the number of terms of the result of FT.

### Description of commands in `<path d="">`

-   M
    -   Usage: `M x y` or `m dx dy`
    -   Action: **Move** the starting point to given position
-   L
    -   Usage: `L x y` or `l dx dy`
    -   Action: Draws a **line** from the last point to given position.
-   H
    -   Usage: `H x` or `h dx`
    -   Action: Draws a **horizontal line** from the last point to the point with given x-coordinate.
-   V
    -   Usage: `V y` or `v dy`
    -   Action: Draws a **vertical line** from the last point to the point with given y-coordinate.
-   Z
    -   Usage: `Z` or `z`
    -   Action: **Closes** the path. It means this draws a line from the last point to the starting point.
-   C
    -   Usage: `C x1 y1, x2 y2, x y` or `c dx1 dy1, dx2 dy2, dx dy`
    -   Action: Draws a **cubic Bezier curve** from the last point to the given point(`x y`), with given two control points(`x1 y1, x2 y2`).
-   S
    -   Usage: `S x2 y2, x y` or `s dx2 dy2, dx dy`
    -   Action: Draws a **continued cubic Bezier curve** from the last point to the given point(`x y`), with one control point used before and new given control point(`x2 y2`).
-   Q
    -   Usage: `Q x1 y1, x y` or `q dx1 dy1, dx dy`
    -   Action: Draws a **quadratic Bezier curve** from the last point to the given point(`x y`), with given control point(`x1 y1`).
-   T
    -   Usage: `T x y` or `t dx dy`
    -   Action: Draws a **continued quadratic Bezier curve** from the last point to the given point, with the control point used before.
-   A

    -   Usage: `A rx ry x-rotate-angle large-arc-flag sweep-flag x y` or `a rx ry x-rotate-angle large-arc-flag sweep-flag dx dy`
    -   Action: Draws a **arc** on an oval with given radius, angle, and flag from the last point to the given point(`x y`).

### Building equations of commands

-   M

    -   Since M is called only at the first time, we can assume that starting point of graph is set to given position.

        <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;h(0)=(f(0),g(0))=(x,y)" title="equation not loaded" />

-   L

    -   Using fact that line segment from (a,b) to (c,d) can be written as <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;h(t)=(f(t),g(t))=(at+c(1-t),bt+d(1-t))" title="equation not loaded" /> where 0&lt;t&lt;1, we can derive equation for L command.

        <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;h(t)=(f(t),g(t))=((1-t)u+tx,(1-t)v+ty" title="equation not loaded" />

-   H

    -   Using the equation of L, equation for H and V is easily derivable.

        <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;h(t)=(f(t),g(t))=((1-t)u+tx,v)" title="equation not loaded" />

-   V

    -   Same with H.

        <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;h(t)=(f(t),g(t))=(u,(1-t)v+ty)" title="equation not loaded" />

-   Z

    -   It is exactly same with L, except destination point is set to the first point of path.

        <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;h(t)=(f(t),g(t))=((1-t)u+tu_0,(1-t)v+tv_0)" title="equation not loaded" />

-   Q

    -   For quadratic bezier curve, points on the curve is written as <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;P=(1-t)^2U+2t(1-t)X_1+t^2X" title="equation not loaded" /> where U is the last point, X<sub>1</sub> is a given control point, and X is the destination point.

        <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;h(t)=(f(t),g(t))=((1-t)^2u+2(1-t)tx_1+t^2x,(1-t)^2v+2(1-t)ty_{1}+t^2y)" title="equation not loaded" />

-   T

    -   Since continued, X<sub>1</sub> in Q is already set as U<sub>1</sub>, the last control point.

        <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;h(t)=(f(t),g(t))=((1-t)^2u+2(1-t)tu_1+t^2x,v(k+1-nt)^2+2v_1(nt-k)(k+1-nt)+y(nt-k)^2)" title="equation not loaded" />

-   C

    -   For cubic bezier curve, points on the curve is written as <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;P=(1-t)^3U+3t(1-t)^2X_{1}+3t^2(1-t)X_2+t^3X" title="equation not loaded" /> where U is the last point, X<sub>1</sub> and X<sub>2</sub> are given control points, and X is the destination point.

        <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;h(t)=(f(t),g(t))=((1-t)^3u+3(1-t)^2tx_1+3(1-t)t^2x_2+t^3x,(1-t)^3v+3(1-t)^2ty_1+3(1-t)t^2y_+t^3v)" title="equation not loaded" />

-   S

    -   Since continued, X<sub>1</sub> in C is already set as U<sub>1</sub>, the last control point.

        <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;h(t)=(f(t),g(t))=((1-t)^3u+3(1-t)^2tu_1+3(1-t)t^2x_2+t^3x,(1-t)^3v+3(1-t)^2tv_1+3(1-t)t^2y_2+t^3v)" title="equation not loaded" />

-   A

    -   Equation deriving

        For given radii a and b, equation for ellipse can be written as <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;\frac{(x-m)^2}{a^2}+\frac{(y-n)^2}{b^2}=1" title="equation not loaded" />, or in parametric form <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;\left\{\begin{matrix}x=m+a\cos\theta\\y=n+b\sin\theta\end{matrix}\right." title="equation not loaded" />. However, ellipse can be rotated by angle given in command. So rotation of axis is needed to manipulate equation.<br/>
        SVG path uses clockwise rotation, so negative value should be used. For given rotation angle χ, let φ ≡ (-χ) mod 180. Then φ will be used since ellipse with rotation angle χ has same phase with one with angle -φ. In addition, φ will have range \[0, π).<br />
        By rotation, new axis x' and y' is set to <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;\left\{\begin{matrix}x'=\frac{x-m+c(y-n)}{\sqrt{1+c^2}}\\y'=\frac{y-n-c(x-m)}{\sqrt{1+c^2}}\end{matrix}\right." title="equation not loaded" />, where <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;c=\tan\phi" title="equation not loaded" />, and new equation will be <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;\frac{(x-m+c(y-n))^2}{a^2}+\frac{(y-n-c(x-m))^2}{b^2}=1+c^2" title="equation not loaded" />, and <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;\left\{\begin{matrix}x'=m+a\cos\theta\cos\phi+b\sin\theta\sin\phi\\y'=n-a\cos\theta\sin\phi+b\sin\theta\cos\phi\end{matrix}\right." title="equation not loaded" /> in parametric form.<br/>
        Using the last point U as (u, v), given destination point X as (s, t), and equation above, new system is built as <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;\left\{\begin{matrix}u=m+a\cos\alpha\cos\phi+b\sin\alpha\sin\phi\\v=n-a\cos\alpha\sin\phi+b\sin\alpha\cos\phi\\s=m+a\cos\beta\cos\phi+b\sin\beta\sin\phi\\t=n-a\cos\beta\sin\phi+b\sin\beta\cos\phi\end{matrix}\right." title="equation not loaded" />. The variables to solve are m, n, α, and β. There will be two solutions for this system.<br/>
        By subtracting equation 1 and 3, following equation is derived. <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;u-s=a(\cos\alpha-\cos\beta)\cos\phi+b(\sin\alpha-\sin\beta)\sin\phi" title="equation not loaded" />. Samely with equation 2 and 4, <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;v-t=-a(\cos\alpha-\cos\beta)\sin\phi+b(\sin{\alpha}-\sin{\beta})\cos\phi" title="equation not loaded" />.<br/>
        By multiplying cosφ, sinφ to each equation and adding/subtracting well, two equations can be derived as following. <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;\left\{\begin{matrix}(u-s)\cos\phi-(v-t)\sin\phi=a(\cos\alpha-\cos\beta)\\(u-s)\sin\phi+(v-t)\cos\phi=b(\sin\alpha-\sin\beta)\end{matrix}\right." title="equation not loaded" /><br/>
        Using trigonometric conversion equation, two equations above can be derived into <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;\left\{\begin{matrix}-A=\sin\gamma\sin\delta\\B=\cos\gamma\sin\delta\end{matrix}\right." title="equation not loaded" />, where <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;\left\{\begin{matrix}A=\frac{(u-s)\cos\phi-(v-t)\sin\phi}{2a}\\B=\frac{(u-s)\sin\phi+(v-t)\cos\phi}{2b}\\\gamma=\frac{\alpha+\beta}{2}\\\delta=\frac{\alpha-\beta}{2}\end{matrix}\right." title="equation not loaded" /><br/>
        By dividing one of those equation by another one, a simple equation <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;\frac{A}{B}=-\tan\gamma" title="equation not loaded" /> is derived. Therefore <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;\gamma=-\tan^{-1}\frac{A}{B}" title="equation not loaded" />, and <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;\delta=\sin^{-1}\frac{B}{\cos(\tan^{-1}\frac{A}{B})}=\sin^{-1}(B\sec({\tan^{-1}\frac{A}{B}}))=\sin^{-1}(B\sqrt{1+\frac{A^2}{B^2}})=\sin^{-1}\sqrt{A^{2}+B^{2}}" title="equation not loaded" />.<br/>
        Since <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;\alpha=\gamma+\delta=\sin^{-1}\sqrt{A^2+B^2}-\tan^{-1}\frac{A}{B}" title="equation not loaded" />, <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;\cos\alpha=\cos(\gamma+\delta)=\cos\gamma\cos\delta-\sin\gamma\sin\delta=\frac{B}{\sqrt{A^2+B^2}}\sqrt{1-(A^2+B^2)}+\frac{A}{\sqrt{A^2+B^2}}\sqrt{A^2+B^2}=A+B\frac{\sqrt{1-(A^2+B^2)}}{\sqrt{A^2+B^2}}" title="equation not loaded" /> and <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;\sin\alpha=\sin(\gamma+\delta)=\sin\gamma\cos\delta+\cos\gamma\sin\delta=-\frac{A}{\sqrt{A^2+B^2}}\sqrt{1-(A^2+B^2)}+\frac{B}{\sqrt{A^2+B^2}}\sqrt{A^2+B^2}=B-A\frac{\sqrt{1-(A^2+B^2)}}{\sqrt{A^2+B^2}}" title="equation not loaded" />.<br />
        Using cosα and sinα, m and n can be written with a, b, y, v, s, and t like following. <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;m=u-a\cos\alpha\cos\phi-b\sin\alpha\sin\phi=u-aA\cos\phi-aB\frac{\sqrt{1-(A^2+B^2)}}{\sqrt{A^2+B^2}}\cos\phi-bB\sin\phi+bA\frac{\sqrt{1-(A^2+B^2)}}{\sqrt{A^2+B^2}}\sin\phi=\frac{u+s}{2}-aBX\cos\phi+bAX\sin\phi" title="equation not loaded" />, <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;n=v+a\cos\alpha\sin\phi-b\sin\alpha\cos\phi=v+aA\sin\phi+aB\frac{\sqrt{1-(A^2+B^2)}}{\sqrt{A^2+B^2}}\sin\phi-bB\cos\phi+bA\frac{\sqrt{1-(A^2+B^2)}}{\sqrt{A^2+B^2}}\cos\phi=\frac{v+t}{2}+aBX\sin\phi+bAX\cos\phi" title="equation not loaded" />, where <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;X=\frac{\sqrt{1-(A^2+B^2)}}{\sqrt{A^2+B^2}}" title="equation not loaded" /><br />
        So the equation will be <img src="https://latex.codecogs.com/gif.latex?\inline&space;\bg_white&space;h(t)=(f(t),g(t))=(m+a\cos\phi\cos((1-t)\alpha+t\beta)+b\sin\phi\sin((1-t)\alpha+t\beta),n-a\sin\phi\cos((1-t)\alpha+t\beta)+b\cos\phi\sin((1-t)\alpha+t\beta))" title="equation not loaded" /><br />
        However, there are sweep-flag and large-arc-flag, and these flags are needed to determine the only solution for arc. First, large-arc-flag will be used for constraint of (β-α) value. Next, the value (sweep-flag ^ large-arc-flag) will be used for determining the only center of ellipse.<br />
        To determine α and β, we have to define the range of both variables first.
        <iframe scrolling="no" title="Setting Range of α and β" src="https://www.geogebra.org/material/iframe/id/bpkbukmj/width/1890/height/874/border/888888/sfsb/true/smb/false/stb/false/stbh/false/ai/false/asb/false/sri/true/rc/false/ld/false/sdz/true/ctl/false" width="800px" height="600px" style="border:0px;"> </iframe>
