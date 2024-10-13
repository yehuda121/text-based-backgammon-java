
import java.util.Arrays;
import java.util.Random;
import java.util.Scanner;

public class Backgammon {
    //Fields
    private int[] board; // Positive numbers - white pieces, negative - black pieces.
    private int[] eaten; // 2 cells - 1st cell for whites eaten pieces, 2nd for blacks.
    private int[] cubesUsages; // How many more usages do we have for each cube
    private boolean whitesTurn; // Is it whites turn?
    private Random rd = new Random(); // Random generator.
    private Scanner sc = new Scanner(System.in); // For getting the users position and cube to use.
    private boolean whiteStart;

    /**
     * Default contractor
     */
    Backgammon() {
        initBoard();
    }

    /**
     * a function that creates a new size board
     * @param board_size .
     */
    Backgammon(int board_size) {
        this.board=new int[board_size];
    }


    @SuppressWarnings("unused")
    public void initBoard() { // useful when playing more than 1 game.
        initBoard(24);
    }



    /**
     * Contractor that initialize the fields and the board
     * @param boardSize .
     */
    public void initBoard(int boardSize)
    {
        this.board=new int[boardSize];
        this.eaten=new int[2];
        this.cubesUsages=new int[4];
        this.whitesTurn=rd.nextBoolean();
        this.whiteStart=this.whitesTurn;

        this.board[0]=2;
        this.board[5]=-5;
        this.board[7]=-3;
        this.board[11]=5;
        this.board[12]=-5;
        this.board[16]=3;
        this.board[18]=5;
        this.board[23]=-2;
    }


    /**
     * a function that return a string that presents the board
     * @return String
     */
    public String toString() {
        int[] partOne=Arrays.copyOfRange(this.board,0, (this.board.length/4));
        int[] partTwo=Arrays.copyOfRange(this.board,(this.board.length/4), (this.board.length/4)*2);
        int[] partThree=Arrays.copyOfRange(this.board,(this.board.length/4)*2, (this.board.length/4)*3);
        int[] reversPartThree = new int[this.board.length/4];
        int[] reversPartFour = new int[this.board.length/4];

        for (int i = 0; i < this.board.length/4; i++) {
            reversPartThree[i]=partThree[partThree.length-1-i];
            reversPartFour[i]=this.board[(this.board.length-1)-i];
        }

        return Arrays.toString(partOne)+Arrays.toString(partTwo)+'\n'+ Arrays.toString(reversPartFour)+
                Arrays.toString(reversPartThree)+'\n'+"Whites eaten - "+this.eaten[0]+", blacks eaten - "+this.eaten[1];
    }


    /**
     * a function that returns an array that presents this board
     * @return board
     */
    public int[] getBoard() {
        return this.board;
    }


    /**
     * a function that returns if its white turn or not
     * @return whitsTurn
     */
    public boolean getWhitesTurn() {
        return this.whitesTurn;
    }


    /**
     * a function that set the any board given to be as this board
     * @param board .
     */
    public void setBoard(int[] board)
    {
        // Use with caution, should be used only for debugging purposes!
        for (int i = 0; i < board.length; i++)
            this.board[i] = board[i];
    }


    /**
     * a function return if the whites has started the game
     * @return true\false
     */
    public boolean whiteStarts() {
        return this.whiteStart;
    }


    /**
     * a function that returns an array valued randomly in range 1-6 like cubes
     * @return cubes
     */
    public int[] roll2Cubes() {
        int cube1,cube2;
        cube1=(int)(Math.random()*(this.board.length/4))+1;
        cube2=(int)(Math.random()*(this.board.length/4))+1;

        return new int[]{cube1, cube2};
    }


    /**
     * a function that checks if the game is over or not
     * @return true\false
     */
    public boolean gameOver() {
        //Variables
        boolean white=true,black=true;

        //checks if there's white or blacks stones
        for (int k : this.board) {
            if (k > 0) {
                white = false;
                break;
            }
        }
        for (int j : this.board)
            if (j < 0) {
                black = false;
                break;
            }
        return white || black;
    }


    /**
     * a function that making the move
     * @param position .
     * @param move .
     * @return true/false
     */
    public boolean move(int position, int move) {
        int direction = this.whitesTurn ? 1 : -1;
        int isWhiteTurn = this.whitesTurn ? 1 : 0;


        //double check for legal move
        if(!legalMove(position, move) || !haveLegalMoves(this.cubesUsages))
            return false;

        //white turn
        if (this.whitesTurn)
        {
            //if the player choose illegal position
            if(this.eaten[0]==0 && this.board[position]<=0)
                return false;

            //if the white as an eaten stone
            if (this.eaten[0]>0)
            {
                if(position>=(this.board.length/4))
                    return false;

                //if the white eat the black
                if (this.board[move-1]==-1)
                {
                    this.eaten[1]++;
                    this.board[move-1]++;
                }

                //the move
                this.eaten[0]--;
                this.board[move-1]++;
                return true;
            }
            //if the whites taking out stones
            if (farthestStoneInLastQuadrant(farthestStone()))
            {
                if (this.board[move-1]>0)
                    this.board[move-1]--;
                return true;
            }
        }
        //blacks turn
        else
        {
            //if the player choose illegal position
            if(this.board[position]>=0 && this.eaten[1]==0)
                return false;

            //if the black as an eaten stone
            if (this.eaten[1]>0)
            {
                if(position<(this.board.length/4)*3)
                    return false;

                //if the black eat the white
                if (this.board[this.board.length - move]==1)
                {
                    this.eaten[0]++;
                    this.board[this.board.length -move]--;
                }

                //the move
                this.eaten[1]--;
                this.board[this.board.length - move]--;
                return true;
            }
            //if the black taking out stones
            if (farthestStoneInLastQuadrant(farthestStone()) && this.board[this.board.length-move]<0)
            {
                this.board[this.board.length-move]++;
                return true;
            }
        }


        //Special cases
        if(outOfBoard(position) || outOfBoard( position + (direction * move)))
            return false;

        if(this.board[position+(direction*move)]==-direction)
        {
            this.eaten[isWhiteTurn]++;
            this.board[position]-=direction;
            this.board[position+(direction*move)]=direction;
            return true;
        }

        //move
        this.board[position+(direction*move)]+=direction;
        this.board[position]-=direction;
        return true;

    }


