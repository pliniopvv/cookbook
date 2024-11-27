# GLFW | Cpp
## Muda cor do fundo
[Tutorial](https://www.youtube.com/watch?v=IejsKqJG9dM)

Configura azul como fundo da aplicação

```CPP
glClearColor(0.0,0.15,0.35,0.1);
```

## ESC finaliza janela

```Cpp
if (glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS)
{
glfwSetWindowShouldClose(window, GLFW_TRUE);
}

glColor3f(1.0,0.0,0.0);
glBegin(GL_TRIANGLES);
    glVertex3f(0.0, 0.5,0.0);
    glVertex3f(-0.5, -0.5, 0.0);
    glVertex3f(0.5, -0.5, 0.0);
glEnd();
```

## Triângulo -> Quadrado
[Tutorial](https://www.youtube.com/watch?v=FwOw70_2UAc)

```Cpp
glMatrixMode(GL_PROJECTION);
glLoadIdentity();
glOrtho(-10.0, 10.0, -10.0, 10.0, 1.0, -1.0);

glBegin(GL_QUADS);
    glVertex3f(-2.5, -2.5, 0.0);
    glVertex3f(2.5, -2.5, 0.0);
    glVertex3f(2.5, 2.5, 0.0);
    glVertex3f(-2.5, 2.5, 0.0);
glEnd();
```
