import java.awt.*;
import java.awt.event.*;

public class TicTacToe extends Frame implements ActionListener {
    private Button[][] buttons;
    private char currentPlayer;
    private Label statusLabel;

    public TicTacToe() {
        setTitle("Tic Tac Toe");
        setSize(300, 300);
        setLayout(new GridLayout(3, 3));
        addWindowListener(new WindowAdapter() {
            public void windowClosing(WindowEvent windowEvent) {
                System.exit(0);
            }
        });

        buttons = new Button[3][3];
        currentPlayer = 'X';

        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                buttons[i][j] = new Button();
                buttons[i][j].addActionListener(this);
                add(buttons[i][j]);
            }
        }

        statusLabel = new Label("Player " + currentPlayer + "'s turn");
        add(statusLabel);

        setVisible(true);
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        Button clickedButton = (Button) e.getSource();

        if (!clickedButton.getLabel().isEmpty()) {
            return; // Cell already marked
        }

        clickedButton.setLabel(String.valueOf(currentPlayer));
        if (checkWinner()) {
            statusLabel.setText("Player " + currentPlayer + " wins!");
            resetGame();
            return;
        }

        currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
        statusLabel.setText("Player " + currentPlayer + "'s turn");

        boolean allCellsFilled = true;
        for (Button[] row : buttons) {
            for (Button button : row) {
                if (button.getLabel().isEmpty()) {
                    allCellsFilled = false;
                    break;
                }
            }
        }

        if (allCellsFilled) {
            statusLabel.setText("It's a draw!");
            resetGame();
        }
    }

    private boolean checkWinner() {
        // Check rows
        for (int i = 0; i < 3; i++) {
            if (!buttons[i][0].getLabel().isEmpty() && buttons[i][0].getLabel().equals(buttons[i][1].getLabel())
                    && buttons[i][0].getLabel().equals(buttons[i][2].getLabel())) {
                return true;
            }
        }
        // Check columns
        for (int i = 0; i < 3; i++) {
            if (!buttons[0][i].getLabel().isEmpty() && buttons[0][i].getLabel().equals(buttons[1][i].getLabel())
                    && buttons[0][i].getLabel().equals(buttons[2][i].getLabel())) {
                return true;
            }
        }
        // Check diagonals
        if (!buttons[0][0].getLabel().isEmpty() && buttons[0][0].getLabel().equals(buttons[1][1].getLabel())
                && buttons[0][0].getLabel().equals(buttons[2][2].getLabel())) {
            return true;
        }
        if (!buttons[0][2].getLabel().isEmpty() && buttons[0][2].getLabel().equals(buttons[1][1].getLabel())
                && buttons[0][2].getLabel().equals(buttons[2][0].getLabel())) {
            return true;
        }
        return false;
    }

    private void resetGame() {
        for (Button[] row : buttons) {
            for (Button button : row) {
                button.setLabel("");
            }
        }
        currentPlayer = 'X';
        statusLabel.setText("Player " + currentPlayer + "'s turn");
    }

    public static void main(String[] args) {
        new TicTacToe();
    }
}
