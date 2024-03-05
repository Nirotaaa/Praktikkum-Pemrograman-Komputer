1. TicTacToe Game

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class TicTacToeGUI extends JFrame {
    private JButton[][] buttons;
    private char currentPlayer;
    private JLabel statusLabel;

    public TicTacToeGUI() {
        setTitle("Tic-Tac-Toe");
        setSize(300, 300);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        JPanel panel = new JPanel(new GridLayout(3, 3));
        buttons = new JButton[3][3];
        currentPlayer = 'X';

        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                JButton button = new JButton("");
                button.setFont(new Font(Font.SANS_SERIF, Font.BOLD, 40));
                final int row = i;
                final int col = j;
                button.addActionListener(new ActionListener() {
                    @Override
                    public void actionPerformed(ActionEvent e) {
                        if (buttons[row][col].getText().equals("")) {
                            buttons[row][col].setText(String.valueOf(currentPlayer));
                            if (checkWinner(String.valueOf(currentPlayer))) {
                                statusLabel.setText("Player " + currentPlayer + " wins!");
                                disableButtons();
                            } else if (isBoardFull()) {
                                statusLabel.setText("It's a tie!");
                            } else {
                                currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
                                statusLabel.setText("Player " + currentPlayer + "'s turn");
                            }
                        }
                    }
                });
                buttons[i][j] = button;
                panel.add(button);
            }
        }

        statusLabel = new JLabel("Player " + currentPlayer + "'s turn");
        statusLabel.setHorizontalAlignment(SwingConstants.CENTER);
        getContentPane().add(panel, BorderLayout.CENTER);
        getContentPane().add(statusLabel, BorderLayout.SOUTH);

        setVisible(true);
    }

    private boolean checkWinner(String player) {
        for (int i = 0; i < 3; i++) {
            if (buttons[i][0].getText().equals(player) &&
                buttons[i][1].getText().equals(player) &&
                buttons[i][2].getText().equals(player)) {
                return true; // horizontal
            }
            if (buttons[0][i].getText().equals(player) &&
                buttons[1][i].getText().equals(player) &&
                buttons[2][i].getText().equals(player)) {
                return true; // vertical
            }
        }
        if (buttons[0][0].getText().equals(player) &&
            buttons[1][1].getText().equals(player) &&
            buttons[2][2].getText().equals(player)) {
            return true; // diagonal
        }
        if (buttons[0][2].getText().equals(player) &&
            buttons[1][1].getText().equals(player) &&
            buttons[2][0].getText().equals(player)) {
            return true; // diagonal
        }
        return false;
    }

    private boolean isBoardFull() {
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (buttons[i][j].getText().equals("")) {
                    return false;
                }
            }
        }
        return true;
    }

    private void disableButtons() {
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                buttons[i][j].setEnabled(false);
            }
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                new TicTacToeGUI();
            }
        });
    }
}

2. Number Guessing Game
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.Random;

public class NumberGUessingGameGUI extends JFrame {
    private int randomNumber;
    private int attempts;
    private JLabel promptLabel;
    private JTextField guessField;
    private JButton guessButton;
    private JLabel resultLabel;

    public NumberGUessingGameGUI() {
        setTitle("Number Guessing Game");
        setSize(300, 200);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        randomNumber = generateRandomNumber();
        attempts = 0;

        promptLabel = new JLabel("I have chosen a number between 1 and 100. Try to guess it!");
        guessField = new JTextField(10);
        guessButton = new JButton("Guess");
        resultLabel = new JLabel("");

        guessButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                checkGuess();
            }
        });

        JPanel panel = new JPanel(new GridLayout(4, 1));
        panel.add(promptLabel);
        panel.add(guessField);
        panel.add(guessButton);
        panel.add(resultLabel);

        getContentPane().add(panel, BorderLayout.CENTER);

        setVisible(true);
    }

    private int generateRandomNumber() {
        Random random = new Random();
        return random.nextInt(100) + 1;
    }

    private void checkGuess() {
        String guessText = guessField.getText();
        try {
            int guess = Integer.parseInt(guessText);
            attempts++;

            if (guess < randomNumber) {
                resultLabel.setText("Too low! Try again.");
            } else if (guess > randomNumber) {
                resultLabel.setText("Too high! Try again.");
            } else {
                resultLabel.setText("Congratulations! You've guessed the number in " + attempts + " attempts.");
                guessButton.setEnabled(false);
            }
        } catch (NumberFormatException ex) {
            resultLabel.setText("Please enter a valid number.");
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                new NumberGUessingGameGUI();
            }
        });
    }
}
