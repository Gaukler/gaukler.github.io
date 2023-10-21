---
layout: post
---

A while ago, I investigated real time hair shading approaches. I looked into the Unreal shading model presented by Karis **[1]** and the additions from 
the Frostbite approach presented by Tafuri **[2]**.
Both approaches are approximations of a model developed at Weta Digital and published by d’Eon et al. **[3]**.
This model, in turn, is based on the seminal paper by Marschner et al. **[4]**.
 
When I looked into the Marschner model, I focused on the high-level concepts and glossed over
some of the finer details. Recently, I returned to the topic to dig a bit deeper into the things I omitted.

This blogpost is not intended to give a full overview of the math behind the Marschner model. There are a lot of moving parts, and the original paper does a great 
job of explaining and deriving them. Instead, I will focus on some insights that helped me improve my intuition, go into some math details that were glossed over
and clear up a few small errors from the original paper.

I am by no means an expert on this topic, so take everything I write with a grain of salt.
If you spot any errors or have suggestions for improvement, feel free to reach out to me. 

# High Level Overview

The Marschner model models hair fibers as cylinders with a rough surface composed of tilted cuticle scales. Three paths of light through the fiber are considered.
First, reflection on the surface, denoted **R**. Second, transmission through the fiber, denoted **TT**. Finally, transmission through the fiber, followed by a reflection 
on the inside, followed by a second transmission, denoted **TRT**.

The R path causes the uncolored, primary highlight on the hair. The TT path is responsible for the appearance of backlit hair and is particularly visible on lighter hair.
The TRT path causes a secondary, rougher, colored highlight. It is shifted compared to the primary one.

The coordinate system is illustrated in *Figure 3* in **[4]**. The fiber direction $$u$$ points from the hair root towards the tip. The vectors $$w$$ and $$v$$ form 
an orthonormal basis with $$u$$ and span the normal plane. $$\omega_i$$ points towards the incidence direction and $$\omega_r$$ is the outgoing direction. 
$$\phi_i$$ and $$\phi_r$$ are the incident and outgoing vector angles on the normal plane and define the vectors' rotation around the hair.
$$\theta_i$$ and $$\theta_r$$ are the angles between the vectors and the normal plane, also known as the inclination. The angles of the halfway vector are 
$$\theta_h = (\theta_i + \theta_r) / 2$$ and $$\phi_h = (\phi_i + \phi_r) / 2$$. In addition, we define the difference angle $$\theta_d = (\theta_r - \theta_i) / 2$$
and the relative azimuth $$\phi = \phi_r - \phi_i$$.

The scattering function $$ S $$ relates incoming to outgoing light. It is composed of the longitudinal scattering function $$ M $$ and the azimuthal scattering function $$ N $$.
These functions are defined once for each path:
$$ M_R $$, $$ M_{TT} $$ and $$ M_{TRT} $$ and their counterparts $$ N_R $$, $$ N_{TT} $$ and $$ N_{TRT} $$. The entire scattering function is defined as:

\begin{equation}
S = M_R N_R / cos^2(\theta_d) + M_{TT} N_{TT} / cos^2(\theta_d) + M_{TRT} N_{TRT} / cos^2(\theta_d)
\end{equation}

Note that I omitted the parameters from the Marschner paper for brevity. The division by $$cos^2(\theta_d)$$ is a normalization factor that accounts for the projected solid 
angle of the specular cone. However, this assumes $$\theta_i=\theta_r$$. This does not hold when taking surface roughness into 
account and is therefore only an approximation, as noted in **[3]**.

The longitudinal function $$ M $$ is quite straightforward. It models a reflection, where roughness is modeled by centering a Gaussian around the reflection direction. 
It is shifted, depending on the path, due to the cuticle tilt.
The only detail that surprised me at first glance was that the same input vector $$ \theta_h $$ was used as the input for all paths,
despite the TT path exiting on the other side of the cylinder. This confusion came from the fact that the geometry of the longitudinal scattering is often visualized using a
2D side view, where the TT path passes through the cylinder and exits on the other side, compared to the R and TRT paths.
However, this is cleared up by visualizing what is happening in 3D.
The longitudinal scattering function models a specular cone. A ray passing through the fiber will simply end up inside it's corresponding cone nevertheless.

