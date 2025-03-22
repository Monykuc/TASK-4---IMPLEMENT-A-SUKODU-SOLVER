public class SudokuSolver {

    // Size of the Sudoku grid (9x9)
    static final int SIZE = 9;

    public static void main(String[] args) {
        // Example unsolved Sudoku puzzle (0 means empty cell)
        int[][] sudoku = {
            {5, 3, 0, 0, 7, 0, 0, 0, 0},
            {6, 0, 0, 1, 9, 5, 0, 0, 0},
            {0, 9, 8, 0, 0, 0, 0, 6, 0},
            {8, 0, 0, 0, 6, 0, 0, 0, 3},
            {4, 0, 0, 8, 0, 3, 0, 0, 1},
            {7, 0, 0, 0, 2, 0, 0, 0, 6},
            {0, 6, 0, 0, 0, 0, 2, 8, 0},
            {0, 0, 0, 4, 1, 9, 0, 0, 5},
            {0, 0, 0, 0, 8, 0, 0, 7, 9}
        };

        if (solveSudoku(sudoku)) {
            System.out.println("Sudoku puzzle solved!");
            printSudoku(sudoku);
        } else {
            System.out.println("No solution exists.");
        }
    }

    // Function to solve the Sudoku puzzle
    public static boolean solveSudoku(int[][] board) {
        for (int row = 0; row < SIZE; row++) {
            for (int col = 0; col < SIZE; col++) {
                // If the cell is empty (contains 0), try to solve it
                if (board[row][col] == 0) {
                    // Try every number from 1 to 9
                    for (int num = 1; num <= SIZE; num++) {
                        // If the number is valid in the current position
                        if (isValid(board, row, col, num)) {
                            board[row][col] = num;

                            // Recursively attempt to solve the rest of the board
                            if (solveSudoku(board)) {
                                return true;
                            }

                            // If no solution is found, backtrack
                            board[row][col] = 0;
                        }
                    }
                    return false; // If no number is valid, return false
                }
            }
        }
        return true; // All cells are filled correctly
    }

    // Function to check if placing 'num' in board[row][col] is valid
    public static boolean isValid(int[][] board, int row, int col, int num) {
        // Check if the number is already in the row or column
        for (int i = 0; i < SIZE; i++) {
            if (board[row][i] == num || board[i][col] == num) {
                return false;
            }
        }

        // Check the 3x3 subgrid
        int startRow = row - row % 3;
        int startCol = col - col % 3;
        for (int i = startRow; i < startRow + 3; i++) {
            for (int j = startCol; j < startCol + 3; j++) {
                if (board[i][j] == num) {
                    return false;
                }
            }
        }

        return true;
    }

    // Function to print the Sudoku grid
    public static void printSudoku(int[][] board) {
        for (int row = 0; row < SIZE; row++) {
            for (int col = 0; col < SIZE; col++) {
                System.out.print(board[row][col] + " ");
            }
            System.out.println();
        }
    }
}
