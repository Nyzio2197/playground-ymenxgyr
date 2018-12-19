// This version has 2 special moves
// 1. Invert: inverts the plays on the move
// 2. Drill: clears the column the play is played into, including the play
// All other rules of connect 4 are the same


import java.util.*;

public class Connect4 {
    public static Scanner in = new Scanner(System.in);
    public static int invertChance = 1;
    public static int drillChance = 1;
    public static boolean inverted = false;

	public static void main(String[] args) {
        // prints the rules of the game
        System.out.println("\n\n\t These are the Rules:\n" + 
                           "You can win on a Special Move\n" + 
                           "You will win if you get 4 in a row\n" +
                           "BEGIN!\n");

        String[][] board = {{" ", " ", " ", " ", " ", " ", " "}, 
                            {" ", " ", " ", " ", " ", " ", " "}, 
                            {" ", " ", " ", " ", " ", " ", " "}, 
                            {" ", " ", " ", " ", " ", " ", " "}, 
                            {" ", " ", " ", " ", " ", " ", " "}, 
                            {" ", " ", " ", " ", " ", " ", " "}, };
        // ask for the mode
        System.out.print("Multiplayer or Singleplayer? ");
        String choice = in.next().substring(0, 1);
        if(choice.equalsIgnoreCase("m")) {
            System.out.println("\n");
            multiplayer(board);
        }
        else if(choice.equalsIgnoreCase("s")) {
            System.out.print("As player 1 or player 2 (type the digit)? ");
            int choice2 = in.nextInt();        
            if(choice2 == 1) {
                System.out.println("\n");
                playerOne(board);
            }
            else if(choice2 == 2) {
                System.out.println("\n");
                playerTwo(board);
            }
            else
                System.out.println("Something went wrong.\nPlease reboot this programm.");
        } else {
            System.out.println("Something went wrong.\nPlease reboot this programm.");
        }
    }

    // checks if there is a winner
    public static boolean checkForWinner(String[][] board) {
        String play = " ";
        boolean isWin = false;
        outerHorizontal:
        for(int a = 0; a < board.length; a++) {
            for(int b = 0; b < board[a].length - 4; b++) {
                play = board[a][b];
                if(play == " ")
                    continue;
                if(board[a][b + 1] == play && board[a][b + 2] == play && board[a][b + 3] == play) {
                    isWin = true;
                    break outerHorizontal;
                }
                    
            }
        }

        outerVertical:
        for(int x = 0; x < board.length - 3; x++) {
            for(int y = 0; y < board[x].length; y++) {
                play = board[x][y];
                if(play == " ")
                    continue;
                if(board[x + 1][y] == play && board[x + 2][y] == play && board[x + 3][y] == play) {
                    isWin = true;
                    break outerVertical;
                }
            }
        }

        outerLeftDiagonal:
        for(int p = 0; p < board.length - 3; p++) {
            for(int o = 0; o < board[p].length - 3; o++) {
                play = board[p][o];
                if(play == " ")
                    continue;
                if(board[p + 1][o + 1] == play && board[p + 2][o + 2] == play && board[p + 3][o + 3] == play) {
                    isWin = true;
                    break outerLeftDiagonal;
                }
            }
        }

        outerRightDiagonal:
        for(int u = 0; u < board.length - 3; u++) {
            for(int i = 3; i < board[u].length; i++) {
                play = board[u][i];
                if(play == " ")
                    continue;
                if(board[u + 1][i - 1] == play && board[u + 2][i - 2] == play && board[u + 3][i - 3] == play) {
                    isWin = true;
                    break outerRightDiagonal;
                }
            }
        }

        return isWin;
    }

    // computer player
    public static int AI(Random rand, String[][] board) {
        int play = rand.nextInt(7);
        while(board[0][play] != " ")
            play = rand.nextInt(7);
        return play + 1;
    }

    // prints out the board
    public static void printBoard(String[][] board) {
        System.out.println("  $  1  $  2  $  3  $  4  $  5  $  6  $  7  $");
        for(int n = 0; n < board.length; n++) {
            for(int x = 0; x < board[n].length; x++) {
                System.out.print("  |  " + board[n][x]);
            }
            System.out.println("  |");
            if(n < board.length - 1)
                System.out.println("  +-----+-----+-----+-----+-----+-----+-----+");
            else
                 System.out.println("  $  1  $  2  $  3  $  4  $  5  $  6  $  7  $");
        }
        // prints out the chance of a special move happening
        System.out.println("Chance of Invert Move: " + (invertChance * 2) + "%");
        System.out.println("Chance of Drill Move: " + drillChance + "%");

    }

