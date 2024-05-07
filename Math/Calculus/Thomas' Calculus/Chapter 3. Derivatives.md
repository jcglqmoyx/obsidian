If $n$ is any real number, then
$${d \over{dx}} x^n = nx^{n-1}$$, for all $x$ where the powers $x^n$ and $x^{n-1}$ are defined.

---

${d \over {dx}} (e^x) = e^x$

${d \over {dx}}f(x)g(x) = f(x)g'(x)+f'(x)g(x)$

${d \over {dx}}[{f(x) \over g(x)}] = {{f'(x)g(x) - f(x)g'(x)} \over {g^2(x)}}$

---

${d \over {dx}}(sin(x)) = cos(x)$

${d \over {dx}}(cos(x)) = -sin(x)$

$\frac{d}{dx}(\tan(x)) = \sec^2(x)$

$\frac{d}{dx}(\sec(x)) = \sec(x)\tan(x)$

$\frac{d}{dx}(\csc(x)) = -\csc(x)\cot(x)$

$\frac{d}{dx}(\cot(x)) = -\csc^2(x)$

---

**The Chain Rule in Derivatives**

If `y = f(u)` and `u = g(x)`, then:

$$\frac{dy}{dx} = \frac{dy}{du} \cdot \frac{du}{dx}$$


---

**The Derivative Rule for Inverses**
If $f$ has an interval I as domain and $f ′(x)$ exists and is never zero on $I$, then $f^{-1}$ is differentiable at every point in its domain (the range of $f$ ). The value of $(f^{−1})′$ at a point b in the domain of $f^{-1}$ is the reciprocal of the value of $f ′$ at the point $a = f^{−1}(b)$:

$\frac{dx}{dy} = 1/ \frac{dy}{dx}$


---

${d \over {dx}}lnx = {1 \over x}, x > 0$

if $a > 0$, then $a^x$ is differentiable and
${d \over {dx}} a^x = a^x ln \ a$

---

${d \over dx}log_ax = {1 \over {x ln \ a}}, a > 0 , a \ne 1$

---
**Example** 

Find $dy \over dx$ if
$$ y = \frac{(x^2 + 1)(x + 3)^{1/2}}{x - 1}, \quad x > 1. $$
**Solution** We take the natural logarithm of both sides and simplify the result with the algebraic properties of logarithms:

$$ \ln y = \ln \left( \frac{(x^2 + 1)(x + 3)^{1/2}}{x - 1} \right) $$

$$ = \ln ((x^2 + 1)(x + 3)^{1/2}) - \ln (x - 1) \quad $$

$$ = \ln (x^2 + 1) + \ln (x + 3)^{1/2} - \ln (x - 1) \quad $$

$$ = \ln (x^2 + 1) + \frac{1}{2} \ln (x + 3) - \ln (x - 1) $$

We then take derivatives of both sides with respect to x:

$$ \frac{1}{y} \frac{dy}{dx} = \frac{1}{x^2 + 1} \cdot 2x + \frac{1}{2} \cdot \frac{1}{x + 3} - \frac{1}{x - 1} $$

**Next we solve for $dy \over dx$:**

$$ \frac{dy}{dx} = y \left( \frac{2x}{x^2 + 1} + \frac{1}{2x + 6} - \frac{1}{x - 1} \right) $$

**Finally, we substitute for y:**

$$ \frac{dy}{dx} = \frac{(x^2 + 1)(x + 3)^{1/2}}{x - 1} \left( \frac{2x}{x^2 + 1} + \frac{1}{2x + 6} - \frac{1}{x - 1} \right). $$