# Azimuthal Scattering

The azimuthal scattering term is made up of two parts. The first is attenuation $$ A $$. The attenuation accounts for the energy lost due to reflection and
refraction at the surface and the volume absorption inside the fiber. The exact form of $$ A $$ is dependent on the path of the light:
R, TT or TRT.

The other component accounts for the intensity of the path. This one took me a bit of effort to understand, which is why I want to focus on it. The full azimuthal scattering
function is:

\begin{equation}
N = A |2 \frac{d\phi}{dh}(p,h(p,r,\phi))|^{-1} \label{eq:azimuthal_scattering}
\end{equation}

By projecting all vectors into the normal plane, we reduce the path intensity term to reflection on a smooth circle. 
We account for the projection by using a modified index of refraction $$ \eta' $$ according to Bravais’s law.
Since we are dealing with a far field model, we assume that the incoming light is parallel and uniform across the entire cross section of the circle.
The multiplication by two accounts for the fact that we are working with a unit circle with a radius of one, so the diameter equals two 
and irradiance is normalized with respect to the observed area.

Let's look at the reflection of parallel rays on the surface of a circle.
Note that I left out the lower half of the incoming rays and their reflection to keep the figure more readable.
In actuality, the lower half would be a mirror image of the upper one.

<div style="text-align:center">
<img src = "/images/Marschner/azimuthalPathR.svg" alt="Cylinder reflection distribution"/>
</div>

**Edit**: The initial version of this figure showed the normals instead of the reflection. Credit goes to Dolkar for pointing out the error. 

The incoming rays are uniformly distributed across the height $$h$$. After the reflection they are non-uniform with regard to their angular distribution. They become sparser
towards the top of the circle. Since the incoming light is uniform across the cross section of the circle, less light is reflected towards the top (and bottom).
The main component of the path intensity is the ratio between the change in height of the incoming rays and the change in angle of the reflected rays. This is formalized in equations *(5)* and *(6)*
of the Marschner paper **[4]**. 

Another way to put this is that we are computing how much the angle of the reflected light is changing with respect to the height.
For rays close to the center, this change is small, and for rays towards the top and bottom, this is higher. This gives intuition as to why
we use the inverse of the ratio of the derivatives: a small change in angle relative to the height should result in more reflected radiance. 
A higher change should result in less reflected radiance since the same irradiance is reflected into a larger angular segment.
*Figure 9* from the Marschner paper contains another illustration of the geometric interpretation of $$ d\phi $$ and $$ dh $$.

I found that visualizing the paths through the circle helped a lot with building intuition. What pointed me in this direction was an old video recording of a presentation on 
the paper from the ACM library **[5]**. Unfortunately, the recording does not contain the slides. However, the speaker's descriptions pointed me towards visualizing 
and thinking about the azimuthal scattering in this way.

Let's derive the azimuthal scattering for the **R** path. We start with the equation of the angle 
$$\phi$$ between the incident and outgoing rays, as given in **[4]**:

\begin{equation}
\phi = 2p\gamma_t - 2\gamma_i +p\pi \label{eq:phi}
\end{equation}

$$\gamma_i$$ is the angle of incidence, the angle between the incoming ray and the normal of the circle. $$\gamma_t$$ is the equivalent of the refraction vector.
 $$p$$ is the number of internal path segments. Since we are looking at the reflection, there are none, so $$p=0$$:

\begin{equation}
\phi = - 2\gamma_i \label{eq:phi_gamma}
\end{equation}

