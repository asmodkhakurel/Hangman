[Document.txt](https://github.com/asmodkhakurel/Hangman/files/6105975/Document.txt)
# Hangman
/* Hangman 
by Asmod Khakurel

This hangman is an interactive game where you will need to make guesses for the word generated randomly by the program */

#include <iostream>
#include <fstream>
#include <vector>
#include <time.h>
#include <stdlib.h>
#include <string>

using namespace std;

// Private functions needed for shuffling
void seedRandom() {
    srand(time(NULL));
}
// Random number between 0 and the number of 
// words in the file
int getRandom(int max) {
    int random = rand()%max;
    return random;
}

//the following function picks up a word at random
string pickWord(vector<string> words) {
  seedRandom();
  int len = words.size();
  return words[getRandom(len)];
}

//the following code accesses the words from words.txt and gives you the number of words the file contains
vector<string> readWords() {
  const string wordfile="words.txt";
  vector<string> words;
  string word;
  int count=0;
  fstream infile(wordfile);
  if (infile.good()) {
    while (!infile.eof()) {
      infile >> word;
      words.push_back(word);
      count++;
    }
    cout << count << " words loaded" << endl;
  } else {
    cout << "Error openning file: " << wordfile << endl;
  }
  return words;
}

bool charInString(char c, string s) {
    /* 
  c: character, the character guessed by user
  or the character in the secret word
  s: string, the guessed letters or the secret word 
  returns boolean, True if character is in the string
  False otherwise
  */
  for (int i=0; i<s.size(); i++) {
    if (s[i]==c) {
       return true;
    }
  }
  return false;
}

//isWordGuessed function is used to check if the guessed letter is in the secretWord or not. 
bool isWordGuessed(string secretWord, string lettersGuessed) {
  for (int i=0; i<secretWord.size(); i++) {
    if (!charInString(secretWord[i],lettersGuessed)) {
      return false;
    }
  }
  /*
  secretWord: string, the word the user is guessing
  lettersGuessed: vector of letters have been guessed so far
  returns: boolean, True if all the letters of secretWord are in lettersGuessed;
    False otherwise
  */
  // finish code
  return true;
}

//the following function is used to write the partially guessed word so far, as well as letters that the user has not yet guessed.
string getGuessedWord(string secretWord, string lettersGuessed) {
  string guessed="";
  for (int i=0; i<secretWord.size(); i++) {
    if (charInString(secretWord[i],lettersGuessed)) {
      guessed+=secretWord[i];
    } else {
      guessed+="_ ";
    }
  }
  /* 
      '''
    secretWord: string, the word the user is guessing
    lettersGuessed: list, what letters have been guessed so far
    returns: string, comprised of letters and underscores that represents
      what letters in secretWord have been guessed so far.
  */
  // finish code
  return guessed;
}

//This function is used to show the letters that are left to be guessed from the set of english alphabets.
string getAvailableLetters(string lettersGuessed) {
  /*
  lettersGuessed: list, what letters have been guessed so far
  returns: string, comprised of letters that represents what letters have not yet been guessed.
  */
  string avail="";
  string lower="abcdefghijklmnopqrstuvwxyz";
  for (int i=0; i<lower.size(); i++) {
    if (!charInString(lower[i],lettersGuessed)) {
      avail+=lower[i];
    }
  }
  // finish code
  return avail;
}

/*the following function checks if the letter is already used or not and if it is then it asks you again to enter a letter  */
char getGuess(string available) {
  char c;
  bool x = true;
  while (x == true){
  cout <<"Guess a letter? ";
  cin >> c ;
  if (available.find(c) == string::npos){
    cout<<"That letter is already used!"<<endl;
  }
  else { x = false;}
  }
  return c;
}


//The function main is makes the game interactive to the user letting the user know whatsoever has happened after each round.
int main() {
  vector<string> words = readWords();
  string secret = pickWord(words);
  cout << "This is hangman, you want to play? " << endl;
  cout << "I'm thinking of a word that is "<<secret.size()<<" letters long." << endl; 
  // checks if the game is finished or not
  bool done = false;
  // letters that the user has guessed so far
  string lettersGuessed="";
  // amount of guesses the user has
  int tries=8;
  // a do while loop that will continue until the game is completed
  do {
    // displays to the user the word as they have guessed so far
    cout << "Current guess: " << getGuessedWord(secret, lettersGuessed) << endl;
    // displays the letters they can use
    cout << "Available Letters: " << getAvailableLetters(lettersGuessed) << endl;
    // asks them to guess another letter
    char c=getGuess(getAvailableLetters(lettersGuessed));
    // if guess is correct, the letter is added to lettersGuessed
    if (charInString(c,secret)) {
      lettersGuessed += c;
      cout << "Good Guess!" << endl;
    }
    // if guess is wrong, user loses a try and the letter is added to lettersGuessed
    else if (!charInString(c,secret)) {
      tries -= 1;
      lettersGuessed += c;
      cout << "Sorry, " << c << " is not in the word." << endl;
      // tells the user how many more tries they can make 
      cout << "You have " << tries << " chance(s) to guess the right letters." << endl;
    }
    
    if (isWordGuessed(secret, lettersGuessed)) {
      done = true;
    }
    // if user runs out of tries, loop is completed
    else if (tries == 0) {
      done = true;
    }
   } while (!done);
   // notifies the user when they win
   if (isWordGuessed(secret, lettersGuessed)) {
     cout << "Well done! You've beat me!" << "\n" << secret << endl;
   }
  //The following conditions is to display when all the guesses are made and the right word can not be guessed by the user
   else if (tries == 0) {
     cout << "You lost! The word was: " << secret << endl;
   }
  return 0;
}

//Thank you
