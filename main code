import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.FlowLayout;
import java.awt.GridLayout;
import java.awt.Toolkit;
import java.awt.datatransfer.Clipboard;
import java.awt.datatransfer.StringSelection;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.HashMap;
import java.util.Map;

import javax.swing.ImageIcon;
import javax.swing.JButton;
import javax.swing.JCheckBox;
import javax.swing.JComboBox;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTabbedPane;
import javax.swing.JTextArea;
import javax.swing.JTextField;
import javax.swing.SwingUtilities;

public class MultiHashGeneratorAndCrackerGUI {
    private JFrame frame;
    private JTabbedPane tabbedPane;
    private JPanel hashGeneratorPanel;
    private JPanel passwordCrackerPanel;
    private JTextField inputDataField;
    private JCheckBox darkModeCheckBox;
    private JTextField targetHashField;
    private JTextArea dictionaryArea;
    private JButton generateButton;
    private JButton crackButton;
    private JComboBox<String> hashAlgorithmComboBox;
    private Map<String, JTextArea> hashTextAreas;
    private Map<String, JButton> copyButtons;

    // Define supported hash algorithms for hash generation and password cracking
    private final String[] supportedAlgorithms = {"MD5", "SHA-1"};
    private final String[] allAlgorithms = {"MD5", "SHA-1", "SHA-256", "SHA-384", "SHA-512"};