    // updates the board to the next play
    public static String[][] updateBoard(String[][] board, int play, int player) {
        play -= 1;
        Random rand = new Random();
        String spacing = "\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t";


        // sees if the next play is a Invert move
        boolean invert = false;
        if(rand.nextInt(50) < invertChance)
            invert = true;
        boolean notFinished = true;


        // sees if the next play is a Drill move
        boolean drill = false;
        if(rand.nextInt(100) < drillChance)
            drill = true;

        
        outer:
        // the actual play itself
        for(int n = board.length - 1; n >= 0; n--) {            
            if(board[n][play] == " ") {
                if(player == 1) {
                    board[n][play] = "O";
                    notFinished = false;
                } else {
                    board[n][play] = "X";
                    notFinished = false;
                }
            }
            // inverts all of the plays on the board
            if(invert) {
                System.out.println("\n" + spacing + "**********************\n" +
                                   spacing + "INVERT MOVE!!!\n" +
                                   spacing + "EVERYTHING IS INVERTED\n" +
                                   spacing + "**********************\n");
                for(int a = 0; a < board.length; a++) {
                    for(int b = 0; b < board[a].length; b++) {
                        if(board[a][b] == "X")
                            board[a][b] = "O";
                        else if(board[a][b] == "O")
                            board[a][b] = "X";
                    }
                }
                notFinished = false;
                invertChance = 0;
                inverted = !inverted;
            }
            // clears column
            if(drill) {
                System.out.println("\n" + spacing + "**************\n" +
                                   spacing + "DRILL MOVE!!!\n" +
                                   spacing + "COLUMN CLEARED\n" +
                                   spacing + "**************\n");
               for(int f = 0; f < board.length; f++) {
                   board[f][play] = " ";
               }
               notFinished = false;
               drillChance = 0;
            }
            if(!notFinished){
                invertChance++;
                drillChance++;
                break outer;
            }
        }
        // in case the column the player chose is filled
        if(notFinished) {
            System.out.println("INVALID! Please try again.");
            updateBoard(board, askForPlay(in), player);
        }
        return board;
    }

    // gets the next play
    public static int askForPlay(Scanner in) {
        System.out.print("Which column would you like to place into? ");
        int play = Integer.parseInt(in.next().substring(0, 1));
        while(play < 1 || play > 7) {
            System.out.print("INVALID! Retry:\nWhich column would you like to place into? ");
            play = Integer.parseInt(in.next().substring(0, 1));
        }
        return play;
    }

    // multiplayer mode
    public static void multiplayer(String[][] board) {
        int player = 1;
        while(true) {
            // player 1
            printBoard(board);
            System.out.println("\nPlayer 1:");
            board = updateBoard(board, askForPlay(in), player);            
            if(winPlay(board, "Player 1", "Player 2"))
                break;
            player = 2;

            // player 2
            printBoard(board);
            System.out.println("\nPlayer 2:");
            board = updateBoard(board, askForPlay(in), player);            
            if(winPlay(board, "Player 2", "Player 1"))
                break;
            player = 1;
        }
    }

    // singleplayer as player 1
    public static void playerOne(String[][] board) {
        int player = 1;
        while(true) {
            // player 1
            printBoard(board);
            System.out.println("\nPlayer 1:");
            board = updateBoard(board, askForPlay(in), player);            
            if(winPlay(board, "Player 1", "AI"))
                break;
            player = 2;

            // player 2
            printBoard(board);
            System.out.println("\nAI:");
            board = updateBoard(board, AI(new Random(), board), player);            
            if(winPlay(board, "AI", "Player 1"))
                break;
            player = 1;
        }
    }

    // singleplayer as player 2
    public static void playerTwo(String[][] board) {
        int player = 1;
        while(true) {
            // player 1
            printBoard(board);
            System.out.println("\nAI:");
            board = updateBoard(board, AI(new Random(), board), player);            
            if(winPlay(board, "AI", "Player 2"))
                break;
            player = 2;

            // player 2
            printBoard(board);
            System.out.println("\nPlayer 2:");
            board = updateBoard(board, askForPlay(in), player);            
            if(winPlay(board, "Player 2", "AI"))
                break;
            player = 1;
        }
    }

    // prints out the winning play
    public static boolean winPlay(String[][] board, String player, String otherPlayer) {
        if(checkForWinner(board)) {
                printBoard(board);
                if(!inverted)
                    System.out.println(player + " won!");
                else
                    System.out.println(otherPlayer + " won!");
        }
        return(checkForWinner(board));
    }

}

