# Scramble-java-game
Java game project


//The first thing we do is import the java utilities
import java.util.*;

import java.io.*;
//Now we have our dictionary class which we will use for verification of words accuracy in the game
class Dictionary{
     
    private String input[]; 

    public Dictionary(){
        input = load("C://words.txt");  
    }
    
    public int getSize(){
        return input.length;
    }
      
    public String getWord(int n){
        return input[n];
    }
    
    private String[] load(String file) {
        File aFile = new File(file);     
        StringBuffer contents = new StringBuffer();
        BufferedReader input = null;
        try {
            input = new BufferedReader( new FileReader(aFile) );
            String line = null; 
            int i = 0;
            while (( line = input.readLine()) != null){
                contents.append(line);
                i++;
                contents.append(System.getProperty("line.separator"));
            }
        }catch (FileNotFoundException ex){
            System.out.println("Can't find the file - are you sure the file is in this location: "+file);
            ex.printStackTrace();
        }catch (IOException ex){
            System.out.println("Input output exception while processing file");
            ex.printStackTrace();
        }finally{
            try {
                if (input!= null) {
                    input.close();
                }
            }catch (IOException ex){
                System.out.println("Input output exception while processing file");
                ex.printStackTrace();
            }
        }
        String[] array = contents.toString().split("\n");
        for(String s: array){
            s.trim();
        }
        return array;
    }
}


