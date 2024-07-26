import java.io.File;
import java.io.FileNotFoundException;
import java.util.ArrayList;
import java.util.List;
import java.util.Random;
import java.util.Scanner;

public class Hangman {
    public static void main(String[] args throws FileNotFoundException)  {

        Scanner scanner = new Scanner(new File("C:/Users/chelh/OneDrive/Documents/School/CIS170 Java Programming I/HangmanWordList .txt")); //importing the word file
        Scanner keyboard = new Scanner(System.in);
        
        //start of game with welcome message and number of players
        System.out.println("Hello! Welcome to Hangman.");
        System.out.println("Are you playing with 1 or 2 players?");
        String players = keyboard.nextLine();

        String word;
        if (players.equals("1")) {
            // Scanner scanner = new Scanner(new File("C:/Users/chelh/OneDrive/Documents/School/CIS170 Java Programming I/HangmanWordList .txt"));

            List<String> words = new ArrayList<>();
            while (scanner.hasNext()) {
                words.add(scanner.nextLine());
            }
            // new Random method to randomly select a word from the list
            Random rand = new Random();
            word =words.get(rand.nextInt(words.size()));
        }
        else {
            System.out.println("Player 1, please enter your word:");
            word = keyboard.nextLine();
            System.out.println("\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n");//spacing so player 2 doesn't see player 1 word
            System.out.println("Ready for player 2! Good luck!");
        }

//        System.out.println(word);

        List<Character> playerGuesses = new ArrayList<>(); // array to store the player guesses

        // method to keep count of wrong letter guesses
        int wrongCount = 0;
        while (true) {

            printHangedMan(wrongCount);

            if (wrongCount >= 6) {
                System.out.println("You lose!");
                System.out.println("The word was " + word + ".");
                break;
            }

            printWordState(word, playerGuesses);
            if (!getPlayerGuesses(keyboard, playerGuesses, word)) {
                wrongCount++;
            }

            if (printWordState(word, playerGuesses)) {  // if the player guesses all the correct letters of the word, they win the game.
                System.out.println("You win!");
                break;
            }
            System.out.print("Please enter your guess for the word."); // allows the player a chance to guess the word, if correct, they win the game.
            if(keyboard.nextLine().equals(word)) {
                System.out.println("You win!");
                break;
            }
            else{
                System.out.println("Sorry, that is incorrect. Please try again."); // message prints for each incorrect word guess

            }
        }
    }

    private static void printHangedMan(int wrongCount) {
        System.out.println("-------"); // start of the hangman picture
        System.out.println(" |    |");
        if (wrongCount >=1) {
            System.out.println(" O"); // first incorrect guess prints the head
        }

        if (wrongCount >= 2) {
            System.out.print("\\ "); // second incorrect guess prints the left arm
            if (wrongCount >= 3) {
                System.out.println("/ ");  // third incorrect guess prints the right arm on the same line as the left arm
            }
            else {
                System.out.println(""); // prints a space so they don't all run together
            }
        }

        if (wrongCount >=4) {
            System.out.println(" |"); // forth incorrect guess prints the body
        }

        if (wrongCount >=5) {
            System.out.print("/ "); // fifth incorrect guess prints the left leg
            if (wrongCount >= 6) {
                System.out.println("\\ ");  // sixth and final incorrect guess prints the right leg on the same line as the left leg
            }
            else {
                System.out.println(""); // prints a space so they don't all run together
            }
        }
        System.out.println(""); // prints a space so they don't all run together
        System.out.println(""); // prints a space so they don't all run together
    }

    // method that takes in letter guess and if correct, prints it within it's placement in the random word
    private static boolean getPlayerGuesses(Scanner keyboard, List<Character> playerGuesses, String word) {
        System.out.println("Please enter a letter.");
        String letterGuess = keyboard.nextLine(); // gets the next string input from the player
        playerGuesses.add(letterGuess.charAt(0));  // takes the first letter in the player input, in case they enter more than one letter

        return word.contains(letterGuess);

    }
    // method to allow number of guesses for the length of the word
    private static boolean printWordState(String word, List<Character> playerGuesses) {
        int correctCount = 0;
        for (int i = 0; i < word.length(); i++) {
            if (playerGuesses.contains(word.charAt(i))) {
                System.out.print(word.charAt(i));
                correctCount++;
            } else {
                System.out.print("-");
            }
        }
        System.out.println();

        return (word.length() == correctCount);

    }
}
