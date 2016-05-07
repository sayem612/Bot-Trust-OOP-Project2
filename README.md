# Bot-Trust-OOP-Project2
CISC 3150 OOP PROJECT 2 -- SIMULATION OF THE (GOOGLE CODE JAM) BOT TRUST PROBLEM

##Project Requirement
 Project 2

Many of the Google Code Jam problems, such as games, racing cars, and robots, can be solved by using simulation. For example, the Bot Trust problem can be solved by simulating how robots push the buttons; the Graduation Requirements problem can be solved by simulating how the car can drive without meeting with any other car when starting at a different intersection at a different time. Choose one from the practice problems and write a simulator in Java or C++ for solving the problem. Your simulator must meet the following requirements:

    It must correctly simulate a solution for the small data set.
    It must provide a GUI that visualizes the solution.
    It should use multiple threads to simulate multiple actors if there are multiple actors involved. 

Please make your project available on a web site, such as GitHub or SourceForge, and email a link to the page with the subject "Project-2: Name" to: cisc3150@picat-lang.org. All projects that meet the minimum requirements will receive 15 points. Outstanding projects will be awarded 5 extra points. 


## BOT TRUST (GCJ QUESTION)
Solution done by: Kazi Ullah
Problem link: https://code.google.com/codejam/contest/975485/dashboard
Problem A Bot Trust
Problem

Blue and Orange are friendly robots. An evil computer mastermind has locked them up in separate hallways to test them, and then possibly give them cake.

Each hallway contains 100 buttons labeled with the positive integers {1, 2, ..., 100}. Button k is always k meters from the start of the hallway, and the robots both begin at button 1. 
Over the period of one second, a robot can walk one meter in either direction, or it can press the button at its position once, or it can stay at its position and not press the button. 
To complete the test, the robots need to push a certain sequence of buttons in a certain order. Both robots know the full sequence in advance. How fast can they complete it?

For example, let's consider the following button sequence:

   O 2, B 1, B 2, O 4
   
Here, O 2 means button 2 in Orange's hallway, B 1 means button 1 in Blue's hallway, and so on. 
The robots can push this sequence of buttons in 6 seconds using the strategy shown below: 


Time | Orange           | Blue
-----+------------------+-----------------
  1  | Move to button 2 | Stay at button 1
  2  | Push button 2    | Stay at button 1
  3  | Move to button 3 | Push button 1
  4  | Move to button 4 | Move to button 2
  5  | Stay at button 4 | Push button 2
  6  | Push button 4    | Stay at button 2
  
  Note that Blue has to wait until Orange has completely finished pushing O 2 before it can start pushing B 1.

Input

The first line of the input gives the number of test cases, T. T test cases follow.

Each test case consists of a single line beginning with a positive integer N, representing the number of buttons that need to be pressed. 
This is followed by N terms of the form "Ri Pi" where Ri is a robot color (always 'O' or 'B'), and Pi is a button position.
Output

For each test case, output one line containing "Case #x: y", where x is the case number (starting from 1) and y is the minimum number
of seconds required for the robots to push the given buttons, in order.
Limits

1 ≤ Pi ≤ 100 for all i.
Small dataset

1 ≤ T ≤ 20.
1 ≤ N ≤ 10.
Large dataset

1 ≤ T ≤ 100.
1 ≤ N ≤ 100.

Sample

Input
3
4 O 2 B 1 B 2 O 4
3 O 5 O 8 B 100
2 B 2 B 1
Output
Case #1: 6
Case #2: 100
Case #3: 4                                      */



## What will be used to solve the problem
I will be using Processing. 

### What is Processing?
Processing is an open source programming language and integrated development environment (IDE) built for the electronic arts, new media art, and visual design communities with the purpose of teaching the fundamentals of computer programming in a visual context, and to serve as the foundation for electronic sketchbooks. 
For more information go to: https://en.wikipedia.org/wiki/Processing_%28programming_language%29

##CHECK THE VIDEO FOR THE SIMULATION OF THIS PROLEM* 
#SOLUTION TO THE PROBLEM USING SIMULATION: (SOURCE CODE)

import static java.lang.Math.abs;

import static java.lang.Math.min;

import static java.lang.Math.max;

 import java.io.FileInputStream;
 
 import java.io.FileNotFoundException;
 
 import java.io.FileOutputStream;
 
 import java.io.BufferedReader;
 
 import java.io.BufferedInputStream;
 
 import java.io.PrintStream;
 