We want to calculate the change of $$\phi$$ in regard to $$h$$, so we have to replace $$\gamma_i$$ with $$h$$.
The relationship between the two is directly taken from the definition of the unit circle:

\begin{equation}
sin(\gamma_i) = h \label{eq:gamma_i_to_h}
\end{equation}

It follows:

\begin{equation}
\gamma_i = arcsin(h) \label{eq:gamma_i}
\end{equation}

Putting this back into equation \eqref{eq:phi_gamma}:

\begin{equation}
\phi = -2 arcsin(h)
\end{equation}

Taking the derivative in regard to $$h$$, keeping in mind that $$\frac d{dx} arcsin(x) = \frac1{\sqrt{1-x^2}}$$:

\begin{equation}
\frac{d\phi}{dh} = -2 \frac1{\sqrt{1-h^2}}
\end{equation}

The next step is to express this in regards to our shading input $$\phi$$. We will use our previous definition \eqref{eq:gamma_i_to_h} to replace $$h$$ by $$\gamma_i$$ again:

\begin{equation}
\frac{d\phi}{dh} = -2 \frac1{\sqrt{1-sin^2(\gamma_i)}}
\end{equation}

We can simplify this using the trigonometric identity $$cos(x) = \sqrt{1-sin^2(x)}$$:

\begin{equation}
\frac{d\phi}{dh} = -2 \frac1{cos(\gamma_i)}
\end{equation}

Now we solve our initial function \eqref{eq:phi_gamma} for $$\phi$$, which is simply $$\gamma_i = - \frac \phi 2$$, and plug this in:

\begin{equation}
\frac{d\phi}{dh} = -2 \frac1{cos(-\frac \phi 2)}
\end{equation}

Since $$cos(x) = cos(-x)$$ we drop the minus sign inside the cosine:

\begin{equation}
\frac{d\phi}{dh} = -2 \frac1{cos(\frac \phi 2)}
\end{equation}

We can plug this into our initial definition of $$N$$ \eqref{eq:azimuthal_scattering}:

\begin{equation}
N_R = A_R |2 (-2 \frac1{cos(\frac \phi 2)}) |^{-1}
\end{equation}

We simplify this by multiplying out the constant factors and dropping the minus sign inside the absolute value:

\begin{equation}
N_R = A_R |4 \frac1{cos(\frac \phi 2)}) |^{-1}
\end{equation}
 
Then we apply the power of minus one:

\begin{equation}
N_R = A_R |\frac1 4 cos(\frac \phi 2))|
\end{equation}

Finally, pulling out the fraction outside the absolute value gives:

\begin{equation}
N_R = A_R \frac1 4 |cos(\frac \phi 2))|
\end{equation}

This matches equation *(6)* from the Weta paper **[3]**. Note that they leave $$A_R$$ out of their equation, since they list it in the context of analyzing energy conservation
for a fiber that reflects all light, so in their case $$A_R = 1$$.

# Bravais Index Derivation

The Marschner paper includes a fairly detailed derivation of the Bravais Index that is used to obtain $$\eta'$$. However, it skips a few intermediate steps and contains
some minor errors. Luckily, there is an extended version of the appendix **[6]**. The paper appendix incorrectly states:

\begin{equation}
\eta cos \gamma = \eta' cos \delta \label{eq:gamma_delta_incorrect}
\end{equation}

$$\gamma$$ is the inclination of the incoming vector with regard to the normal plane. $$\delta$$ is the corresponding inclination of the refracted vector.
Solving equation \eqref{eq:gamma_delta_incorrect} for $$\eta'$$, we get:

\begin{equation}
\eta' = \frac{\eta cos \gamma}{cos \delta} \label{eq:eta_prime_incorrect}
\end{equation}

Compare this to the extended appendix **[6]** that states:

\begin{equation}
\eta' = \frac{\eta cos \delta}{cos \gamma} \label{eq:eta_prime_correct}
\end{equation}

