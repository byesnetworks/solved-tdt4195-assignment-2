Download Link: https://assignmentchef.com/product/solved-tdt4195-assignment-2
<br>
Understand how multiplying a 4×4 matrix with single coordinates allows for a wide variety and combinations of transformations. Get a taste of combining transformations, and what the limits are of their possibilities.

In the previous lab we’ve seen how to draw geometric primitives using OpenGL, and had a taste of GLSL Shaders. This lab focuses on affine transformations, and their effects on a scene.

This lab starts from the point where lab 1 ended. As such you should use the code from the previous as your starting point of this one.

<h1>Task 1: Repetition</h1>

<ol>

 <li>a) In Lab 1, you created a function in Task 1a which converted a float (f32) and integer (u32) vector into a Vertex Array Object (VAO).</li>

 <li>i) Extend this function to in addition take another float vector as a parameter. This additional vector should contain color values – one RGBA color value per vertex. This additional float vector should be put into a Vertex Buffer Object, and attached to the VAO.</li>

</ol>

<em>Hint: </em>This should only be a matter of copying and pasting sections of the original function and modifying some parameters. Bear in mind that the attribute pointers need to be specified while the buffer they’re referring to is bound.

<em>Hint: </em>Colors in OpenGL usually consist of 4 floats. Each float represents one channel in the order [red, green, blue, alpha]. The value of each channel should be within the range [0, 1]. In each channel, a value of 1 denotes the maximum value, while 0 represents the minimum value.

The alpha channel denotes the <em><sub>transparency </sub></em>of the color. In this case, a value of 1 represents an entirely opaque color, while a value of 0 means the color is completely transparent.

In Gloom-rs, transparency has been turned on by default. This is something that has to be explicitly enabled in an OpenGL project, and is done with the following lines:

gl::Enable(gl::BLEND);

gl::BlendFunc(gl::SRC_ALPHA, gl::ONE_MINUS_SRC_ALPHA);

The <sub>gl::Enable() </sub>function enables a particular OpenGL feature. In this case, the parameter <sub>gl::BLEND </sub>specifies that you’d like to turn on alpha blending. The gl::BlendFunc() specifies how a transparent color should be mixed with colors from objects behind it. The values shown above are usually the ones you’ll want to use.

Blending in OpenGL is done by looking at the color currently present in the framebuffer, as well as its transparency, and combining it with the new color and its transparency as defined by the blending function specified in <sub>gl::BlendFunc()</sub>.

In this case, a new color is calculated like this:

ColorNew = ColorSource <em>· </em>AlphaSource + ColorDestination <em>· </em>(1 <em>− </em>AlphaSource) (1) The source color and alpha are the color and transparency of the new pixel being drawn, respectively. The destination color is the color of the pixel already present at the same location in the framebuffer.

Unless you want to achieve some very specific effects, the <sub>gl::BlendFunc() </sub>parameters shown above are almost always the ones you’ll want to use.

<ol>

 <li>ii) Modify the Vertex (“gloom-rs/shaders/simple.vert”) and Fragment (“gloomrs/shaders/simple.frag”) shaders to use colors from the new color buffer as the colors of triangle(s) in the buffer. If vertices in the same triangle have been assigned different colors, colors should be interpolated between these vertices.</li>

</ol>

<em>Hint: </em>both shaders require modification to accomplish this.

<ol>

 <li>b) <strong><sub>[report] </sub></strong>Render a scene containing at least 3 different triangles, where each vertex of each triangle has a different color. Put a screenshot of the result in your report. All triangles should be visible on the screenshot.</li>

</ol>

<h1>Task 2: Alpha Blending and Depth</h1>

<ol>

 <li><strong><sub>[report] </sub></strong>For this task, we want to show you what happens to a blended color when multiple triangles overlap from the camera’s perspective (where the triangles are positioned at different distances from the camera).</li>

</ol>

To this end, draw at least 3 triangles which satisfy the following conditions:

<ul>

 <li>There exists a section on the xy-plane on which all triangles overlap.</li>

 <li>For any single triangle, the three vertices of that triangle have the same zcoordinate.</li>

 <li>No two triangles share the same z-coordinate.</li>

 <li>All triangles have a transparent color (alpha &lt; 1).</li>

 <li>All triangles have a different color.</li>

 <li>Each triangle’s vertices have the same color.</li>

</ul>

