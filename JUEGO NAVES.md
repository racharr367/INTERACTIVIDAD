//# INTERACTIVIDAD
//Diseño de un juego básico- Processing


Nave mi_nave;
NaveEnemiga2 nave_enemiga2;
NaveEnemiga nave_enemiga;
PImage fondo;
PImage inicio;
int pantalla;
PFont press;


void setup() {

  pantalla = 1;
  fondo = loadImage("background.jpg");
  inicio = loadImage("incio.jpg");
  press = createFont("ACaslonPro-Bold-48", 20);
  textFont(press);
  fill(126);
  size(500, 720);

  mi_nave= new Nave();
  nave_enemiga = new NaveEnemiga();
  nave_enemiga2 = new NaveEnemiga2();
}

void draw() {

  image (inicio, 0, 0);

  if (pantalla==1) {

    textAlign(CENTER);
    text("shoot", 250, 400);
    text("Press Click to start", 250, 430);
    if (mousePressed == true) {
      pantalla = 2;
    }
  }




  if (pantalla==2) {

    image(fondo, width/2, height/2, width, height);

    nave_enemiga.mover();
    nave_enemiga.display();
    nave_enemiga2.mover();
    nave_enemiga2.display();
    mi_nave.display();


    if (nave_enemiga2.nacer()) {
      nave_enemiga2= new  NaveEnemiga2();
    }
    if (nave_enemiga.nacer()) {
      nave_enemiga= new  NaveEnemiga();
    }
  }
}


void impacto() {
  
  if (nave_enemiga.getPosX() == mi_nave.bala.getPosX()+100
    && nave_enemiga.getPosX() == mi_nave.bala.getPosX()+100
    && nave_enemiga.getPosY() == mi_nave.bala.getPosY()+100
    && nave_enemiga.getPosY() == mi_nave.bala.getPosY()+100) {
    println("impacto");
    //nave_enemiga= new  NaveEnemiga();
    nave_enemiga.posY+=500;
  }
}

void keyPressed() {
  if (keyCode == RIGHT) {
    mi_nave.moverDer();
  }
  if (keyCode == LEFT) {
    mi_nave.moverIzq();
  }
  if (keyCode == CONTROL) {
    mi_nave.disparar();
  }
}


//BALA


class Bala {
  PImage imagen;
  float posX;
  float posY;
  String nombre;
  float desp;

  Bala() {
    posX = width/2;
    posY = height/2;
    nombre = "bala";
    imagen = loadImage("bullet.png");
    desp = -50.0;
  }

  void setPosX(float newX) {
    posX = newX;
  }

  void setPosY(float newY) {
    posY = newY;
  }
  float getPosX() {
    return posX;
  }

  float  getPosY() {
    return posY;
  }

  void mover() {
    if (posY > -1)
      posY = posY + desp;         
    else
      posY = height;
      
  }

  void display() {
    imageMode(CENTER);
    image(imagen, posX, posY, 40, 40);
  }
}


//NAVEBUENA

class Nave {
  PImage imagen;
  float posX;
  float posY;
  String nombre;
  Bala bala;
  boolean disparando;

  Nave() {
    posX = width/2;
    posY = 0.8*height;

    nombre = "naveespacial";
    imagen = loadImage("buena.png");
    disparando = false;
    bala = new Bala();
    bala.setPosX(posX);
    bala.setPosY(posY);
  }

  void setPosX(float newX) {
    posX = newX;
  }

  void setPosY(float newY) {
    posY = newY;
  }

  void moverIzq() {
    if (posX>0) {
      posX = posX - 10;
    }
    if (!disparando) {
      bala.setPosX(posX);
    }
  }

  void moverDer() {
    if (posX<width) {
      posX = posX + 10;
    }
    if (!disparando) {
      bala.setPosX(posX);
    }
  }

  void disparar() {
    disparando = true;
  }

  void updateBala() {
    if (bala.getPosY() > -30) {
      bala.setPosY(bala.getPosY()-20);
    } else {
      bala.setPosY(posY);
      disparando = false;
      println("balaX"+ mi_nave.bala.getPosX());
      println("balaY"+ mi_nave.bala.getPosY());
      println("NaveX"+ nave_enemiga.getPosX());
      println("NaveY"+ nave_enemiga.getPosY());
    }
  }

  void display() {
    imageMode(CENTER);
    image(imagen, posX, posY, 60, 100);

    if (disparando) {
      updateBala();
    }

    bala.display();
  }
}


// NAVE ENEMIGA

class NaveEnemiga2 {
  PImage imagen;
  float posX;
  float posY;
  float desp;
  float tam;

  NaveEnemiga2() {
    posX = random(0, width);
    posY = 0;
    tam = random(80, 150);
    desp = random(3.0, 5.0);
    imagen = loadImage("mala.png");
  }


  float getPosX() {
    return posX;
  }

  float  getPosY() {
    return posY;
  }
  void mover() {

    posY = posY + desp;
  }

  void detener() {
    posX = height +500;
    posY = width +500;
  }

  void display() {

    imageMode(CENTER);
    imagen = loadImage("mala.png");
    image(imagen, posX, posY, tam, tam);
  }
  
boolean nacer () {
  
  if (posY > height){
    return true;
  } else 
  return false;
}
}

class NaveEnemiga {

  PImage imagen;
  float posX;
  float posY;

  float desp;
  float tam;

  NaveEnemiga() {
    posX = random(0, width);
    posY = 0;
    tam = random(80, 145);

    desp = random(3.0, 5.0);
    imagen = loadImage("mala.png");
  }


  float getPosX() {
    return posX;
  }

  float  getPosY() {
    return posY;
  }
  void mover() {

    posY = posY + desp;
  }


  void deneter() {
    posX = height +500;
    posY = width +500;
  }

  void display() {

    imageMode(CENTER);
    imagen = loadImage("mala.png");
    image(imagen, posX, posY, tam, tam);
  }
  boolean nacer() {
  
  if (posY > height){
    return true;
  } else 
  return false;
}
}