    public MultiHashGeneratorAndCrackerGUI() {
        frame = new JFrame("Multi-Hash Generator and Password Cracker");

        // Load the custom icon image (replace with your image path)
        final ImageIcon icon = new ImageIcon("custom_icon.png"); //use your desired icon by replacing custom_icon.png with your icon's path.
        frame.setIconImage(icon.getImage());

        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(800, 600);
        frame.setLayout(new BorderLayout());

        tabbedPane = new JTabbedPane();

        // Hash Generator Panel
        hashGeneratorPanel = new JPanel();
        hashGeneratorPanel.setLayout(new BorderLayout());

        JPanel inputPanel = new JPanel();
        inputPanel.setLayout(new FlowLayout());

        JLabel dataLabel = new JLabel("Enter Data: ");
        inputDataField = new JTextField(30);
        inputPanel.add(dataLabel);
        inputPanel.add(inputDataField);

        generateButton = new JButton("Generate Hashes");
        inputPanel.add(generateButton);

        darkModeCheckBox = new JCheckBox("Dark Mode");
        inputPanel.add(darkModeCheckBox);

        hashGeneratorPanel.add(inputPanel, BorderLayout.NORTH);

        JPanel hashPanel = new JPanel();
        hashPanel.setLayout(new GridLayout(allAlgorithms.length, 1)); // Display all supported algorithms for hash generation
        hashTextAreas = new HashMap<>();
        copyButtons = new HashMap<>();

        for (String algorithm : allAlgorithms) {
            JTextArea textArea = new JTextArea();
            textArea.setEditable(false);
            hashTextAreas.put(algorithm, textArea);

            JButton copyButton = new JButton("Copy");
            copyButton.addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    copyHashToClipboard(algorithm);
                }
            });
            copyButtons.put(algorithm, copyButton);

            JPanel hashRowPanel = new JPanel();
            hashRowPanel.setLayout(new BorderLayout());
            hashRowPanel.add(textArea, BorderLayout.CENTER);
            hashRowPanel.add(copyButton, BorderLayout.EAST);

            hashPanel.add(hashRowPanel);
        }

        hashGeneratorPanel.add(hashPanel, BorderLayout.CENTER);

        generateButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                generateHashes();
            }
        });

        darkModeCheckBox.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                toggleDarkMode();
            }
        });

        // Password Cracker Panel
        passwordCrackerPanel = new JPanel();
        passwordCrackerPanel.setLayout(new BorderLayout());

        JPanel crackerInputPanel = new JPanel();
        crackerInputPanel.setLayout(new GridLayout(4, 1)); // Increase rows to accommodate hash algorithm selection

        // Add a combo box to select the hash algorithm
        JLabel algorithmLabel = new JLabel("Select Hash Algorithm:");
        hashAlgorithmComboBox = new JComboBox<>(supportedAlgorithms);
        crackerInputPanel.add(algorithmLabel);
        crackerInputPanel.add(hashAlgorithmComboBox);

        JLabel targetHashLabel = new JLabel("Target Hash:");
        targetHashField = new JTextField(64);
        crackerInputPanel.add(targetHashLabel);
        crackerInputPanel.add(targetHashField);

        JLabel dictionaryLabel = new JLabel("Dictionary File:");
        dictionaryArea = new JTextArea(5, 20);
        JScrollPane dictionaryScrollPane = new JScrollPane(dictionaryArea);
        crackerInputPanel.add(dictionaryLabel);
        crackerInputPanel.add(dictionaryScrollPane);

        crackButton = new JButton("Crack Password");
        crackerInputPanel.add(crackButton);

        passwordCrackerPanel.add(crackerInputPanel, BorderLayout.NORTH);

        crackButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                crackPassword();
            }
        });

        tabbedPane.addTab("Hash Generator", hashGeneratorPanel);
        tabbedPane.addTab("Password Cracker", passwordCrackerPanel);

        frame.add(tabbedPane, BorderLayout.CENTER);
    }

    private void generateHashes() {
        String inputData = inputDataField.getText();
        for (String algorithm : allAlgorithms) { // Generate hashes for all supported algorithms
            try {
                MessageDigest md = MessageDigest.getInstance(algorithm);
                byte[] hashBytes = md.digest(inputData.getBytes());

                StringBuilder hexString = new StringBuilder();
                for (byte b : hashBytes) {
                    String hex = String.format("%02x", b);
                    hexString.append(hex);
                }

                JTextArea textArea = hashTextAreas.get(algorithm);
                textArea.setText("Hash (" + algorithm + "):\n" + hexString.toString());

            } catch (NoSuchAlgorithmException e) {
                JOptionPane.showMessageDialog(frame, "Error: " + e.getMessage(), "Error", JOptionPane.ERROR_MESSAGE);
            }
        }
    }

    private void toggleDarkMode() {
        boolean darkModeEnabled = darkModeCheckBox.isSelected();
        Color backgroundColor = darkModeEnabled ? Color.BLACK : Color.WHITE;
        Color textColor = darkModeEnabled ? Color.WHITE : Color.BLACK;

        // Apply dark mode to Hash Generator panel
        hashGeneratorPanel.setBackground(backgroundColor);
        for (JTextArea textArea : hashTextAreas.values()) {
            textArea.setBackground(backgroundColor);
            textArea.setForeground(textColor);
        }

        // Apply dark mode to Password Cracker panel
        passwordCrackerPanel.setBackground(backgroundColor);
        targetHashField.setBackground(backgroundColor);
        targetHashField.setForeground(textColor);
        dictionaryArea.setBackground(backgroundColor);
        dictionaryArea.setForeground(textColor);
        crackButton.setBackground(backgroundColor);
        crackButton.setForeground(textColor);

        frame.getContentPane().setBackground(backgroundColor);
        frame.repaint(); // Repaint the frame to apply the changes
    }

    private void copyHashToClipboard(String algorithm) {
        String selectedHash = hashTextAreas.get(algorithm).getText();
        StringSelection stringSelection = new StringSelection(selectedHash);
        Clipboard clipboard = Toolkit.getDefaultToolkit().getSystemClipboard();
        clipboard.setContents(stringSelection, null);
        JOptionPane.showMessageDialog(frame, "Copied to clipboard:\n" + selectedHash);
    }

    private void crackPassword() {
        String targetHash = targetHashField.getText();
        String dictionaryFilePath = dictionaryArea.getText();
        String selectedAlgorithm = (String) hashAlgorithmComboBox.getSelectedItem();

        try (BufferedReader br = new BufferedReader(new FileReader(dictionaryFilePath))) {
            String line;
            while ((line = br.readLine()) != null) {
                line = line.trim();
                String crackedHash = hash(line, selectedAlgorithm);

                if (crackedHash.equals(targetHash)) {
                    // Display a custom dialog when the password is found
                    JOptionPane.showMessageDialog(frame, "Password found: " + line, "Password Found", JOptionPane.INFORMATION_MESSAGE);
                    return;
                }
            }
            JOptionPane.showMessageDialog(frame, "Password not found in the dictionary.", "Password Not Found", JOptionPane.INFORMATION_MESSAGE);
        } catch (IOException | NoSuchAlgorithmException ex) {
            JOptionPane.showMessageDialog(frame, "An error occurred: " + ex.getMessage(), "Error", JOptionPane.ERROR_MESSAGE);
        }
    }

    private String hash(String input, String algorithm) throws NoSuchAlgorithmException {
        MessageDigest md = MessageDigest.getInstance(algorithm);
        md.update(input.getBytes());
        byte[] digest = md.digest();

        StringBuilder sb = new StringBuilder();
        for (byte b : digest) {
            sb.append(String.format("%02x", b));
        }
        return sb.toString();
    }

    public void display() {
        frame.setVisible(true);
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                MultiHashGeneratorAndCrackerGUI gui = new MultiHashGeneratorAndCrackerGUI();
                gui.display();
            }
        });
    }
}