    /**
     * a function that checks if there is a legal move
     * @param cubes .
     * @return true\false
     */
    public boolean haveLegalMoves(int[] cubes) {

        //checking for legal moves
        for(int i = 0; i <this.board.length; i++)
        {
            if(this.board[i]!=0)
                if(legalMove(i, cubes[0]) || legalMove(i, cubes[1]))
                    return true;
        }
        return false;
    }


    /**
     * a function that checks if the move is legal
     * @param startPosition .
     * @param move .
     * @return true/false
     */
    public boolean legalMove(int startPosition, int move) {

        //in case someone as an eaten stones or out of board
        if (this.whitesTurn)
        {
            if(outOfBoard(startPosition+move) || this.board[startPosition+move]<-1)
                return false;
            if (this.eaten[0] > 0 && this.board[move-1] <-1)
                return false;
        }
        else {
            if (outOfBoard(startPosition-move) || this.board[startPosition-move]>1)
                return false;
            if (this.eaten[1] > 0 && this.board[this.board.length - move] >1)
                return false;
        }
        return true;
    }


    /**
     * a function that checks what is the farest stone on both players
     * @return index of the far stone
     */
    public int farthestStone() {
        int indexOfWhiteFarthestStone=0, indexOfBlackFarthestStone=0;

        //find the index of the white farest stone
        for (int i = 0; i < this.board.length; i++) {
            if(this.board[i]>0) {
                indexOfWhiteFarthestStone=i;
                break;
            }
        }
        //find the index of the black farest stone
        for (int i = this.board.length-1; i >= 0; i--) {
            if(this.board[i]<0) {
                indexOfBlackFarthestStone=i;
                break;
            }

        }
        if(this.whitesTurn)
            return indexOfWhiteFarthestStone;
        else
            return indexOfBlackFarthestStone;
    }


    /**
     * a function that check if the far stone in the last quadrant
     * @param farthestStone .
     * @return true/false
     */
    public boolean farthestStoneInLastQuadrant(int farthestStone) {
        return (!this.whitesTurn && farthestStone < (this.board.length/4)) ||
                (this.whitesTurn && farthestStone >= (this.board.length/4)*3);
    }


    /**
     * a function that checks if the number is within the board
     * @param position .
     * @return true\false
     */
    public boolean outOfBoard(int position) {
        return position >=this.board.length || position < 0;
    }


    /**
     * a function that changes the turns
     */
    public void nextTurn() {
        this.whitesTurn= !this.whitesTurn;
    }


    public void runGame(Backgammon bg) {
        while (!this.gameOver()) {
            int[] cubes = this.roll2Cubes();
            this.cubesUsages[1] = this.cubesUsages[0] = (cubes[0] == cubes[1]) ? 2 : 1;

            // Move the board using legal moves rolled by the cubes:
            while (this.cubesUsages[0] > 0 || this.cubesUsages[1] > 0) {
                // System.out.print("\033[H\033[2J");  
                // System.out.flush();  
                System.out.println(bg);
                if (!this.haveLegalMoves(cubes))
                    break;

                System.out.print(this.whitesTurn ? "Whites turn (⇄)" : "Blacks turn (⇆)");
                System.out.println(", Rolled " + cubes[0] + " " + cubes[1]);
                System.out.print("Insert position number: ");
                int choosenPosition = this.sc.nextInt();
                System.out.print("Insert cube number (0 or 1): ");
                int cubeToUse = this.sc.nextInt();
                if (cubeToUse < 0 || cubeToUse >= cubes.length) {
                    System.out.println("Please select a cube from the range of 0 to " + (cubes.length - 1));
                    continue;
                }
                if (this.cubesUsages[cubeToUse] <= 0) {
                    System.out.println("Can\'t use this cube again!");
                    continue;
                }
                int choosenMove = cubes[cubeToUse];
                boolean moved = this.move(choosenPosition, choosenMove);
                if (moved) {
                    this.cubesUsages[cubeToUse] -= 1;
                    System.out.println(this);
                } else
                    System.out.println("Illegal move!");
            }
            this.nextTurn();
        }
        System.out.println(this.whitesTurn ? "Black won!" : "White won!");
    }

    public static void main(String[] args) {
        Backgammon bg = new Backgammon();
        bg.runGame(bg);
    }
}