//This is the beginning of our game class
public class Game {
	
//this is the method for calculating each words score	
public static int scoreCalculator(String userWord, String [] userWordChars) {
		//calculating the word score
		int score = 0;
		for(int i = 0;i<userWordChars.length;i++) {
			if(userWordChars[i].equals("Z")||userWordChars[i].equals("Q")) {
				score += 10;
			}
			else if(userWordChars[i].equals("J")||userWordChars[i].equals("X")){
				score += 8;
			}
			else if(userWordChars[i].equals("K")){
				score += 5;
			}
			else if(userWordChars[i].equals("F")||userWordChars[i].equals("H")
					||userWordChars[i].equals("V")||userWordChars[i].equals("W")
					||userWordChars[i].equals("Y")){
				score += 4;
			}
			else if(userWordChars[i].equals("B")||userWordChars[i].equals("C")
					||userWordChars[i].equals("M")||userWordChars[i].equals("P")){
				score += 3;
			}
			else if(userWordChars[i].equals("G")||userWordChars[i].equals("D")){
				score += 2;
			}
			else {
				score += 1;
			}
			
		}
		if(userWord.length()==7) {
			score += 50;
		}
		return score;
	}

//This method is used to check if the players entered letters match the letters they had in hand
public static boolean letterMatch(String [] userWordChars,String [] userLetters) {
	
		boolean letterMatch = false;
		for(int i = 0;i<userWordChars.length;i++) {
		
			if(userWordChars[i].equals(userLetters[0])||userWordChars[i].equals(userLetters[1])|| 
				userWordChars[i].equals(userLetters[2])||userWordChars[i].equals(userLetters[3])||
				userWordChars[i].equals(userLetters[4])||userWordChars[i].equals(userLetters[5])||
				userWordChars[i].equals(userLetters[6])) {
				letterMatch = true;
			}
			else {
				letterMatch = false;
			}
		}
		return letterMatch;
}

//this method checks if the players entered word can be found in the dictionary
public static boolean wordMatch (String str,Dictionary dict) {
	boolean wordMatch = false;
	for(int i =0;i<dict.getSize()-1;i++) {
		String temp = dict.getWord(i).substring(0,dict.getWord(i).length()-1);
		if(str.equalsIgnoreCase(temp)) {
			wordMatch = true;
			break;
		}
	}
	return wordMatch;
}

//In the case of an illegal letter entry, this method will return the illegal letter
public static String errorLetter(String [] userWordChars,String [] userLetters) {
	//this is a method to check which input was incorrect
		boolean letterMatch = false;
		String errorLetter = "";
		for(int i = 0;i<userWordChars.length;i++) {
		
			if(userWordChars[i].equals(userLetters[0])||userWordChars[i].equals(userLetters[1])|| 
				userWordChars[i].equals(userLetters[2])||userWordChars[i].equals(userLetters[3])||
				userWordChars[i].equals(userLetters[4])||userWordChars[i].equals(userLetters[5])||
				userWordChars[i].equals(userLetters[6])) {
				letterMatch = true;
			}
			else {
				letterMatch = false;
				errorLetter = userWordChars[i];
			}
		}
		return errorLetter;
}

//This method updates the players 7 letters after every round so they can replace their used letters
public static String [] updateLetters(String [] userWordChars,String [] userLetters, String [] letterSet) {
	for(int i = 0;i<userWordChars.length;i++) {
		for(int j = 0;j<userLetters.length;j++) {
			if(userWordChars[i].equals(userLetters[j])) {
				userLetters[j] = letterSet[(int)Math.round(Math.random()*97)];
				userWordChars[i] = "";
			}
		}
	}
	return userLetters;
}
	
//Our main method starts here
public static void main(String[]args) {
	Dictionary dict = new Dictionary();
	
	//beginning output is a welcome message to the game
	System.out.println("Welcome to Scramble!");
	System.out.println("~~~~~~~~~~~~~~~~~~~~~~~");
	System.out.println("The Rules are as follows!");
	System.out.println("You must make the best word you can out of your 7 letters.\n"
			+ "If you use any other letters or if your word is invalid\n"
			+ "you will score nothing for that round so choose your words carefully!\n\n"
			+ "if you want to change your letters you can do so by entering them as a word\n"
			+ "you will score nothing, but this is the price to change your letters\n"
			+ "you can change just 1 letter, or any amount up to 7, it's up to you.\n\n"
			+ "Some letters are worth more than others, so the longest doesn't always mean the best...\n"
			+ "(Z,Q)10p -- (J,X)8p -- (K)5p -- (F,H,V,W,Y)4p -- (B,C,M,P)3p -- (G,D)2p -- (Others)1p\n\n"
			+ "However if you can use all of your tiles in one word...You score an extra 50 points!!!\n"
			+ "You have 15 rounds! Score 100 or more points to win....Good Luck!!");
	System.out.println("~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~");
	System.out.println("Below are your seven letters, underneath these letters please type and enter your best word");
	
	
	//initialising the alphabet array
	String [] letterSet = new String [] {"E","E","E","E","E","E","E","E","E","E","E","E",
			"A","A","A","A","A","A","A","A","A","I","I","I","I","I","I","I","I","I",
			"O","O","O","O","O","O","O","O","N","N","N","N","N","N","R","R","R","R","R","R",
			"T","T","T","T","T","T","L","L","L","L","S","S","S","S","U","U","U","U","D","D","D","D",
			"G","G","G","B","B","C","C","M","M","P","P","F","F","V","V","W","W","Y","Y","H","H","K",
			"J","X","Y","Z"
			};
	

	//initialising the players array of 7 letters
	String [] userLetters = new String [7];
	for(int x = 0; x < userLetters.length;x++) {
		
	userLetters[x] = letterSet[(int)Math.round(Math.random()*97)];
	
	}
	//initialising our counters, roundCurrent keeps track of the current round
	//round decreases each round until it hits zero
	//scoretotal updates the total score after every round
	int round = 15;
	int roundCurrent = 1;
	int scoreTotal = 0;
	
	
while(round>0) {      //the start of our 15 round while loop
	
	//print out which round we are in
	System.out.print("Round " + roundCurrent + ": ");
	
	// printing players letters to the screen
	for(int i = 0;i<userLetters.length;i++) {
		
		System.out.print(userLetters[i] + " ");	
	}
	
	System.out.println();
	//take in the users word
	Scanner sc = new Scanner(System.in);
	String userWord = sc.nextLine().toUpperCase();
	
	//break down the players word into a string array with each character separated
	String [] userWordChars = new String [userWord.length()];
	for(int i = 0;i<userWordChars.length;i++) {
		userWordChars[i] = userWord.substring(i,i+1);
	}
	
	// our score variable for each word
	int scoreCurrentWord = 0;
	
	//if the letters are legal and the word is legal we print out their word, the word score, and the total score
	if(letterMatch(userWordChars,userLetters)==true&&wordMatch(userWord,dict)==true) {
		scoreCurrentWord = scoreCalculator(userWord,userWordChars);
		scoreTotal = scoreTotal + scoreCurrentWord;
		System.out.println("Your word " + userWord + " is accepted");
		System.out.println("your score for this word is " + scoreCurrentWord);
		System.out.println("your total score is " + scoreTotal);
	}
	
	//or else if both letters and word is illegal we print the failure message and a score of zero
	else if (wordMatch(userWord,dict)!=true&&letterMatch(userWordChars,userLetters)!=true){
		System.out.println("The word " + userWord + " is not a valid word");
		System.out.println("And...There are no '" + errorLetter(userWordChars,userLetters) + "'s in your 7 letters");
		System.out.println("You score zero for this round...Maybe you should read the rules again........");
	}
	
	//If just the letters were illegal we print that error out and a score of zero
	else if (letterMatch(userWordChars,userLetters)!=true){
		System.out.println("There are no '" + errorLetter(userWordChars,userLetters) + "'s in your 7 letters");
		System.out.println("You score zero for this round...sorry, please try pay attention to the letters next time");
	}
	//if just the word was illegal we print that error out and a score of zero
	else if (wordMatch(userWord,dict)!=true){
		System.out.println("The word " + userWord + " is not a valid word");
		System.out.println("You score zero for this round...sorry, please try a real word next time....");
	}
	
	//If we still have more rounds to play we let the player know we've updated their letters
	if(round>1) {
		System.out.println("We've replaced your " + userWordChars.length + " tiles with new ones");
	}
	
//calling the method to update the players hand of 7 letters
updateLetters(userWordChars,userLetters,letterSet);

//round decreases by one until 15 rounds have been played
round --;

//current round increase by one to let the player know which round we're on
roundCurrent++;

}//end of while loop

		//printout to let you know the game has finished and your total score
       System.out.println();
       System.out.println("~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~");
       System.out.println("After 15 rounds your score in Scramble is " + scoreTotal);
       
       //if total score is 100 or more you get a congratulations message
   if(scoreTotal>=100) {
	   System.out.println("Congratulations!!!! This means you win the game!!");
	   System.out.println("Try refreshing the game and playing again");
	}
   
   //otherwise you don't.....
      else {
    	  System.out.println("You Lose!!! You need to score 100 or more to win scramble!");
    	  System.out.println("Try refreshing and playing again...redeem some dignity");
      }
   System.out.println("~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~");
}
}
