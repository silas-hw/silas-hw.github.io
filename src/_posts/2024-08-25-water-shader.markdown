---
layout: post
title:  "Water Shaders"
date:   2024-08-25
categories: devlog
tags: shaders glsl libgdx
---

## Water what now?

It's probably one of the last things you should make when programming a game. In fact, you should only even really *consider* juice once your core mechanics are in place. Did I consider any of that? Well... uhm.... not really.

I had an idea for how to create cool (ish) looking water reflections in a 2d game and wanted to program it, so I used the game I'm already making and whacked some stuff on top of it.

Before we get into anything, here's how it looks:

![Water Shader Gif](/assets/img/scngame/watershader1.gif)

As you can see, my little guy is reflected has a wavy effect applied over them, giving a great illusion of water reflection!

## How does it work?

It's quit simple really. In layman's terms, we:

    1. Draw anything we want reflected upside down relative to the original thing
    2. Apply a wavy effect to the upside down render
    3. draw the tilemap on top, with translucent parts where we want the reflections showing (i.e. the water)
    4. draw all the other entities normally

At first, I thought I could do the reflecting part with a glsl shader and just call the normal render loop twice (once with the shader and once without). Flipping the coordinate space of *everything* is quite easy, but flipping things relative to their original position isn't so easy. Not to mention, most of my assets exist in texture atlases, so flipping the coordinates would cause some wacky side effects.

As per usual, the 'dumb' way ended up being the correct way. I simply added a new draw method to all of my entities explicitly for drawing the reflection, so my code now looks like this (sort of):

{% highlight java %}
// more verbose than my actual code and missing managing viewports and the sprite batch

frameBuffer.start();

scene.drawEntitiesReflectedInWater();

frameBuffer.end();

// set the time uniform in the shader for the wavy effect
waterShader.bind()
waterShader.setUniformf("time", stateTime);

batch.setShader(waterShader);
batch.draw(frameBuffer, ...) // other values to make the frame buffer draw correctly

// draw everything else normally
batch.setShader(null);

tileMap.draw();
scene.drawEntities();
{% endhighlight %}

As you can see, we draw the reflections to a frame buffer and *then* apply the water wavy effect to the frame buffer. This is because fragment shaders (at least in LibGDX) work with a sample texture (the texture being drawn). If we used the shader when drawing individual textures, it would only offset what pixels from the texture are being sampled when the texture is being drawn (so the drawn texture would still appear the same size, but the sampled pixels sort of wonky). If we do the same with a frame buffer, however, it offsets what pixels are being drawn for the entire scene! This makes our individual textures actually look wavy!

The nifty bit comes with the GLSL shader. Right now it's quite simple, but I do plan on eventually making it more complicated to make the effect nicer. Here's the code:

{% highlight glsl %}
    #ifdef GL_ES
    precision highp float;
    #endif

    in vec4 gl_FragCoord;

    uniform sampler2D u_texture;

    uniform float u_time;

    // values to mess with!
    uniform float u_amplitude = 0.002;
    uniform float u_frequency = 400;
    uniform float u_timeCoefficient = 10;

    varying vec2 v_texCoords;

    void main()
    {
        vec2 texSize = textureSize(u_texture, 0);

        // offset the rows of the texture according to sine
        float offsetX = u_amplitude * sin(v_texCoords.y * u_frequency + u_time*u_timeCoefficient);

        vec2 texCoords = v_texCoords + vec2(offsetX, 0);

        gl_FragColor = texture2D(u_texture, texCoords);
    }
{% endhighlight %}

And thats it! That's everything I had to do to get these cool water reflections! Quite nice, isn't it?