import java.text.CharacterIterator;

import java.text.StringCharacterIterator;

import java.util.ArrayList;

import java.util.List;

 import java.util.Scanner;
 
int  min_x=0;

int min_y=0;

int max_x=1560;

int max_y=800;

int grid_size=15;

PVector orangeRobot= new PVector(0,0);

PVector blueRobot= new PVector(0,0);

 ArrayList<String> positionX= new ArrayList<String> ();
 
 //The following ArrayLists are to keep track of the buttons pressed
 
 ArrayList<PVector> OrangeButtonPressed= new ArrayList<PVector>();
 
 ArrayList<PVector> BlueButtonPressed= new ArrayList<PVector>();
 
 PFont f;//STEP 1 declare font
 
 int i=1;  //for keeping track of the case number
 
 int j=1;//this is for arraylist of button pressed by the orange robot
 
 int k=1;//this is for arraylist of button pressed by the blue robot
 
 PVector zero1;
 
 PVector zero2;
 
 int time=0;
 
 boolean Running;//This variable is used to determine if to run the simulation or stop(pause)
 

void settings()

{

  size(max_x, max_y);
  
}

void setup(){

  f = createFont("Arial",16,true); // STEP 2 Create Font
  
  ellipseMode(CORNER);
  
  frameRate(4);
  
  orangeRobot=new PVector(30, 60);
  
  blueRobot= new PVector(30,120);
  
  positionX= botTrust();
  
  Running=true;
  
zero1= new PVector(0,0);

zero2= new PVector(15,0);

 OrangeButtonPressed.add(zero1);
 
 BlueButtonPressed.add(zero2);
 
}