I do not recommend drawing the exact same triangle 3 times at different depths; it will make the next question more difficult to solve.

Remember to turn on alpha blending, as described in the previous task.

Put a screenshot in your report.

<ol>

 <li><strong><sub>[report] </sub></strong>First make sure your triangles are being drawn back to front. That is, the triangle furthest away from the screen is drawn first, the second-furthest one next, and so on. You can change the draw order of triangles by modifying the index buffer. Remember the coordinate space of the Clipbox here.

  <ol>

   <li>Swap the colors of different triangles by modifying the VBO containing the color Vertex Attribute. Observe the effect of these changes on the blended color of the area in which all triangles overlap. What effects on the blended color did you observe, and how did exchanging triangle colors cause these changes to occur?</li>

   <li>Swap the depth (z-coordinate) at which different triangles are drawn by modifying the VBO containing the triangle vertices. Again observe the effect of these changes on the blended color of the overlapping area. Which changes in the blended color did you observe, and how did the exchanging of z-coordinates cause these changes to occur? Why was the depth buffer the cause this effect?</li>

  </ol></li>

</ol>

<h1>Task 3: The Affine Transformation Matrix</h1>

<ol>

 <li>Modify your Vertex Shader so that each vertex is multiplied by a 4×4 matrix. The elements of this matrix should also be defined in the Vertex Shader.</li>

</ol>

Change the individual elements of the matrix so that it becomes an identity matrix. Render the scene from Task 1b to ensure it has not changed.

Note that matrices in GLSL are column major. As such, individual elements in a matrix can be addressed using: matrixVariable[column][row] = value;

It is also possible to assign a vec4 to a column, to set all 4 values at once: matrixVariable[column] = <strong>vec4</strong>(a, b, c, d); Refer to the guide for more information.

<ol>

 <li><strong><sub>[report] </sub></strong>Individually modify each of the values marked with letters in the matrix in equation 2 below, one at a time. In each case use the identity matrix as a starting point.</li>

</ol>

Observe the effect of modifying the value on the resulting rendered image. Deduce which of the four distinct transformation types discussed in the lectures and the book modifying the value corresponds to. Also write down the direction (axis) the transformation applies to.

<table width="311">

 <tbody>

  <tr>

   <td width="30"><em>a</em><em>d </em> 00</td>

   <td width="21"><em>b e</em>00</td>

   <td width="21">00 10</td>

   <td width="221"><em>c</em> <em>f</em>1</td>

   <td width="19">(2)</td>

  </tr>

 </tbody>

</table>

<em>Hint: </em>You can use a uniform variable to pass a floating point value slowly oscillating between -0.5 and 0.5 from the main loop in main.rs into the Vertex Shader. Use the elapsed variable in the main loop, and compute the sine of of it (<sub>elapsed.sin()</sub>) before sending it into the shader with a uniform variable. See the guide for how to do this.

It makes the effects of modifying individual values in the transformation matrix much easier to recognise. The guide includes a description on how to work with uniform variables.

<ol>

 <li><strong><sub>[report] </sub></strong>Why can you be certain that none of the transformations observed were rotations?</li>

</ol>

<strong>Optional Recommended Intermediate:</strong>

<h1>Play around with the i3t-tool</h1>

The task after this one requires you to implement a camera with multiple axis of freedom. Games usually implement this as a combination of transformations.

You’ve seen in the previous task how a single matrix can cause a number of deformations to occur to a shape as a direct result of the behaviour of matrix multiplication. But that’s not what makes these matrices so amazing.

The main thing that causes affine transformations to be used ubiquitously in games nowadays is because of the following equation:

(<em>A · B</em>)<em>v </em>= (<em>A · </em>(<em>Bv</em>))                                                             (3)

Here v is a given vertex inside your model, while A and B are transformation matrices. While the difference may appear insignificant, the important thing here is that A and B can be precomputed. A and B can even be a much longer sequence of arbitrary transformations if desirable. It is not necessary to multiply each matrix with each vertex individually; something which would take an unholy amount of time even on good hardware.

But there’s also a conceptual side to this. If I have a long sequence of matrices, each representing some sort of transformation:

<em>v<sup>0 </sup></em>= (<em>A · B · C · D · E · F · G · H</em>)<em>v                                                   </em>(4)

The <em><sub>order </sub></em>in which they are multiplied <em><sub>matters</sub></em>.

