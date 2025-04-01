import java.util.*;

public class WordScrambleGame {
    public static void main(String[] args) {
        Scanner s = new Scanner(System.in);
        String[] words = {"PROGRAMMING", "SCRAMBLE", "HELLO", "DEVELOPER"};
        String[] addedWords = new String[10];
        int addedCount = 0;
        int score = 0;
        int correct = 0;
        int incorrect = 0;

        
        System.out.println("\n-- Welcome to the Word Scramble Game! --");

        
        System.out.println("\n-- Word Addition Section --");
        System.out.print("Do you want to add new words? (yes/no): ");
        String response = s.next().trim();
        s.nextLine(); 
        
        while (response.equalsIgnoreCase("yes") && addedCount < addedWords.length) {
            System.out.print("Enter a word to add: ");
            addedWords[addedCount] = s.nextLine().trim();
            addedCount++;

            if (addedCount < addedWords.length) {
                System.out.print("Add another word? (yes/no): ");
                response = s.next().trim();
                s.nextLine();
            } else {
                System.out.println("Word limit reached! No more words can be added.");
            }
        }

        
        String[] allWords = new String[words.length + addedCount];
        System.arraycopy(words, 0, allWords, 0, words.length);
        System.arraycopy(addedWords, 0, allWords, words.length, addedCount);

        System.out.println("\n-- Game Start! --");

        int i = 0;
        while (i < allWords.length) {
            int wrongAttempts = 0;
            String word = allWords[i];
            String scrambled = scrambleWord(word);

            while (true) {
                System.out.println("\n-- New Word --");
                System.out.println("Scrambled word: " + scrambled);
                System.out.print("Do you want to switch to a different word? (yes/no): ");
                String switchWord = s.next().trim();

                if (switchWord.equalsIgnoreCase("yes")) {
                    i++;
                    if (i >= allWords.length) {
                        break;
                    }
                    word = allWords[i];
                    scrambled = scrambleWord(word);
                    continue;
                }

                System.out.print("Guess the word: ");
                s.nextLine();
                String guess = s.nextLine().trim();

                if (guess.equalsIgnoreCase(word)) {
                    System.out.println("\n-- Correct! The word was: " + word + " --");
                    score += 1;
                    correct += 1; //add score to correct
                    System.out.println("Your score is: " + score); //score
                    if (!askToContinue(s)) {
                        System.out.println("\n-- Thanks for playing! --");
                        System.out.println("Your correct is: " + correct); //final score
                        System.out.println("Your incorrect is: " + incorrect); //final score
                        System.out.println("Your final score is: " + score + "/" + (correct+incorrect)); //final score
                        s.close();
                        return;
                    }
                    i++;
                    break;
                } else {
                    System.out.println("-- Wrong! Try again. --");
                    wrongAttempts++;
                }

                if (wrongAttempts >= 3) {
                    System.out.println("\n-- You've made 3 mistakes! The correct word was: " + word + " --");
                    incorrect += 1;
                    if (!askToContinue(s)) {
                        System.out.println("\n-- Thanks for playing! --");
                        System.out.println("Your correct is: " + correct); //final score
                        System.out.println("Your incorrect is: " + incorrect); //final score
                        System.out.println("Your final score is: " + score + "/" + (correct+incorrect)); //final score
                        s.close();
                        return;
                    }
                    i++;
                    break;
                }
            }

            if (i >= allWords.length) {
                System.out.println("\n-- No more words left! Thanks for playing. --");
                System.out.println("Your correct is: " + correct); //final score
                System.out.println("Your incorrect is: " + incorrect); //final score
                System.out.println("Your final score is: " + score + "/" + (correct+incorrect)); //final score
                break;
            }
        }

        s.close();
    }

    public static String scrambleWord(String word) {
        char[] letters = word.toCharArray();
        for (int i = 0; i < letters.length; i++) {
            int randomIndex = (int) (Math.random() * letters.length);
            char temp = letters[i];
            letters[i] = letters[randomIndex];
            letters[randomIndex] = temp;
        }
        return new String(letters);
    }

    public static boolean askToContinue(Scanner s) {
        System.out.print("\nDo you want to continue playing? (yes/no): ");
        String response = s.next().trim();
        return response.equalsIgnoreCase("yes");
    }
}