void draw(){

   // botTrust();
   
  background(#ffffff);
  
  fill(#00FF00);
  
    rect( 30,60, 1515, grid_size);
    
     rect( 30,120, 1515, grid_size);
     
   for ( int x=min_x; x<=max_x; x+=grid_size ) {
   
    line( x,min_y,x,max_y );
    
  }

  //drawing the button pressed by the Orange Robot
  
   if((OrangeButtonPressed.size()-1)!=-1){
   
 // PVector buttonpressed=OrangeButtonPressed.get(OrangeButtonPressed.size()-1);

 // if (buttonpressed.x<=orangeRobot.x){

   if(j==i){

  OrangeButtonPressed.add(ButtonClicked(orangeRobot));

   for (PVector e: OrangeButtonPressed){

     fill(255,0,0);

     rect( e.x, e.y, grid_size, grid_size );

   }

  }

  else{

    OrangeButtonPressed.clear();

    OrangeButtonPressed.add(zero1);

    j=i;

  }

  }
  
  //drawing the button pressed by the Blue Robot

  if((BlueButtonPressed.size()-1)!=-1){

  //PVector buttonpressed=BlueButtonPressed.get(BlueButtonPressed.size()-1);

  //if (buttonpressed.x<=blueRobot.x){

    if(k==i){

  BlueButtonPressed.add(ButtonClicked(blueRobot));

   for (PVector e: BlueButtonPressed){

     fill(255,0,0);

     rect( e.x, e.y, grid_size, grid_size );

   }

  }

  else{

    BlueButtonPressed.clear();

    BlueButtonPressed.add(zero2);

    k=i;

  }

  }

  
  for ( int y=min_y; y<=max_y; y+=grid_size ) {
  
    line( min_x,y,max_x,y );
  
  }
 
 //  stroke( #0000ff );
 
  fill( #0000ff );
 
  ellipse( blueRobot.x, blueRobot.y, grid_size, grid_size );
 
 
  fill(#ffa500);
 
   ellipse( orangeRobot.x, orangeRobot.y, grid_size, grid_size );
   
   
   
   if(Running==true) {
 
   /**Function call to determine location based on the data read from the chart*/
 
   presentPositions();
 
   }
 
   /** Data on the sketch*/
 
   textFont(f,24);  // STEP 3 Specify font to be used
 
   fill(#FFFF00);
 
   rect(600,550, 400,150);
 
  fill(0);                         //STEP 4 Specify font color 
 
  text( "Case#"+ (i-2) + " Total Run Time: " + time ,600,600);   // STEP 5 Display Text
 
   text( "This time Orange at Button: "+(int)(orangeRobot.x/15-2),600,640);   
 
    text( "This time Blue at Button: "+(int)(blueRobot.x/15-2),600,680);  
   
}


/** Press any key to pause the simulation*/

void keyPressed(){

  if (keyPressed==true){

    if(Running==true){

      Running=false;

    }

    else{

      Running=true;
    }

  }

}

/** This function is to test if the robots are moving */ 

void NewMove(){

if (orangeRobot.x<(max_x-15)){

  orangeRobot.x+=grid_size;

  orangeRobot.y=60;

}

  if (blueRobot.x<(max_x-15)){

blueRobot.x+=grid_size;

blueRobot.y=120;

  }

}


/** This function collects the postions data for the robots from the data file and return a list with that information*/

ArrayList<String> botTrust() {

  ArrayList<String> orangeBlueX= new ArrayList<String>();

    try{

      
     //File CP_file = new File("C:/Users/XYZ/workspace/Customer_Product_info.txt");

      System.setIn( new FileInputStream("C:/Users/Kazi/Desktop/Bot_Trust/A-small-practice.in"));

      System.setOut(new PrintStream(new FileOutputStream("C:/Users/Kazi/Desktop/Bot_Trust/A-small-practice.out")));

      
      
      //ArrayList <Integer> blueX= new ArrayList<Integer>();
      
      
      Scanner in = new Scanner(System.in);

      int tests = in.nextInt(); in.nextLine();

      for (int test=1; test<= tests; test++) {      
     
        int time = 0;

        int posOrange = 1, posBlue = 1;

        int timeOrange = 0, timeBlue = 0;
        
        int n = in.nextInt();

       // orangeRobot.x=30;

       // blueRobot.x=30;
      
        orangeBlueX.add("O");

        orangeBlueX.add("30");

        orangeBlueX.add("0");

        orangeBlueX.add("B");

        orangeBlueX.add("30");

         orangeBlueX.add("0");
        
          ;

        for (int i=0; i<n; i++) {
          
         
          if ("O".equals(in.next())) {

             int button = in.nextInt();

              int add =  max(1 - timeOrange, abs(button-posOrange) + 1 - timeBlue);

             timeOrange += add;

            time += add;

             timeBlue = 0;
            
             posOrange = button;

            // orangeRobot.x=(grid_size*button);

            orangeBlueX.add( "O");
            
            int x1= grid_size*button+30;

            orangeBlueX.add(Integer.toString(x1));
            
             orangeBlueX.add(Integer.toString(time));
             
          } 
          
          else {
       
             int button = in.nextInt();
       
              int add =  max(1 - timeBlue, abs(button-posBlue) + 1 - timeOrange);
       
             timeBlue += add;
       
            time += add;
       
            timeOrange = 0;
          
             posBlue = button;
          
           //   blueRobot.x=(grid_size*button);
          
             orangeBlueX.add( "B");
          
            int x2= grid_size*button+30;
          
            orangeBlueX.add(Integer.toString(x2));
          
             orangeBlueX.add(Integer.toString(time));
          
          }     
        
        }
        
        System.out.println(String.format("Case #%d: %d", test, time));
      
      }
      
    }
    
        catch(IOException ioe){
    
    System.out.println("Error: " +ioe);
    
    }

  return orangeBlueX;

}


/** This functions determines the positions of the Robots based 

on the data collected through the function call for( BotTrust() ) */

void presentPositions(){

if(positionX.isEmpty()==false){
         
   if(positionX.get(0)=="O"){

     positionX.remove(0);

     orangeRobot.x=Integer.parseInt(positionX.get(0));

     positionX.remove(0);

   }

   else if(positionX.get(0)=="B"){

     positionX.remove(0);

     blueRobot.x=Integer.parseInt(positionX.get(0));

     positionX.remove(0);

   }

    time=Integer.parseInt(positionX.get(0));

          positionX.remove(0);

   if((orangeRobot.x==30)&&(blueRobot.x==30)){

     i++;
   
   }

   }

}

/** This function returns the PVector (Button location) 

pressed by the agent passed in the parameter*/

 PVector ButtonClicked(PVector agent){

   PVector PressedButton= new PVector(agent.x, agent.y);

   return PressedButton;

 }
   
   
   