This is due to the rules of matrix multiplication not being commutative. In order to gain an understanding of how the order of multiplication affects the resulting transformation, I

<em>highly </em>recommend checking out a tool called “I3T”, which you can download from here: <a href="http://i3t-tool.org/">http://i3t-tool.org/</a>

Unfortunately, since this is a Windows application, we can’t install them on the lab machines. In Figure 1 you can see a screenshot of what the program looks like when you start it initially.

The area marked with A is where you can view the scene you’ve constructed. B is the scene’s origin (the x, y, and z-axis are marked with a red, green, and blue color, respectively).

The scene initially contains a single cube object (C). As you can see, it has its own coordinate system, which has undergone some transformations. These transformations are

Figure 1: The main window of the i3t tool.

shown as yellowish boxes (F) in the scene editing area (D).

There are three transformation matrices present in the area which defines the “sequence” of transformations applied to the cube present in the scene (E). Note that the transformation type is listed in the header of each matrix.

You can add a new transformation by right clicking somewhere in the vicinity of D, click

“transformation”, and select the type you’d like to add. Next, you can drag the new matrix that appears into the existing sequence. You can also reorder them this way.

To modify matrices, notice that some fields are highlighted with green. You can drag these to incrementally modify the transformation (such as the angle in case of rotations). For a given sequence of transformations, the matrices applied on the object are multiplied together in the order in which they are shown.

<h1>Interlude: Transformation functions in glm</h1>

glm (provided by <sub>nalgebra_glm</sub>) is a collection of mathematical functions and types that behave much like the those found in <sub>glsl</sub>. Here are the (simplified rust signatures of) <sub>glm </sub>functions you’ll want to use to produce various transformation matrices, with examples of how you’d use them:

<strong>Rotation </strong><strong>fn </strong><strong>glm</strong>::rotation(angle: <strong>f32</strong>, axis: <strong>&amp;</strong><strong>glm</strong>::Vec3) -&gt; <strong>glm</strong>::Mat4; Example usage: <strong>let </strong>rotation: <strong>glm</strong>::Mat4 = glm::rotation(angle, &amp;glm::vec3(<strong>1.0</strong>, <strong>0.0</strong>, <strong>0.0</strong>)); axis should be a normalized vector representing the vector around which to do the rotation. The angle is in radians.

<strong>Translation </strong><strong>fn </strong><strong>glm</strong>::translation(direction: <strong>&amp;</strong><strong>glm</strong>::Vec3) -&gt; <strong>glm</strong>::Mat4; Example usage: <strong>let </strong>translation: <strong>glm</strong>::Mat4 = glm::translation(&amp;glm::vec3(<strong>0.0</strong>, <strong>1.0</strong>, <strong>0.0</strong>)); Generates a 4×4 transformation matrix representing a translation by [<em><sub>x,y,z</sub></em>] units.

<strong>Scaling </strong><strong>fn </strong><strong>glm</strong>::scaling(scale: <strong>&amp;</strong><strong>glm</strong>::Vec3) -&gt; <strong>glm</strong>::Mat4; Example usage: <strong>let </strong>scaling: <strong>glm</strong>::Mat4 = glm::scaling(&amp;glm::vec3(<strong>1.0</strong>, <strong>1.0</strong>, <strong>1.0</strong>));

Generates a 4×4 transformation matrix representing a scale transformation by a factor of [<em><sub>x,y,z</sub></em>]. Remember that scaling by a factor of 1 leaves the scene intact.

<h2>Identity matrix</h2>

Generate a 4×4 identity matrix:

<strong>let </strong>identity: <strong>glm</strong>::Mat4 = glm::identity();

<h1>Task 4: Combinations of Transformations</h1>

<strong>Please do not be put off by the size of this question. </strong>The main reason for there being so much text is to ensure the objective of the question is clear, and to explain some concepts surrounding it. For reference, when I implemented this question, I needed around 30 lines of code. Most of which was a matter of copying and pasting.

We’ve now seen some basic affine transformations. However, we have not yet looked at what makes them so useful and powerful: their ability to combine. To demonstrate this, consider a point <em><sub>p</sub></em>, and two transformations which we wish to apply on this point; <em><sub>A </sub></em>and <em><sub>B</sub></em>. Here <em>p </em>is a 4×1 matrix, and <em><sub>A </sub></em>and <em><sub>B </sub></em>are both 4×4 matrices. We can transform <em><sub>p </sub></em>by <em><sub>A</sub></em>, simply by performing matrix multiplication on them: <em><sub>A · p</sub></em>. Since the result of this multiplication is again a 4×1 matrix, we can multiply the result with <em><sub>B </sub></em>to get a transformed point <em><sub>p</sub><sup>0</sup></em>: <em>p<sup>0 </sup></em>= <em>B · </em>(<em>A · p</em>).

