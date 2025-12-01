# Calculadora-IMC---Ari
App para calcular tu IMC - Ari Alexis - Esc. Cetis 72

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.Properties;
import javax.mail.*;
import javax.mail.internet.*;

public class IMCCalculatorApp {
    private JFrame frame;
    private JTextField nameField, weightField, heightField;
    private JLabel resultLabel, messageLabel;
    private JButton calculateButton, emailButton, restartButton;

    public IMCCalculatorApp() {
        frame = new JFrame("Calculadora de IMC");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(400, 300);
        frame.setLayout(new FlowLayout());

        JLabel titleLabel = new JLabel("Elaborado por Ari Alexis");
        titleLabel.setForeground(Color.GRAY);
        frame.add(titleLabel);

        JButton startButton = new JButton("Iniciar");
        startButton.addActionListener(e -> showInputFields());
        frame.add(startButton);

        frame.setVisible(true);
    }

    private void showInputFields() {
        frame.getContentPane().removeAll();
        frame.repaint();

        nameField = new JTextField(15);
        weightField = new JTextField(15);
        heightField = new JTextField(15);
        calculateButton = new JButton("Calcular");
        calculateButton.addActionListener(e -> calculateIMC());

        frame.add(new JLabel("Nombre:"));
        frame.add(nameField);
        frame.add(new JLabel("Peso (kg):"));
        frame.add(weightField);
        frame.add(new JLabel("Talla (m):"));
        frame.add(heightField);
        frame.add(calculateButton);

        frame.revalidate();
        frame.repaint();
    }

    private void calculateIMC() {
        String name = nameField.getText();
        double weight = Double.parseDouble(weightField.getText());
        double height = Double.parseDouble(heightField.getText());
        double imc = weight / (height * height);
        String result = String.format("IMC: %.2f", imc);
        String message = (imc < 25) ? "Normal" : "Sobrepeso";

        resultLabel = new JLabel(result);
        messageLabel = new JLabel(message);
        emailButton = new JButton("Enviar por correo");
        emailButton.addActionListener(e -> sendEmail(result, message));
        restartButton = new JButton("Realizar otro cálculo");
        restartButton.addActionListener(e -> showInputFields());

        frame.getContentPane().removeAll();
        frame.add(resultLabel);
        frame.add(messageLabel);
        frame.add(emailButton);
        frame.add(restartButton);

        frame.revalidate();
        frame.repaint();
    }

    private void sendEmail(String result, String message) {
        String to = JOptionPane.showInputDialog("Ingrese su correo:");
        String from = "your_email@example.com"; // Replace with your email
        String host = "smtp.example.com"; // Replace with your SMTP server

        Properties properties = System.getProperties();
        properties.setProperty("mail.smtp.host", host);
        Session session = Session.getDefaultInstance(properties);

        try {
            MimeMessage mimeMessage = new MimeMessage(session);
            mimeMessage.setFrom(new InternetAddress(from));
            mimeMessage.addRecipient(Message.RecipientType.TO, new InternetAddress(to));
            mimeMessage.setSubject("Resultado de IMC");
            mimeMessage.setText(result + "\n" + message);
            Transport.send(mimeMessage);
            JOptionPane.showMessageDialog(frame, "Correo enviado exitosamente.");
        } catch (MessagingException mex) {
            mex.printStackTrace();
        }
    }

    public static void main(String[] args) {
        new IMCCalculatorApp();
    }
}