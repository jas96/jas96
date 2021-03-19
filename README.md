// on the top of your Program File you are submitting:
// use comments:
// Name:Jasmeen kaur
// Student ID: 201903583
// Date Submitted:20-03-2021
// Class IN2203 Section Number
// Name of work: Assignment 1: The Gambling Game
import java.util.Random;
import java.util.Scanner;
import java.lang.Math;
import java.io.*;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
public class main.java {
    public static void main(String[] names) {
        System.out.println("Welcome to this random game of selection");
        
        if(names.length == 0){
            System.out.println("NO USER IS ENTERING INTO THE CASINO");
            System.out.println("__________________________________________");
            System.exit(0);
        }else if(names.length > 1 ){ //following is the condition to 
                                   // check that only one person will 
                                   // enter into the Casino       
            System.out.println("More than one Player is not allowed in Casio"
                    + " at a time");
        }else{ // this is else condition that will execute is only one user is 
                // entering into Casino
            Player player = new Player(names[0]);
            Casino casino = new Casino(player);
            System.out.println("Welcome \""+player.getName()+"\" You have successfully entered the Casino");
            System.out.println("-------------------------------------");
            casino.showPlayer();
            casino.playGame();
        }
    }
}
public class Player {
    private String name;
    
    Player(String name){
        this.name = name;
    }
    
    void showName(){
        System.out.println("Name of Player is : " + this.name);
    }
    
    String getName(){
        return name;
    }
    
}
class Casino{
    private Player player;  //this is player that will enter in casino
    private boolean playerWins;
    
    Casino(Player player) { // Constructor of Casino class
        this.player = player;
        playerWins = false;
    }
    
    void showPlayer(){  // This will show which player have entered into Casino
        this.player.showName();
    }
    
    void playGame(){    //This function will be called to play game
        System.out.println("Please select following option to start game");
        System.out.print("Enter \"YES\" to play game and \"NO\" to exit : ");
 
        //Input buffer to get input from console
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        
        //this is try catch to catch input exceptions
        try{
            String option = br.readLine();//this will read from console for option from user
            //this is condition is user enters yes(in any case - lower or upper)
            if(option.toLowerCase().equals("yes")){
                System.out.println("Entered yes");
                Game game = new Game(this.player);
                game.play();
                if(!playerWins){
                    System.out.println("=========================================================");
                    System.out.println("SORRY!! "+this.player.getName()+" you loss this Game");
                    System.out.println("=========================================================");
                }
                game.showResult();
                
            }else{//this else will execute if user will enters any other values.
                System.out.println("You exit from Casino without playing game");
            }
        }catch(IOException e){//this will execute if some exception will occur.
            System.out.println("This is an exception for out input");
        }
    }
    
    //this is nested class Game inside Casino as Games are always inside casino
    class Game{

        private Player player;          //player who wants to play game
        List<Integer> computerNumbers ; //list of values that computer will select
        List<Integer> playerNumbers;    //list of values that player will enter
        
        Game(Player player){    //constructor of Game class
            this.player = player;
            this.computerNumbers = new ArrayList<>();
            this.playerNumbers = new ArrayList<>();
        }
        
        //Function to play game
	void play(){
            //this loop is give player 5 chances.
            for( int i  = 5 ; i>0; --i){
                //this function will give player a chance to beat computer
                //in random number game
                System.out.println("____________________________");
                
                if(chances(i)){//if chances(i) will return true that means player wins and loop will break
                    break;
                }
            }
	}
        
        // this will return true value if player wins otherwise false
	private boolean chances(int i){

            // create instance of Random class 
            Random randomNumber = new Random(); 

            // Generate random integers in range 1 to 100 
            int computerNumber = randomNumber.nextInt(100);
            computerNumber += 1;  
            this.computerNumbers.add(computerNumber); // adding computer number in list
            
            //this is Scanner class to get values from console
            Scanner console = new Scanner(System.in);
	    int userNumber;
		    
            System.out.println("Enter random number between 0-100 : (only "+i+" chances left)");
            // This reads the input provided by user
            System.out.print("Enter a valid number: ");
            
            // try catch to get value from user and caught exceptions
            try 
            {
                userNumber = Integer.parseInt(console.nextLine());
                if(userNumber > 100 || userNumber < 1 ){
                    System.out.println("Entered Number not in Range (1-100) ");
                    this.playerNumbers.add(-1);
                    return false;
                }
                this.playerNumbers.add(userNumber);

            }catch (NumberFormatException e) 
            {                    
                this.playerNumbers.add(-1);
                System.out.println("Invalid. Not a number. Try again.");
                return false;
            }

            int difference = Math.abs(userNumber-computerNumber);
            if(difference <= 10 ){
                System.out.println("====================================");
                System.out.println("CONGRATULATIONS : " + this.player.getName());
                System.out.println("YOU WIN FROM COMPUTER");
                System.out.println("====================================");
                playerWins = true;
                return true;
            }
            return false;
        }
        
        //This will show result after after game finished
        private void showResult(){
            System.out.println("-----------------------------------------------------------------------");
            System.out.println("FINAL RESULT OF GAME");
            for (int i = 0; i < computerNumbers.size(); i++) {
                if (this.playerNumbers.get(i) == -1){
                    System.out.println("ComputerNumber : "+this.computerNumbers.get(i)+" INVALID USER NUMBER");
                }
                int difference = Math.abs(this.computerNumbers.get(i)-this.playerNumbers.get(i));
                System.out.println("ComputerNumber : "+this.computerNumbers.get(i)+"  Difference: "+ difference+"  UserNumber: "+this.playerNumbers.get(i));

            }
        }
    }
}