$$ \gamma $$ and $$ \delta $$ are swapped. Initially, I was unsure which version was correct. I wrote a small Octave script to manually compute
some refraction vectors and their projections to confirm that the extended appendix is the correct one. 
Using the script to check the paper equations used for deriving equation \eqref{eq:eta_prime_incorrect}, I found the following two statements to be incorrect as well:

\begin{equation}
\sin \theta_i' = sin \theta_i cos \gamma
\end{equation}

\begin{equation}
\sin \theta_t' = sin \theta_t cos \delta
\end{equation}

The extended appendix does not go into great detail how it arrives at equation \eqref{eq:eta_prime_correct}. Let's look into this next.
As stated in the paper appendix, Snell's law applies in the vertical normal plane:

\begin{equation}
sin \theta_i' = \eta' sin \theta_t' \label{eq:snells_law_projected}
\end{equation}

$$\theta_i'$$ and $$\theta_t'$$ are the inclinations of the incoming and refracted vectors after projecting them onto the normal plane. 
In addition, the following relationship is given in the paper appendix as part of *Figure 16 (c)*:

\begin{equation}
s_t' = \frac{s_i'}{\eta}
\end{equation}

Equivalent:

\begin{equation}
\eta = \frac{s_i'}{s_t'} \label{eq:eta_ratio}
\end{equation}

$$s_i'$$ is the length of the incoming vector projected onto the normal plane and then projected onto the surface. $$s_t'$$ is the refracted vector's counterpart.

Looking at the geometry of *Figure 16 (c)* in the paper appendix **[4]**, we can use some trigonometry to find some useful relations. We know that the incoming vector $$v_i$$
is a direction and therefore a unit vector, so $$||v_i|| = 1$$. We can use this to compute the length of the projection of $$v_i$$ on the normal plane, denoted $$v_i'$$:

\begin{equation}
cos\gamma = \frac{||v_i'||}{||v_i||}
\end{equation}

\begin{equation}
cos\gamma = ||v_i'||
\end{equation}

The top of the triangle between $$v_i'$$ and it's projection on the normal $$n$$ is equal to $$s_i'$$. This gives us:

\begin{equation}
sin \theta_i' = \frac{s_i'}{||v_i'||}
\end{equation}

\begin{equation}
sin \theta_i' = \frac{s_i'}{cos \gamma} \label{eq:incoming_projection_and_inclination}
\end{equation}

We can apply the equivalent steps to the refracted vector to obtain:

\begin{equation}
sin \theta_t' = \frac{s_t'}{cos \delta} \label{eq:refracted_projection_and_inclination}
\end{equation}

Substituting \eqref{eq:incoming_projection_and_inclination} and \eqref{eq:refracted_projection_and_inclination} into equation \eqref{eq:snells_law_projected}:

