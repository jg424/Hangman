# Hangman
Python code created intended to play a game of hangman against the computer


# Problem Set 2, hangman.py
# Name: Jason Goldsmith
# Collaborators:N/A


# Hangman Game
# -----------------------------------
# Helper code

import random
import string

WORDLIST_FILENAME = "words.txt"


def load_words():
    """
    Returns a list of valid words. Words are strings of lowercase letters.
    
    Depending on the size of the word list, this function may
    take a while to finish.
    """
    print("Loading word list from file...")
    # inFile: file
    inFile = open(WORDLIST_FILENAME, 'r')
    # line: string
    line = inFile.readline()
    # wordlist: list of strings
    wordlist = line.split()
    print("  ", len(wordlist), "words loaded.")
    return wordlist



def choose_word(wordlist):
    """
    wordlist (list): list of words (strings)
    
    Returns a word from wordlist at random
    """
    return random.choice(wordlist)

# end of helper code

# -----------------------------------

# Load the list of words into the variable wordlist
# so that it can be accessed from anywhere in the program
wordlist = load_words()
secret_word = choose_word(wordlist)
    


def is_word_guessed(secret_word, letters_guessed):
    '''
    secret_word: string, the word the user is guessing; assumes all letters are
      lowercase
    letters_guessed: list (of letters), which letters have been guessed so far;
      assumes that all letters are lowercase
    returns: boolean, True if all the letters of secret_word are in letters_guessed;
      False otherwise
    '''
    test=[]
    for sec in secret_word:
        if sec in letters_guessed:
            test.append(1)
        else:
            test.append(0)
        if sum(test)==len(secret_word):
            return True
        else:
            return False
     
        
    

def get_guessed_word(secret_word, letters_guessed):
    '''
    secret_word: string, the word the user is guessing
    letters_guessed: list (of letters), which letters have been guessed so far
    returns: string, comprised of letters, underscores (_), and spaces that represents
      which letters in secret_word have been guessed so far.
    '''
    l=['_ ']*len(secret_word)
    for i in range(len(secret_word)):
        for let in letters_guessed:
            if secret_word[i]==let:
                l[i]=let
    return ''.join(l)
            
            




def get_available_letters(letters_guessed):
    '''
    letters_guessed: list (of letters), which letters have been guessed so far
    returns: string (of letters), comprised of letters that represents which letters have not
      yet been guessed.
    '''
    al=string.ascii_lowercase
    al=list(al)
    for j in range(len(al)):
        for let in letters_guessed:
            if al[j]==let:
               al[j]=''
    al=''.join(al)
    return al


def hangman(secret_word):
    '''
    secret_word: string, the secret word to guess.
    Starts up an interactive game of Hangman.
    * At the start of the game, let the user know how many
      letters the secret_word contains and how many guesses s/he starts with.
    * The user should start with 6 guesses
    * Before each round, you should display to the user how many guesses
      s/he has left and the letters that the user has not yet guessed.
    * Ask the user to supply one guess per round. Remember to make
      sure that the user puts in a letter!
    * The user should receive feedback immediately after each guess
      about whether their guess appears in the computer's word.
    * After each guess, you should display to the user the
      partially guessed word so far.
    Follows the other limitations detailed in the problem write-up.
    '''

    guesses_remaining = 6
    warnings_remaining = 3
    letters_guessed = []
    vowels = ['a', 'e', 'i', 'o', 'u']
    consonants = list('bcdfghjklmnpqrstvwxyz')
    print('Welcome to the game Hangman!')
    print('I am thinking of a word that is', len(secret_word), 'letters long.')
    print('You have', warnings_remaining, 'warnings left.')
    print('-------------')
    print('You have', guesses_remaining, 'guesses left.')
    print('Available letters: abcdefghijklmnopqrstuvwxyz')
    # entered_letter = str.lower(input('Please guess a letter: '))

    while True:
        entered_letter = str.lower(input('Please guess a letter: '))
        if len(entered_letter) > 1:
            print('That is not a letter.')
            continue
        # ENTER THE SAME LETTER WITH WARNING REMAINS
        if (entered_letter in letters_guessed) and warnings_remaining >= 1:
            letters_guessed.append(entered_letter)
            warnings_remaining = warnings_remaining - 1
            print('Oops! You\'ve already guessed that letter. You have', warnings_remaining, 'warnings left:',
                  get_guessed_word(secret_word, letters_guessed))
            print('-------------')
            print('You have', guesses_remaining, 'guesses left.')
            print('Available letters:', get_available_letters(letters_guessed))

            # ENTER SYMBOLS & NUMBERS INSTEAD OF LETTERS WITH WARNING REMAINS
        elif not str.isalpha(entered_letter) and warnings_remaining >= 1:
            letters_guessed.append(entered_letter)
            warnings_remaining = warnings_remaining - 1
            print('Oops! That is not a valid letter. You have', warnings_remaining, 'warnings left:',
                  get_guessed_word(secret_word, letters_guessed))
            print('-------------')
            print('You have', guesses_remaining, 'guesses left.')
            print('Available letters:', get_available_letters(letters_guessed))

            # OUT OF WARNINGS
        elif not str.isalpha(entered_letter) and warnings_remaining == 0:
            letters_guessed.append(entered_letter)
            guesses_remaining = guesses_remaining - 1
            print('Oops! That is not a valid letter:', get_guessed_word(secret_word, letters_guessed))
            print('-------------')
            print('You have', guesses_remaining, 'guesses left.')
            print('Available letters:', get_available_letters(letters_guessed))
            if guesses_remaining == 0:
                print('-------------')
                print('Sorry, you ran out of guesses. The word was', secret_word)
                break
        elif (entered_letter not in letters_guessed) and warnings_remaining == 0:
            letters_guessed.append(entered_letter)
            guesses_remaining = guesses_remaining - 1
            print('Oops! You\'ve already guessed that letter:', get_guessed_word(secret_word, letters_guessed))
            print('-------------')
            print('You have', guesses_remaining, 'guesses left.')
            print('Available letters:', get_available_letters(letters_guessed))
            if guesses_remaining == 0:
                print('-------------')
                print('Sorry, you ran out of guesses. The word was', secret_word)
                break

                # ENTER THE RIGHT LETTER
        elif (entered_letter in secret_word) and not (entered_letter in letters_guessed):
            letters_guessed.append(entered_letter)
            print('Good guess:', get_guessed_word(secret_word, letters_guessed))
            print('-------------')
            print('You have', guesses_remaining, 'guesses left.')
            print('Available letters:', get_available_letters(letters_guessed))

            # ENTER THE WRONG LETTER
        elif (entered_letter not in secret_word) and str.isalpha(entered_letter) and (
                    entered_letter not in letters_guessed):
            guesses_remaining = guesses_remaining - 1 if entered_letter in consonants else guesses_remaining - 2

            letters_guessed.append(entered_letter)
            print('Oops! That letter is not in my word:', get_guessed_word(secret_word, letters_guessed))

            if guesses_remaining > 0:
                print('-------------')
                print('You have', guesses_remaining, 'guesses left.')
                print('Available letters:', get_available_letters(letters_guessed))
            else:
                print('-------------')
                print('Sorry, you ran out of guesses. The word was', secret_word)
                break

        # ALL WORDS HAVE BEEN GUESSED
        if is_word_guessed(secret_word, letters_guessed):
            print('-------------')
            print('Congratulations, you won!')
            print('Your total score for this game is:', guesses_remaining * len(secret_word))
            break