The key point here is that matrix multiplication is <em><sub>associative</sub></em>. That is, we don’t necessarily need to multiply matrices in the order shown above. Instead, we can multiply <em><sub>A </sub></em>and <em><sub>B </sub></em>first, which yields another 4×4 matrix, and multiply this result with point <em><sub>p</sub></em>. This 4×4 matrix represents the combined result of the transformations <em><sub>A </sub></em>and <em><sub>B</sub></em>. Multiplying this matrix with <em><sub>p </sub></em>will yield the same point as before <em><sub>p</sub><sup>0</sup></em>. In other words: <em><sub>B </sub>·</em>(<em><sub>A</sub>·</em><em><sub>p</sub></em>) = (<em><sub>B </sub>·</em><em><sub>A</sub></em>)<em>·</em><em><sub>p </sub></em>= <em><sub>C </sub>·</em><em><sub>p</sub></em>

We can thus multiply any number of transformation matrices together and still always be left with a single 4×4 matrix which represents the entire sequence of transformations.

Why is this such an important property? It means that we can apply any number of affine transformations on a point by multiplying it with a single 4×4 matrix, rather than having to compute the result of each individual transformation separately for every single vertex. This saves a massive amount of computation in the vertex shader for each frame.

We will now implement the next major component of our animated scene: a controllable camera. We will be combining multiple affine transformations to accomplish this.

<ol>

 <li>a) Alter your Vertex Shader so that the transformation matrix by which input coordinates are multiplied is passed in as a uniform variable. The guide contains information on how to accomplish this. An important thing to note is that when passing a matrix from glm to the OpenGL uniform functions, you want to use .as_ptr() on the matrix.</li>

</ol>

Alter the main loop to set the value of the created uniform variable each frame, prior to rendering the scene.

For the remainder of this task, do all creation and multiplication of matrices on the CPU (i.e. in Rust, not in the vertex shader), and pass on the resulting combined 4×4 matrix through the uniform variable you just created.

<em>Hint: </em>Shader Programs must be activated (used) before you can change the values of any of their uniform variables.

Note: If your OpenGL version is below 4.3, you will <em><sub>not </sub></em>be able to use the layout qualifier to explicitly set the index of your uniform variables, and thus you will have to query the shader for the positions of uniforms. See the newest version of gloom-rs to find a helper method for getting this location.

<ol start="4">

 <li>b) The first transformation we’ll apply is a projection. Specifically, we’ll apply a perspective projection so that our scene looks more comparable to a “real world” scene. Fortunately, the GLM library (<sub>nalgebra</sub><sub>–</sub><sub>glm</sub>) which has been included with Gloom-rs contains a helpful function for generating such a transformation matrix: Any time you use any of the functions from the glm library, you should be specifying the type for the resulting variable as <sub>glm::Mat4</sub>. This is a 4×4 matrix consisting of <strong>f32</strong>, which will help you avoid any nasty surprises down the line if the type system concludes you want a double (<strong><sub>f64</sub></strong>) but you tell OpenGL you’re using floats (<strong><sub>f32</sub></strong>). This is necessary because the <sub>glm </sub>matrix functions are implemented as generic over matrix dimensions and datatypes, and OpenGL doesn’t help Rust do its type inference.</li>

</ol>

<strong>let </strong>variable_name: <strong>glm</strong>::Mat4 = glm::perspective( aspect : <strong>f32</strong>, fovy       : <strong>f32</strong>, near           : <strong>f32</strong>, far          : <strong>f32</strong>.

);

The first parameter (<sub>aspect</sub>) specifies the aspect ratio of the window, defined as the window height divided by the window width. The second parameter (<sub>fovy</sub>) specifies the vertical Field Of View (FOV) the <em><sub>camera </sub></em>is capable of capturing. That is, the angle between the top and the bottom plane of the view frustum.

