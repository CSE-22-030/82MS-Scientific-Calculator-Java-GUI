# 82MS-Scientific-Calculator-Java-GUI
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import static java.lang.Math.*;

public class ScientificCalculator82MS extends JFrame implements ActionListener {
    JTextField txt1, txt2, resultField;
    JComboBox<String> operationBox;
    JButton calculateButton, clearButton, clearHistoryButton;
    JTextArea historyArea;

    String[] operations = {
            "Add", "Subtract", "Multiply", "Divide", "Modulus",
            "Permutation & Combination", "Power", "Sin(a+b)", "Cos(a+b)", "Tan(a+b)",
            "Log(a+b)", "Log(a-b)", "Log(a*b)", "Log(a/b)", "Log(a), Log(b)", "Log base 10",
            "Factorial"
    };

    public ScientificCalculator82MS() {
        setTitle("Scientific Calculator - 82MS");
        setLayout(new BorderLayout(10, 10));

        // === Top Panel (Input + Operations) ===
        JPanel inputPanel = new JPanel(new GridLayout(7, 2, 10, 10));

        inputPanel.add(new JLabel("Enter 1st number:"));
        txt1 = new JTextField();
        inputPanel.add(txt1);

        inputPanel.add(new JLabel("Enter 2nd number:"));
        txt2 = new JTextField();
        inputPanel.add(txt2);

        inputPanel.add(new JLabel("Choose Operation:"));
        operationBox = new JComboBox<>(operations);
        inputPanel.add(operationBox);

        calculateButton = new JButton("Calculate");
        calculateButton.addActionListener(this);
        inputPanel.add(calculateButton);

        clearButton = new JButton("Clear");
        clearButton.addActionListener(this);
        inputPanel.add(clearButton);

        inputPanel.add(new JLabel("Result:"));
        resultField = new JTextField();
        resultField.setEditable(false);
        inputPanel.add(resultField);

        clearHistoryButton = new JButton("Clear History");
        clearHistoryButton.addActionListener(this);
        inputPanel.add(clearHistoryButton);

        add(inputPanel, BorderLayout.NORTH);

        // === Bottom Panel (History) ===
        historyArea = new JTextArea();
        historyArea.setEditable(false);
        JScrollPane scrollPane = new JScrollPane(historyArea);
        scrollPane.setBorder(BorderFactory.createTitledBorder("Calculation History"));
        add(scrollPane, BorderLayout.CENTER);

        setSize(600, 500);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setVisible(true);
    }

    public void actionPerformed(ActionEvent e) {
        if (e.getSource() == clearButton) {
            txt1.setText("");
            txt2.setText("");
            resultField.setText("");
            operationBox.setSelectedIndex(0);
            return;
        }

        if (e.getSource() == clearHistoryButton) {
            historyArea.setText("");
            return;
        }

        try {
            double a = Double.parseDouble(txt1.getText());
            double b = Double.parseDouble(txt2.getText());
            String selected = (String) operationBox.getSelectedItem();
            double r = 0;
            String output = "";

            switch (selected) {
                case "Add":
                    r = a + b;
                    output = a + " + " + b + " = " + r;
                    break;
                case "Subtract":
                    r = a - b;
                    output = a + " - " + b + " = " + r;
                    break;
                case "Multiply":
                    r = a * b;
                    output = a + " * " + b + " = " + r;
                    break;
                case "Divide":
                    r = a / b;
                    output = a + " / " + b + " = " + r;
                    break;
                case "Modulus":
                    r = a % b;
                    output = a + " % " + b + " = " + r;
                    break;
                case "Permutation & Combination":
                    int n = (int) a;
                    int r1 = (int) b;
                    if (n < r1) {
                        output = "Error: a must be ≥ b";
                        resultField.setText(output);
                        return;
                    }
                    long factN = factorial(n);
                    long factR = factorial(r1);
                    long factNminusR = factorial(n - r1);
                    long comb = factN / (factR * factNminusR);
                    long perm = factN / factNminusR;
                    output = "C(" + n + "," + r1 + ") = " + comb + ", P(" + n + "," + r1 + ") = " + perm;
                    resultField.setText(output);
                    historyArea.append(output + "\n");
                    return;
                case "Power":
                    r = pow(a, b);
                    output = a + " ^ " + b + " = " + r;
                    break;
                case "Sin(a+b)":
                    r = sin(toRadians(a + b));
                    output = "sin(" + (a + b) + ") = " + r;
                    break;
                case "Cos(a+b)":
                    r = cos(toRadians(a + b));
                    output = "cos(" + (a + b) + ") = " + r;
                    break;
                case "Tan(a+b)":
                    r = tan(toRadians(a + b));
                    output = "tan(" + (a + b) + ") = " + r;
                    break;
                case "Log(a+b)":
                    r = log(a + b);
                    output = "log(" + (a + b) + ") = " + r;
                    break;
                case "Log(a-b)":
                    r = log(a - b);
                    output = "log(" + (a - b) + ") = " + r;
                    break;
                case "Log(a*b)":
                    r = log(a * b);
                    output = "log(" + (a * b) + ") = " + r;
                    break;
                case "Log(a/b)":
                    r = log(a / b);
                    output = "log(" + (a / b) + ") = " + r;
                    break;
                case "Log(a), Log(b)":
                    output = "log(" + a + ") = " + log(a) + ", log(" + b + ") = " + log(b);
                    resultField.setText(output);
                    historyArea.append(output + "\n");
                    return;
                case "Log base 10":
                    r = log10(a);
                    output = "log10(" + a + ") = " + r;
                    break;
                case "Factorial":
                    int num = (int) a;
                    if (num < 0) {
                        output = "Factorial not defined for negative number.";
                    } else {
                        output = num + "! = " + factorial(num);
                    }
                    resultField.setText(output);
                    historyArea.append(output + "\n");
                    return;
            }

            resultField.setText(String.valueOf(r));
            historyArea.append(output + "\n");

        } catch (Exception ex) {
            resultField.setText("Error: " + ex.getMessage());
        }
    }

    public static long factorial(int n) {
        long fact = 1;
        for (int i = 1; i <= n; i++)
            fact *= i;
        return fact;
    }

    public static void main(String[] args) {
        new ScientificCalculator82MS();
    }
}

