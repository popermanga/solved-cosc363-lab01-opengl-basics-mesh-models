Download Link: https://assignmentchef.com/product/solved-cosc363-lab01-opengl-basics-mesh-models
<br>
<u>Aim:</u>

In this lab, you will familiarize yourself with the structure of basic OpenGL programs, and also implement camera motion in a simple scene consisting of GLUT objects.  You will also learn to create and display mesh models in OFF format.

<ol>

 <li><u> Teapot.cpp:</u></li>

 <li>The program <strong>cpp</strong> has a structure similar to that given in lecture notes ([1]:8-10). It draws a teapot (using the function glutSolidTeapot), and additionally a floor plane (using the function “drawFloor()”).  The teapot is drawn at the origin of the reference frame. The floor is drawn using a set of lines parallel to the <em>x</em> and <em>z</em> axes.  The program produces the output shown in</li>

</ol>

Fig. (a).

Fig. (b)




<ol start="2">

 <li><strong>Camera:</strong> The output you just obtained is the view of the teapot from a point along the <em>z</em>-axis, at a distance 12 units from the origin (see Fig. (b)).  The first three parameters (0, 0, 12) of the  gluLookAt()  function define this position of the camera. Try changing the camera position to (0, 10, 12), lifting the camera up by 10 units along the y-direction.  The display should change to that shown in Fig. (c).  Note that OpenGL has removed hidden segments on the floor and the teapot using the depth buffer.</li>

 <li><strong>Call-back functions:</strong> We will now implement a special keyboard callback function to move the camera up and down using the arrow keys. Define a global int variable cam_hgt  to represent the height of the camera and initialize it with the current value 10.  Modify the corresponding parameter in the function  gluLookAt()to use this variable instead of a constant value. Define a call-back function special()  as shown below  (also see slides [1]-42,43):</li>

</ol>

//–Special keyboard event callback function ——— void special(int key, int x, int y)

{     if(key == GLUT_KEY_UP) cam_hgt++;     else if(key == GLUT_KEY_DOWN) cam_hgt–;     glutPostRedisplay();

}

You will need to register the above function as a call-back, by adding the following statement in main()  (See slide [1]-43).

glutSpecialFunc(special);

Please note that the above statement should be added <em>before</em> the call to glutMainLoop(). Run your program to test the correctness of your implementation. Limit the movement of the camera to the range 2 – 20.

<ol start="4">

 <li>Our next task is to continuously move the camera along a circular path around the teapot, at the current height from the floor given by cam_hgt (Fig. (d)). The radius of the camera’s orbit is 12 units. Define a global float variable theta to represent the positional angle of the camera (in degs), and initialize it to 0.</li>

</ol>




Fig. (d)

Specify the position of the camera in gluLookAt()  as  (12sin,  cam_hgt,

12cos). [Warning:  cos(), sin() functions assume that the parameter is defined in radians, not degrees! So please convert theta to radians]. Define a timer callback function (also see slide [1]-51) to continuously increment theta at regular intervals of  50 msecs  as follows:




void myTimer(int value)

{

theta ++;    glutPostRedisplay();  glutTimerFunc(50, myTimer, 0); }




Also add the statement glutTimerFunc(50, myTimer, 0); inside main().  If implemented correctly, the camera should continuously hover around the teapot. The height can still be changed using arrow keys.

<ol start="5">

 <li>Replace the teapot with other GLUT objects. Please refer to the file “GLUTGLU-Objects.pdf” for a brief description of the built-in objects.</li>

 <li><u> Mesh Models:</u></li>

</ol>

The Object File Format (OFF) is a convenient ASCII format for storing 3D model definitions (meshes). It uses simple vertex-list and polygon-list structures for specifying a polygonal model.  Unlike other 3D mesh formats, the OFF format does not intersperse commands with values on every line, and therefore can be easily parsed to extract vertex coordinates and triangle indices.  The basic structure of a mesh file in OFF format is shown in Fig. (e).

Fig. (e)

The first line contains the header keyword OFF.  The second line contains the total number of vertices (<em>n<sub>v</sub></em>), total number of polygons (<em>n<sub>p</sub></em>) and edges (<em>n<sub>e</sub></em>) in the model. The number of edges is always set to 0.  The second line will be followed by <em>n<sub>v</sub></em> lines containing three-dimensional coordinates of the vertices. The file will then contain <em>n<sub>p</sub></em>  lines defining each polygon in terms of its vertex indices. The first number in a polygon definition indicates the number of vertices of that polygon. For a triangular mesh, this number will always be 3. The following three numbers give the vertex indices of that polygon.  We will consider a simple example below.




<table width="564">

 <tbody>

  <tr>

   <td width="243"></td>

   <td width="39"><em>X </em></td>

   <td width="282"> </td>

  </tr>

 </tbody>

</table>

Fig. (f)                                                               Fig. (g)

An octahedron contains 6 vertices and 8 triangles. The octahedron in Fig. (f) intersects each of the principal axes at a distance 0.5 units from the origin. Thus the first vertex (with index 0) has coordinates (0, 0, 0.5), the second vertex has index 1 and coordinates (0.5, 0, 0) and so on.  Now refer to Fig. (e), which gives the model definition for this shape.  The first triangle has vertex indices 0, 1, 4. Triangles can be defined in any order, but note that the<u> vertices must be oriented in an anti-clockwise</u> <u>sense when viewed from outside</u> the model. This will ensure that the surface normal vectors are directed outwards from each triangle, as required by illumination models.

<ol>

 <li>Create a plain text file with name “Octahedron.off” and create the model definition for the octahedron (by completing the missing entries in Fig. (e)).</li>

</ol>




<ol start="2">

 <li>The program <strong>cpp</strong> includes the function and variable declarations for reading data from an OFF file.   The vertex coordinates are stored in arrays x, y, z;  and the triangle indices in arrays  t1, t2, t3.  The variable nvert stores the total number of vertices, and nface the total number of polygonal faces (triangles or quads). The  display function contains a glBegin()..glEnd()  block to render the triangles.  If your model definition is correct, the output should look similar to that given in Fig. (g). The program also allows you to rotate the object about the x, y axes using the arrow keys.</li>

</ol>




<ol start="3">

 <li>Load the mesh file “Sphere.off” by specifying the file name on the second line of the initialize() function. The output of the program is shown in Fig. (h). One of the triangular faces of the sphere appears to be wrongly rendered/lit. This triangle is the third triangle in the face list of the OFF file, and has 4, 2, 5 as the vertex indices.  Please correct this entry in the face list to get a proper rendering of the sphere.</li>

</ol>

Fig. (h)

<ol start="4">

 <li>Three models “Cone.off”, “Cow.off”, “Oni.off” are provided in the file Models_OFF.zip.   Change the mesh file name in the program to get the displays of these models (Fig. (i)).</li>

 <li>You can get a wireframe display of the models by changing the polygon display mode from GL_FILL to GL_LINE in the function call glPolygonMode(…). Please disable lighting in the wireframe display mode, by changing glEnable(GL_LIGHTING) to glDisable(GL_LIGHTING).</li>

</ol>




<table width="520">

 <tbody>

  <tr>

   <td width="177"> </td>

   <td width="180"></td>

   <td width="164"> </td>

  </tr>

 </tbody>

</table>

Fig. (i)





