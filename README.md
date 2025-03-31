#include <GL/glut.h>
#include <cmath>

void drawCircle(float cx, float cy, float r, int num_segments) {
    glBegin(GL_TRIANGLE_FAN);
    glVertex2f(cx, cy);
    for (int i = 0; i <= num_segments; i++) {
        float theta = 2.0f * 3.1415926f * float(i) / float(num_segments);
        float x = r * cosf(theta);
        float y = r * sinf(theta);
        glVertex2f(cx + x, cy + y);
    }
    glEnd();
}

void display() {
    glClear(GL_COLOR_BUFFER_BIT);
    
    // Dibujar el tallo
    glColor3f(0.0f, 0.8f, 0.0f);  // Verde
    glBegin(GL_POLYGON);
        glVertex2f(-0.05f, -0.8f);
        glVertex2f(0.05f, -0.8f);
        glVertex2f(0.05f, 0.0f);
        glVertex2f(-0.05f, 0.0f);
    glEnd();

    // Dibujar los pétalos
    glColor3f(1.0f, 0.5f, 0.0f);  // Naranja
    for (int i = 0; i < 5; i++) {
        float angle = 2.0f * 3.1415926f * float(i) / 5;
        float x = 0.2f * cosf(angle);
        float y = 0.2f * sinf(angle) + 0.2f;
        drawCircle(x, y, 0.15f, 100);
    }

    // Dibujar el centro de la flor
    glColor3f(1.0f, 1.0f, 0.0f);  // Amarillo
    drawCircle(0.0f, 0.2f, 0.1f, 100);

    glFlush();
}

void init() {
    glClearColor(1.0f, 1.0f, 1.0f, 1.0f);  // Fondo blanco
    glMatrixMode(GL_PROJECTION);
    glLoadIdentity();
    gluOrtho2D(-1, 1, -1, 1);
}

int main(int argc, char** argv) {
    glutInit(&argc, argv);
    glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB);
    glutInitWindowSize(500, 500);
    glutCreateWindow("Flor con OpenGL");
    init();
    glutDisplayFunc(display);
    glutMainLoop();
    return 0;
}