\begin{equation}
\eta' \frac{s_t'}{cos \delta} = \frac{s_i'}{cos \gamma}
\end{equation}

\begin{equation}
\eta' = \frac{s_i'}{s_t'} \frac{cos \delta}{cos \gamma}
\end{equation}

Substituting $$\eta$$ into this using equation \eqref{eq:eta_ratio} yields equation \eqref{eq:eta_prime_correct}. As a reminder:

$$
\eta' = \eta \frac{cos \delta}{cos \gamma}
$$

The next step is to get rid of the $$cos\delta$$ term. The extended appendix observes:

\begin{equation}
\eta sin \delta = sin \gamma
\end{equation}

We transform this for substitution by using the trigonometric identity $$ sin(x) = \sqrt{1 - cos^2(x)} $$:

$$
\begin{aligned}
\eta sin \delta &= sin \gamma \\
\eta \sqrt{1 - cos^2 \delta} &= sin \gamma \\
\eta^2(1 - cos^2 \delta) &= sin^2 \gamma \\
\eta^2 - \eta^2 cos^2 \delta &= sin^2 \gamma \\
\eta^2 cos^2 \delta &= \eta^2 - sin^2 \gamma
\end{aligned}
$$

This intermediate result is also listed in the extended appendix. When we take the square root of this, we get:

\begin{equation}
\eta cos \delta = \sqrt{\eta^2 - sin^2 \gamma}
\end{equation}
 
Finally,     we substitute this term into equation \eqref{eq:eta_prime_correct} to obtain:

\begin{equation}
\eta'(\gamma) = \frac{\sqrt{\eta^2 - sin^2 \gamma}}{cos \gamma}
\end{equation}

# Angle for computing $$\eta'$$

One more thing that surprised me initially was the use of $$\theta_d$$ to compute $$\eta'$$. According to the derivation of the Bravais Index, the angle that we should use is the 
inclination towards the normal plane $$\theta$$. So my first instinct was to use $$\theta_i$$. And this is correct for a perfectly smooth cylinder. 
In this case, reflection only occurs when $$\theta_i = -\theta_r$$, as noted in section 4.1 in **[4]**. Because of this, the scattering function
contains a Dirac delta function before taking surface roughness into account.

When we introduce roughness, the Dirac delta is replaced by the longitudinal scattering term $$M$$ that accounts for the cuticle tilt and roughness.
Looking at this through the lens of microfacet theory, we can assume that our rough surface consists of small patches with varying normals.
But crucially, every patch still acts like a mirror-like  reflection.
This means that for a given $$\theta_i$$ and $$\theta_r$$, the underlying geometry normal is replaced by the exact normal that causes $$\theta_i$$ to be reflected into $$\theta_r$$.
The normal that causes this reflection has the inclination $$\theta_h$$.

This clears up why $$\eta'$$ is a function of $$\theta_d$$. 
Since the reflecting microfacet has the inclination $$\theta_h$$, which is centered between $$\theta_i$$ and $$\theta_r$$, the inclination between
either of them and the halfway vector is half of the inclination between the two, which is $$\theta_d$$.
This also ensures that the term is reciprocal.

# Internal Transmission Path Segment Length

As noted in **[7]**, the internal path segment length $$l$$ is incorrect in the original Marschner paper **[4]**.
There it is incorrectly stated:

\begin{equation}
l_{Marschner}(\gamma_t) = 2 + 2cos(2\gamma_t) \label{eq:path_length_marschner}
\end{equation}

We can derive the correct term using the law of cosines. The law of cosines relates the length of the triangle sides using the cosine of the angle:

\begin{equation}
c^2 = a^2 + b^2 - 2ab cos \gamma \label{eq:law_of_cosines}
\end{equation}

As seen in *Figure 9* in **[4]**, $$a=1$$ and $$b=1$$, since this is the radius of the unit circle. We require the angle opposite of $$l$$.
Since we know that the sum of all angles in a triangle is $$\pi$$, we know $$\gamma= \pi - 2 \gamma_t$$. Inserting these values into \eqref{eq:law_of_cosines} we get:

$$
\begin{aligned}
l^2 &= 1^2 + 1^2 - 2 cos(\pi - 2 \gamma_t) \\
l^2 &= 2 - 2 cos(\pi - 2 \gamma_t)
\end{aligned}
$$

Then we simplify this using $$cos(x) = cos(-x)$$ and $$cos(x) = -cos(x - \pi)$$:

$$
\begin{aligned}
l^2 &= 2 - 2 cos( 2 \gamma - \pi) \\
l^2 &= 2 + 2 cos(2 \gamma_t)
\end{aligned}
$$

Taking the square root, the final path segment length is:

\begin{equation}
l = \sqrt{2 + 2 cos(2 \gamma_t)} \label{eq:path_length_derived}
\end{equation}

Comparing this to equation \eqref{eq:path_length_marschner}, it is clear that the latter is missing the square root. In **[7]**, the path length is given as:

\begin{equation}
l_{Zinke} = 2 cos(\gamma_t) \label{eq:path_length_zinke}
\end{equation}

Note that my notation differs slightly from **[7]**. For one, I assume that the fiber radius is one, so I omitted $$r=1$$ . Furthermore, they relate the 
projected path length $$l'$$ to the actual path length $$l$$  by accounting for the inclination to the normal plane $$\theta_t$$. 
The original Marschner paper accounts for the impact of $$\theta_t$$ by using a modified extinction coefficient.
I follow the latter approach, which is why equation \eqref{eq:path_length_zinke} looks slightly different from **[7]**.

Plotting \eqref{eq:path_length_derived} and \eqref{eq:path_length_zinke}, it is obvious that they are equivalent in the range $$-\frac{\pi}{2} \leq \theta_t \leq \frac{\pi}{2}$$
or when taking their absolute value. We can show that they are equal. First, we pull out a factor of two from the square root twice.

$$
\begin{aligned}
l &= \sqrt{2 + 2 cos(2 \gamma_t)} \\
l &= \sqrt{2} \sqrt{1 + cos(2 \gamma_t)} \\
l &= \sqrt{2} \sqrt{2} \sqrt{\frac{1 + cos(2 \gamma_t)}{2}} \\
l &= 2 \sqrt{\frac{1 + cos(2 \gamma_t)}{2}}
\end{aligned}
$$

Then we use the trigonometric identity $$cos(\frac{x}{2}) = sign(cos\frac{x}{2})\sqrt{\frac{1+cos(x)}{2}}$$ to arrive at:

\begin{equation}
sign(cos(\gamma_t)) l = 2 cos(\gamma_t)
\end{equation}

Taking the absolute value of this equation gives:

\begin{equation}
|sign(cos(\gamma_t)) l| = |2 cos(\gamma_t)|
\end{equation}

From equation \eqref{eq:path_length_derived}, we know that $$l \geq 0$$ and $$l$$ is zero when $$cos(\gamma_t)$$ is zero, so we can drop the sign:

\begin{equation}
|l| = |2 cos(\gamma_t)|
\end{equation}

Since we know that $$l$$ is never negative, we can drop the absolute value on the left side of the equation as well:

\begin{equation}
l = |2 cos(\gamma_t)|
\end{equation}

This matches \eqref{eq:path_length_zinke}, except for the absolute value.
It is interesting to note that the equation used by Pixar **[8]** is almost identical to this term but is missing the factor of two.

# References

**[1]** Physically Based Hair Shading in Unreal, Karis 2016 \
**[2]** Strand-based Hair Rendering in Frostbite, Tafuri 2019 \
**[3]** An Energy-Conserving Hair Reflectance Model, d’Eon 2011 \
**[4]** Light Scattering from Human Hair Fibers, Marschner 2003 \
**[5]** Light Scattering from Human Hair Fibers, Marschner 2003, ACM Digital Library, Supplemental Material \
**[6]** Derivations for appendix of “Light Scattering from Hair Fibers”, Marschner 2003 \
**[7]** Light Scattering from Filaments, Zinke 2007 \
**[8]** A Data-Driven Light Scattering Model for Hair, Pekelis 2015

# Appendix

Here are some additional visualizations for the azimuthal light paths. The IoR is fixed at 1.55, the common value for hair.

<div style="text-align:center">
<img src = "/images/Marschner/azimuthalPathTT.svg" alt="Azimuthal scattering TT path"/>
<p>Azimuthal scattering TT path</p>
</div>

<div style="text-align:center">
<img src = "/images/Marschner/azimuthalPathTRT.svg" alt="Azimuthal scattering TRT path"/>
<p>Azimuthal scattering TRT path</p>
</div>

<div style="text-align:center">
<img src = "/images/Marschner/azimuthalPathTRTCaustic.svg" alt="Azimuthal scattering TRT path with caustic marked"/>
<p>Azimuthal scattering TRT path with the caustic marked in red</p>
</div>