Finally, the <sub>near </sub>and <sub>far </sub>parameters specify how far the <em><sub>near </sub></em>and <em><sub>far </sub></em>clipping planes of the view frustum should be located away from the origin. It is recommended you set these to <strong><sub>1.0 </sub></strong>and <strong><sub>100.0</sub></strong>, respectively.

If you do, any triangles whose coordinates are near the <em><sub>x </sub></em>= 0 and <em><sub>y </sub></em>= 0 mark (which your triangles from task 1 already are) will be visible between z-coordinates ranging between -1 and -100. Why, you might ask? You see, the projection matrix returned by glm::perspective() <em>flips the z-axis</em>.

For this reason, the coordinate system of the Clipbox, which is a left-handed one, is converted into a right-handed one. There’s not much logic to this other than that it’s an OpenGL convention. This means that the order of which triangles are closest to the screen will get reversed.

In order to both draw your triangles with a perspective projection, ensuring they don’t go out of view, do the following:

Apply a translation transformation on the matrix you send into the vertex shader. Translate the triangles into the negative z direction. Note that the visible coordinate range of triangles lies between -1 and -100, assuming you went with the near and far plane distances we suggested. At this point, your triangles should become invisible.

Now apply the perspective projection on the transformation matrix sent into the vertex shader. Make sure it’s the last transformation applied on the triangles. You can now compile and run your program, and your triangles should be visible again.

<h2>Creating the Camera</h2>

Before we continue, let us first consider some important aspects to understand for creating a camera. The most important of which is that a projection effectively only changes the shape and dimensions of the Clipbox. In a way, the projection causes the contents of an arbitrary frustum volume to be morphed into the shape of the Clipbox.

As such any geometry located inside of the view frustum defined by the projection is inserted into the rendering pipeline and processed. Anything outside of it is clipped away.

For instance, an Orthographic projection will linearly scale coordinates along each axis to fall into the range [-1, 1]. The perspective projection is a bit more complicated, and is shown in Figure 3.

There are two important observations to make here. First, projections define theirown origin, view frustum, and scale. Knowing where your origin is located is important because affine transformations are applied relative to this origin. For instance, a rotation transformation causes an object to be rotated around the origin in a particular direction.

Scale is also relevant, because it determines the size the objects you are rendering need to be. For instance, using an Orthographic projection to create a frustum with a height of 100 units will cause a model with a height of 1000 units to almost be clipped away in its entirety, because it does not fit in the view frustum.

The second and more relevant observation revolves around the notion that the view frustums defined by projections can not moved around. For instance, the perspective projection is dependent on being “located” at the origin due to its mathematical definition.

Since the purpose of a camera is to be able to move through a scene and view it from different angles and positions, but the view frustum of the camera cannot be moved around, we instead have to <em>move the entire world around the camera</em>. In other words, our “camera matrix” needs to translate the world in such a way that the camera’s position becomes the origin, and rotate it such that the world’s x, y and z axes becomes aligned with the camera’s orientation. The end result is that anything that is directly in front of the camera will end up laying along the z-axis. This has been illustrated in Figure 2.

<table width="573">

 <tbody>

  <tr>

   <td width="314">(a) The perspective projection view frustum containing a scene of cubes.</td>

   <td width="259">(b) The same frustum morphed into the shape and size of the Clipbox, causing the shape of the scene it contains to morph with it.</td>

  </tr>

 </tbody>

</table>

Figure 3: The process of projecting a scene with a perspective projection, causing the view frustum to be morphed into the shape and size of the Clipbox.<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a>

<table width="574">

 <tbody>

  <tr>

   <td width="314">(a) A perspective projection frustum seen from above, and a cube moving through it towards the camera.</td>

   <td width="259">(b) From the perspective of the camera, the box has either moved towards the camera, or the camera has moved towards the box.</td>

  </tr>

 </tbody>

</table>

Figure 2: Moving a camera forward through a scene

Figure 4: A camera on a tripod, with indications showing which motions the camera implemented in this task should support. The origins of the rotational and translational motions have been separated for clarity. <a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a>

<ol start="4">

 <li>c) The objective of this task is to implement a camera which behaves like one standing on a tripod, as shown in Figure 4. It should be possible to move the tripod along each major axis, as well as being able to rotate it horizontally and vertically. Each of these motions has been illustrated in greater detail in Figure 5. For each of these motions, do the following:</li>

</ol>

