import java.util.*;

public class Connect4 {
    public static Scanner in = new Scanner(System.in);
    public static int chance = 1;
    public static boolean normal = true;
    public static boolean inverted = false;

	public static void main(String[] args) {
        System.out.println("\n\n\t These are the Rules:\n" + 
                           "You cannot win on a Special Move\n" + 
                           "You will win if you get 4 in a row\n" +
                           "BEGIN!\n");

        String[][] board = {{" ", " ", " ", " ", " ", " ", " "}, 
                            {" ", " ", " ", " ", " ", " ", " "}, 
                            {" ", " ", " ", " ", " ", " ", " "}, 
                            {" ", " ", " ", " ", " ", " ", " "}, 
                            {" ", " ", " ", " ", " ", " ", " "}, 
                            {" ", " ", " ", " ", " ", " ", " "}, };
    
        System.out.print("Multiplayer or Singleplayer? ");
        String choice = in.next().substring(0, 1);
        if(choice.equalsIgnoreCase("m"))
            multiplayer(board);
        else if(choice.equalsIgnoreCase("s")) {
            System.out.print("As player 1 or player 2 (type the digit)? ");
            int choice2 = in.nextInt();        
            if(choice2 == 1)
                playerOne(board);
            else if(choice2 == 2)
                playerTwo(board);
            else
                System.out.println("Something went wrong.\nPlease reboot this programm.");
        } else {
            System.out.println("Something went wrong.\nPlease reboot this programm.");
        }
    }

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

    public static int AI(Random rand, String[][] board) {
        int play = rand.nextInt(7);
        while(board[0][play] != " ")
            play = rand.nextInt(7);
        return play + 1;
    }

    public static void printBoard(String[][] board) {
        System.out.println(" + 1 + 2 + 3 + 4 + 5 + 6 + 7 +");
        for(int n = 0; n < board.length; n++) {
            for(int x = 0; x < board[n].length; x++) {
                System.out.print(" | " + board[n][x]);
            }
            System.out.println(" |");
            System.out.println(" +---+---+---+---+---+---+---+");
        }
        System.out.println("Chance of Special Move: " + (chance * 2) + "%");
    }

    public static String[][] updateBoard(String[][] board, int play, int player) {
        play -= 1;


        Random rand = new Random();
        boolean isWhite = false;
        if(rand.nextInt(50) < chance)
            isWhite = true;
        boolean notFinished = true;
        boolean whiteNotHappened = true;


        outer:
        for(int n = board.length - 1; n > 0; n--) {            
            if(board[n][play] == " ") {
                if(player == 1) {
                    board[n][play] = "O";
                    notFinished = false;
                } else {
                    board[n][play] = "X";
                    notFinished = false;
                }
            }
            if(whiteNotHappened) {
                if(isWhite) {
                    System.out.println("\n**********************\n" +
                                       "SPECIAL MOVE!!!\n" +
                                       "EVERYTHING IS INVERTED\n" +
                                       "**********************\n");
                    for(int a = 0; a < board.length; a++) {
                        for(int b = 0; b < board[a].length; b++) {
                            if(board[a][b] == "X")
                                board[a][b] = "O";
                            else if(board[a][b] == "O")
                                board[a][b] = "X";
                        }
                    }
                    notFinished = false;
                    whiteNotHappened = false;
                    chance = 0;
                    if(normal) {
                        normal = false;
                        inverted = true;
                    } else {
                        normal = true;
                        inverted = false;
                    }
                }
            
            }
            if(!notFinished){
                chance++;
                break outer;
            }
        }
        if(notFinished) {
            System.out.println("That is a invalid move. Please try again.");
            updateBoard(board, askForPlay(in), player);
        }
        return board;
    }

    public static int askForPlay(Scanner in) {
        System.out.print("Which column would you like to place into? ");
        return Integer.parseInt(in.next().substring(0, 1));
    }

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

    public static boolean winPlay(String[][] board, String player, String otherPlayer) {
        if(checkForWinner(board)) {
                printBoard(board);
                if(normal)
                    System.out.println(player + " won!");
                else
                    System.out.println(otherPlayer + " won!");
        }
        return(checkForWinner(board));
    }

}

