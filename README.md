#include <stdio.h>
#include <string.h>
#include <ctype.h>
#include <time.h>
#include <stdlib.h> // For srand, rand, and system

#ifdef _WIN32
    #include <conio.h> // For _getch
    #include <windows.h> // For Sleep
#else
    #include <unistd.h> // For usleep
#endif

#define MAX_WRONG_GUESSES 10 // Maximum number of incorrect guesses allowed
#define MAX_WORD_LENGTH 20   // Maximum length of the word to be guessed

// Function prototypes
const char* selectRandomWord(const char* words[], int numWords);
int checkGuess(const char* word, char guess, char* guessedWord);
void clearConsole();
void sleepMilliseconds(int milliseconds);
void waitForKeyPress();

// Function to select a random word from a list
const char* selectRandomWord(const char* words[], int numWords) {
    srand((unsigned int)time(NULL)); // Seed the random number generator with the current time
    return words[rand() % numWords]; // Return a random word from the list
}

// Function to check if the guessed letter is in the word
int checkGuess(const char* word, char guess, char* guessedWord) {
    int found = 0;
    for (int i = 0; word[i] != '\0'; i++) {
        if (tolower(word[i]) == guess) { // Convert to lowercase for case-insensitive comparison
            guessedWord[i] = word[i]; // Update the guessed word with the correct letter
            found = 1;
        }
    }
    return found;
}

// Function to clear the console
void clearConsole() {
    #ifdef _WIN32
        system("cls");
    #else
        system("clear");
    #endif
}

// Function to sleep for a specified number of milliseconds
void sleepMilliseconds(int milliseconds) {
    #ifdef _WIN32
        Sleep(milliseconds);
    #else
        usleep(milliseconds * 1000);
    #endif
}

// Function to wait for a key press
void waitForKeyPress() {
    #ifdef _WIN32
        _getch();
    #else
        getchar();
    #endif
}