<ul>

 <li>Create a variable which can store the value of the motion. That is, the current rotation of the camera along a certain axis, or its x, y, or z coordinates (one float or double per (rotation) axis).</li>

 <li>Add a key press handler for controlling the motion. You will need two keys every time; one to control the forward direction of the motion, and one for the reverse.

  <ol>

   <li>The key press handler should modify the variable corresponding to the rotation/translation axis you created previously.</li>

   <li>Gloom-rs by default already contains a sample key press handler you can use (it increments and decrements an arbitrary number). iii. Keys are identified with keycodes, the full list can be found here <a href="https://docs.rs/glutin/0.24.1/glutin/event/enum.VirtualKeyCode.html"><sub>https:</sub></a></li>

  </ol></li>

</ul>

<a href="https://docs.rs/glutin/0.24.1/glutin/event/enum.VirtualKeyCode.html">//docs.rs/glutin/0.24.1/glutin/event/enum.VirtualKeyCode.html</a>

<ol>

 <li>Creating a key handler is nothing more than copying and pasting the existing one in main.rs, changing the key code, and modifying the body of the if statement to change what happens when the key is pressed.</li>

 <li>Be aware that key states are checked 60 times per second (once per frame). Make sure to increment or decrement the value of your variables by something multiplied by <sub>delta_time </sub>to get movement independent of framerate..</li>

 <li>You are free to define yourself which keys are bound to which camera motion. Please list the keybinds you use in the report. I suggest using WASDEQ for movement and the arrow keys for rotation.</li>

</ol>

<em>Note: </em>by default Q closes the window. To use it for camera movement, remove the “Q =&gt; {*control_flow = ControlFlow::Exit}” case near the bottom of main.rs.

<ul>

 <li>Every frame, generate a transformation matrix which applies the effect of the motion on your scene.

  <ol>

   <li>Make sure you start with a “fresh” identity matrix each frame.</li>

   <li>See Figure 5 for a visual demonstrations what each of the different camera movements entail.</li>

  </ol></li>

</ul>

<ul>

 <li>Each camera motion only requires a single “basic” affine transformation (translation, rotation, scaling, etc).</li>

</ul>

<ul>

 <li>Apply each individual transformation on the transformation matrix which is later sent into the Vertex Shader.

  <ol>

   <li>Each movement results in one transformation matrix. Multiply these 5 matrices and the projection matrix together <em><sub>in the correct order </sub></em>to mimic the behaviour of a camera mounted on a tripod.</li>

  </ol></li>

</ul>

Some tips:

<ul>

 <li>Start with the identity matrix each frame. Build up the entire transformation from scratch again.</li>

 <li>In GLM, it is possible to do matrix multiplication by simply multiplying two variables containing matrices (e.g. <sub>matrix1 </sub><sub>* </sub><sub>matrix2</sub>). Don’t forget that matrix multiplication is <em><sub>not </sub></em></li>

 <li>Understand which origin you are rotating around and what can be seen by the camera (again, the i3t tool can help a lot here making sense of what is going on!).</li>

 <li>Try moving your camera into various situations to make sure you have something that <em><sub>is </sub></em>correct, and doesn’t just look correct when looking straight ahead.</li>

 <li>Try to keep in mind exactly what kinds of motions you need to do in order to move the world in such a way that it appears as though the camera is looking straight ahead from the origin. There are quite a few solutions that seem to be correct at first glance that most certainly are not!</li>

</ul>

Reference Position                       Yaw Rotation            Forward/Backward            Sideways

<ul>

 <li>A part of the camera motions you should implement in this task, as seen from above. A scene (agrey box in this case) is shown to indicate how the scene geometry moves relative to the camera’s coordinate system. Try to see which specific transformations you need to accomplish the various movements, given the indicated camera’s coordinate system.</li>

</ul>

Reference Position                             Pitch Rotation                  Upward/Downward

Object in Scene

View Frustrum

Camera

<ul>

 <li>The remaining camera motions you should implement, in this case seen from the side.</li>

</ul>

Figure 5: A visual demonstration of the camera movements you should implement in this task. In each case, there is a “base” state shown on the left hand side. To the right of it are shown different camera movements where the camera has been displaced along the direction of the motion. You should in particular look closely at how the scene (grey box) is transformed relative to the camera’s coordinate system, and use this to deduce the affine transformation needed to accomplish the particular movement.

<h1>Task 5: Optional Bonus Challenges</h1>

