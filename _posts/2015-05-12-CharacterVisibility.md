---
title: Super Mario 3D Land-style Character Visibility
permalink: /CharacterVisibility/
---
I needed to solve a problem in my game where the character is obstructed when moving behind objects.

Initially my first implementation was to make any object in front of my character turn transparent. I would make a ray from the camera to the user controlled character. Then I would intersect my world geometry with the ray to determine which objects should turn translucent. I got it working at first and it look alright but there are a few draw backs that I didn’t like.

- Object granularity was difficult to control. I had to split my level geometry into small chucks if I didn’t want large pieces of objects turning translucent.
- Ray casting can be expensive if there are lots of objects in the scene.
- Translucent objects aren’t very pretty as you tend to see polygon shapes.
- Alpha blending can be expensive on some hardware and requires sorting of objects to render correctly.

I then remembered a technique I noticed when playing Super Mario 3D Land on the Nintendo 3DS.

{% include figure image_path="/assets/images/mario_hidden01.jpg" alt="Visible Character Image" caption="Visible Character" %}
{% include figure image_path="/assets/images/mario_hidden02.jpg" alt="Partially hidden character Image" caption="Partially hidden character" %}

Instead of making the environment translucent, you can see here that the character’s silhouette is darken and visible through all geometry.

This looks kinda neat so I decided to try this technique out. I used the depth buffer to determine visibility, the stencil buffer to mask out the character, and finally a final color pass to draw the character in either dark or full color depending on the stencil test.

The benefits of this technique over the previous one includes the following:

- No ray casting needed, therefore this technique is independent of the amount of world geometry.
- No ugly translucent geometry.
- No need to alpha blend anything.

The drawbacks are:

- You need to draw your character twice. Although the obstructed character should have a simpler inexpensive shader (i.e. all black vs. fully shaded).
- Requires writing and testing against the stencil buffer.

So the technique is the following:

- Clear stencil buffer to all zeros.
- Clear depth buffer.
- Draw your world geometry.
- Enable stencil write.
- Set front face depth failed operation to output zero.
- Set front face depth passed operation to increment.
- Set front face stencil function compare to always.
- Draw your character.
- Set front face depth failed operation to keep.
- Set front face depth passed operation to keep.
- Set front face stencil function compare to equal to 0.
- Draw your character with a color you want it to appear when hidden.

{% include figure image_path="/assets/images/game_hidden01.jpg" alt="Final result: Visible Image" caption="Final result: Visible" %}
{% include figure image_path="/assets/images/game_hidden02.jpg" alt="Final result: Hidden Image" caption="Final result: Hidden" %}