int main() {
    // Lists of words in different categories
    const char* animals[] = {"cow", "dog", "tiger", "lion", "fox", "monkey", "cat", "horse", "camel", "goat"};
    const char* countries[] = {"germany", "belarus", "bhutan", "china", "poland", "brazil", "korea", "turkey", "nepal", "brunei"};
    const char* flowers[] = {"rose", "tulip", "abutilon", "sunflower", "orchid", "daisy", "jasmine", "aconite", "peony", "begonia"};
    const char* people[] = {"akib", "ridoy", "masud", "supta", "rocky", "nuwash", "jayed", "fardin", "alvi", "shovan"};

    // Displaying the initial hangman art
    printf("\033[0;31m");
    printf("\n\n\n\n\n\n\n");
    printf("                                                  _____\n");
    printf("                                                 |/    |\n");
    printf("                                                 |     |\n");
    printf("                                                 |     O\n");
    printf("                                                 |    /|\\\n");
    printf("                                                 |    / \\\n");
    printf("                                                 |\n");
    printf("                                               _/|\\__\n");
    printf("                                                  |   \\_\n");
    printf("\033[0m");
    printf("                            _       _       _       _       _       _       _   \n");
    printf("                           / \\     / \\     / \\     / \\     / \\     / \\     / \\ \n");
    printf("                          ( H )   ( A )   ( N )   ( G )   ( M )   ( A )   ( N )\n");
    printf("                           \\_/     \\_/     \\_/     \\_/     \\_/     \\_/     \\_/ \n");
    printf("\033[0m");
    sleepMilliseconds(3000); // Wait for 3 seconds

    printf("\n\n\n                          ------------------------------------------------------");

    sleepMilliseconds(1000); // Wait for 1 second

    clearConsole(); // Clear the screen
    // Displaying the rules
    printf("+------------------------------------------------+\n");
    printf("|                Hangman Rules                   |\n");
    printf("+------------------------------------------------+\n");
    printf("| 1. The game selects a secret word.             |\n");
    printf("| 2. The player tries to guess the word by       |\n");
    printf("|    suggesting letters.                         |\n");
    printf("| 3. Each correct guess reveals the positions    |\n");
    printf("|    of the guessed letter in the word.          |\n");
    printf("| 4. The player has 10 chances to guess letters. |\n");
    printf("| 5. Incorrect guesses reduce the number of      |\n");
    printf("|    remaining chances.                          |\n");
    printf("| 6. The game ends when the word is completely   |\n");
    printf("|    guessed or the player runs out of chances.  |\n");
    printf("| 7. The player wins by guessing the word before |\n");
    printf("|    running out of chances.                     |\n");
    printf("| 8. The player loses if all chances are used    |\n");
    printf("|    up before the word is guessed.              |\n");
    printf("|                                                |\n");
    printf("+------------------------------------------------+\n");
    printf("Press any key to start\n");
    waitForKeyPress(); // Wait for the user to press a key
    clearConsole(); // Clear the screen

    // Displaying the categories
    printf("                 ___________   ___________\n");
    printf("                |           | |           |\n");
    printf("                | 1. Animal | | 2. Country|\n");
    printf("                |___________| |___________|\n");
    printf("                    |             |\n");
    printf("                    |             |\n");
    printf("                 ___________   ___________\n");
    printf("                |           | |           |\n");
    printf("                | 3. People | | 4. Flower |\n");
    printf("                |___________| |___________|\n");

    printf("\n\n                 -----------------\n");
    printf("                | Choose any one: |\n");
    printf("                |    Option 1     |\n");
    printf("                |    Option 2     |\n");
    printf("                |    Option 3     |\n");
    printf("                |    Option 4     |\n");
    printf("                 -----------------\n");

    int x;
    // Prompt the user to select a category
    while (1) {
        printf("Which one You want to choose:- ");
        scanf("%d", &x);
        if (x >= 1 && x <= 4) {
            clearConsole(); // Clear the screen if a valid option is chosen
            break;
        } else {
            printf("Invalid!!! Please type between 1 to 4\n");
        }
    }

    const char* wordToGuess;

    if (x == 1) {
        wordToGuess = selectRandomWord(animals, 10);
    } else if (x == 2) {
        wordToGuess = selectRandomWord(countries, 10);
    } else if (x == 4) {
        wordToGuess = selectRandomWord(flowers, 10);
    } else {
        wordToGuess = selectRandomWord(people, 10);
    }

    int wordLength = (int)strlen(wordToGuess);
    char guessedWord[MAX_WORD_LENGTH];
    memset(guessedWord, '_', wordLength); // Initialize guessedWord with underscores
    guessedWord[wordLength] = '\0'; // Null-terminate the guessed word

    int wrongGuesses = 0;
    int correctGuesses = 0;
    char guessedLetters[MAX_WORD_LENGTH];
    memset(guessedLetters, 0, MAX_WORD_LENGTH); // Initialize guessedLetters with zeros

    printf("Word: %s\n", guessedWord);

    // Main game loop
    while (wrongGuesses < MAX_WRONG_GUESSES && correctGuesses < wordLength) {
        printf("Enter a letter: ");
        char guess;
        scanf(" %c", &guess);
        guess = tolower(guess); // Convert the guessed letter to lowercase

        if (isalpha(guess)) {
            if (!strchr(guessedLetters, guess)) { // Check if the letter was already guessed
                guessedLetters[strlen(guessedLetters)] = guess; // Add the letter to guessed letters
                if (checkGuess(wordToGuess, guess, guessedWord)) {
                    correctGuesses++;
                    printf("Correct guess!\n");
                } else {
                    wrongGuesses++;
                    printf("Wrong guess! You have %d guesses left.\n", MAX_WRONG_GUESSES - wrongGuesses);
                }
                printf("Guessed letters: %s\n", guessedLetters);
                printf("Word: %s\n", guessedWord);
            } else {
                printf("You already guessed that letter. Try again.\n");
            }
        } else {
            printf("Invalid input. Please enter a letter.\n");
        }
    }

    // Check if the player has won or lost
    if (correctGuesses == wordLength) {
        printf("Congratulations! You guessed the word: %s\n", wordToGuess);
    } else {
        // Display the hangman art if the player loses
        printf("                                                  _____\n");
        printf("                                                 |/    |\n");
        printf("                                                 |     |\n");
        printf("                                                 |     O\n");
        printf("                                                 |    /|\\\n");
        printf("                                                 |    / \\\n");
        printf("                                                 |\n");
        printf("                                               _/|\\__\n");
        printf("                                                  |   \\_\n");
        printf("Sorry, you're out of guesses. The word was: %s\n", wordToGuess);
    }

    return 0;
}