<em>This task is optional. </em>These questions are meant as further challenges or to highlight things you may find interesting. Their intent is to be more difficult than the tasks in this assignment, however, for those of you interested in learning more they also represent an opportunity to ensure your assignment grade is as good as possible.

As was the case with the previous assignment, getting full score on all tasks, including this one, does not give you a score that’s higher than the maximum score of 5%. Similarly, doing this task is not necessary to obtain a full score on this assignment. Finally, completing all bonus challenges will at most give you 0.3 bonus points (although we’d be extremely impressed!).

<ol>

 <li>The camera you implemented in the previous task works well in many situations, however, it works a bit counterintuitively when the camera moves strictly along major axes without taking into account which direction it’s facing.</li>

</ol>

Change the camera control scheme such that the “forward”, “backward”, “left”,

“right”, “up”, and “down” motions move relative to the direction the camera is facing, rather than having all of them move you in the direction of each major axis regardless of camera orientation.

Tip: You may want to limit range of motion of the vertical rotation of the camera (pitch) to between “straight down” and “straight up” to avoid weird camera movement. Also, there’s a really neat trick here if you properly understand your transformation matrices that allows you to turn movement relative to the camera into movement relative to the world, which solves this question in very few lines of code.

<ol>

 <li><strong><sub>[report] </sub></strong>When passing values from the vertex shader to the fragment shader, there is a slight problem: a triangle described by 3 vertices can cover any number of pixels on the screen. As such there is no one-on-one correspondence between the outputs of a vertex shader, and inputs handed to all the instances of the fragment shader for each pixel being drawn on screen.</li>

</ol>

To solve this, output values of the vertex shader are interpolated depending on the location of the fragment and the vertices of the triangle (or primitive). You saw the result of this in task 1 of this assignment.

The way these values are interpolated (if at all), can be changed by adding special qualifiers to Fragment Shader inputs. You simply add them as a special attribute to your input variable definitions. For instance: <strong>in layout</strong>(location=<strong>0</strong>) <strong>flat </strong><strong>vec4 </strong>vertexColour; GLSL supports the following interpolation qualifiers:

<h2>flat</h2>

No interpolation is performed. Instead one vertex in the triangle is designated to be the “Provoking” vertex, from which all values are copied directly. <strong>noperspective</strong>

Regular interpolation takes perspective into account in order to correctly interpolate values. Enabling this interpolation type instead causes values to be interpolated in window/screen space, giving some visually oddities. <strong>smooth</strong>

If no interpolation qualifier is present, this is the default interpolation mode. Performs “perspective corrected” interpolation.

Create a scene (or simply a camera position) that looks different with noperspective compared to smooth, and attach screenshots of the two versions.

Tip: You might want to change the fragment shader in such a way that you get a sharp border of color within a triangle in order to more easily see the effect of the qualifier.

<ol>

 <li>Special effects such as smoke or fire are commonly implemented as socalled “particle systems”. For large productions, particle systems are often highly advanced and configurable. However, in their simplest form they mostly come down to a lot of small geometry (a few triangles or even single points) being animated along some common motion.</li>

</ol>

One method for rendering particles is the so-called “billboard”. Billboards are usually rectangles (two triangles), which are transformed in such a way that they always face the camera.

One easy way of accomplishing this is to set all components related to rotation in the transformation matrix to 0 (thus orienting the drawing geometry towards the camera), then drawing your billboard with it.

<ol>

 <li><strong><sub>[report] </sub></strong>Affine transformations can accomplish many different transformations. However, when applying the same transformation on something like a 3D model, there are limits too.</li>

</ol>

The figures in this question show pairs of geometric structures. In each pair, one or more objects are shown, as well as the same object(s) having undergone some transformation.

Coordinates are shown where relevant to indicate scale. Determine in each case whether all vertices of the input object(s) can be transformed by a single 4×4 transformation matrix to produce the shown transformed object.

All coordinates of the shown shapes lie on the xy-plane (all z-coordinates are 0).

Additional mindbender: do the above task again, but you’re now allowed to place all vertices at arbitrary z-coordinates. Which of the shown transformations are now possible?

<a href="#_ftnref1" name="_ftn1">[1]</a> Image credit: opengl-tutorial.org

<a href="#_ftnref2" name="_ftn2">[2]</a> Image credit: https://www.amazon.ca/Zx600b300-Carbon-Fiber-Tripod-Cameras/dp/B007HO50NO