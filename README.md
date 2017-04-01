//snake positioning
ArrayList<Integer> x = new ArrayList<Integer>(), y = new ArrayList<Integer>();

//the board
int w = 30; //width
int h = 30; //height
int bs = 20; //block space

//moving the snake in a direction
int dir = 2; //direction
int [] dx = {0,0,1,-1}, dy = {1,-1,0,0}; //x and y direction

//snake food
int foodx = 12;
int foody = 10;

//gameover
boolean dead = false;

void setup(){
  size (600,600); 
  
  //the starting coordinates 
  x.add(5);
  y.add(5);
}

void draw(){
////////////////// CHANGE BACKGROUND
  background(255);
//////////////////  
  
  //optional grid
  for(int i = 0 ; i < w; i++) line(i*bs, 0, i*bs, height);
  for(int i = 0 ; i < h; i++) line(0, i*bs, width, i*bs);
  
  //the snake
  for(int i = 0 ; i < x.size(); i++) {
    
////////////////// CHANGE COLOR AND SHAPE
    fill(0,0,0); //color of snake
    rect(x.get(i)*bs, y.get(i)*bs, bs, bs); //shape of snake
//////////////////
  }
  
  //food variables
////////////////// CHANGE COLOR AND SHAPE, ADD IMAGE POSSIBLY
  fill(0,0,0); //color of food
  rect(foodx*bs, foody*bs, bs, bs); //shape of food
//////////////////
  
  //IF player is NOT dead, this happens
  if(!dead){ 
    
    //moving force that keeps the snake from over growing
    if(frameCount%5==0){
      x.add(0,x.get(0) + dx[dir]);
      y.add(0,y.get(0) + dy[dir]);
      
      //the player will die if snake hits itself
      for(int i = 1; i < x.size(); i++) if(x.get(0)==x.get(i) && y.get(0)==y.get(i)) dead = true;
      
      //IF player hits any boarders, player dies
      if(x.get(0) < 0 || y.get(0) < 0 || x.get(0) >= w || y.get(0) >= h) dead = true;
     
      //eat the food and make new one somewhere random
      if(x.get(0)==foodx && y.get(0)==foody){
        foodx = (int)random(0,w);
        foody = (int)random(0,h);
      }else {
        x.remove(x.size()-1);
        y.remove(y.size()-1);
      }
    }
  //WHEN player dies, this happens
  }else {
    
////////////////// CHANGE FONT, COLOR, SIZE, AND TEXT (MAYBE ANIMATION)
    fill(0); //text color
    textSize(30); //text size
    textAlign(CENTER);
    text("GAME OVER", width/2, height/2); //text
//////////////////
    
    //press SPACE to start over
    if(keyPressed&&key==' ') {
      x.clear();
      y.clear();
      x.add(5);
      y.add(5);
      dead = false;
    }
  }
}

//the controls WASD
void keyPressed() {
               //s = UP       //w = DOWN       //d = RIGHT     //a = LEFT
  int newdir = key=='s' ? 0 : (key=='w' ? 1 : (key=='d' ? 2 : (key=='a' ? 3 : -1)));
  if(newdir != -1 && (x.size() <= 1 || !(x.get(1)==x.get(0)+dx[newdir] && y.get(1)==y.get(0)+dy[newdir]))) dir = newdir;
}
