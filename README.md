# Jump-Jump
Android-Paticle.io app that keeps the track of the number of jumps using a Photon device. Takes the number of jumps input from the user and update the particle device so its shows green colour on completing that task(number of jumps).
HARDWARE CODE


______________________________________________________________________________________________________

// This #include statement was automatically added by the Particle IDE.
#include <InternetButton.h>

//int initialValue;
// int initVal = initialValue + 30;
int initial;

int numberChanged;
int jumpCount;
float num;

InternetButton b = InternetButton();

void setup() {

    b.begin();
    initial = b.readZ();
    //setting the nickname as : jumpCount
    Particle.function("jumpCount", checkJumps);
    jumpCount = 0;
    num = 0;
    
    
}

void loop(){
    if ( 0 < num) {
        // b.allLedsOn( 255, 255, 255);
        // b.allLedsOff();
        // b.ledOn(1, 255, 255, 255);
        // b.ledOn(2, 255, 255, 255);
        // b.ledOn(3, 255, 255, 255);
        int zVal = b.readZ();
        
        int initVal = initial + 30;
        if (zVal > initVal) {
            zVal = initial;
            
            jumpCount += 1;
            numberChanged = (jumpCount / num) * 100 ;
            Particle.publish("jumpCount", String(jumpCount));
            
            delay(1000);
             
             for (int a = 1; a <= (numberChanged / 10); a++) {
                 b.ledOn(a, 100, 50, 50);
             }
             
             if(numberChanged == 100)
              {
                  num = 0;
                  jumpCount = 0;
                  numberChanged = 0;
                  delay(200);
                  b.allLedsOn(124,252,0);
                  delay(800);
                
                  b.allLedsOff();
              }
        }
    
    }
}
   
    int checkJumps(String command)
    {
        if( num == 0 )
        {
        num = atof(command.c_str());
       
         Particle.publish("jumps", String(num));
        }
       
    }
        
 
