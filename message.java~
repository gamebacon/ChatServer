import javax.swing.*;
import java.awt.*;
import java.io.*;
import java.net.*;
import java.awt.event.*;
import java.util.*;

class GuiMessage {

	JTextField textField;
	JTextArea textArea;
	Socket socket;
	PrintWriter writer;
	BufferedReader reader;
	JFrame frame;

	public static void main(String[] args) {
			new GuiMessage().setUpGui();
	}

		void setUpGui() {
			Dimension screenSize = Toolkit.getDefaultToolkit().getScreenSize();
			JPanel topPanel = new JPanel();
			JPanel bottomPanel = new JPanel();
			bottomPanel.setBackground(randomColor());
			JButton sendButton = new JButton("Send");
			sendButton.addActionListener(new ButtonSendActionListener());
			textField = new JTextField(20);
			textArea = new JTextArea(20,20);
			textArea.setLineWrap(true);
			textArea.setWrapStyleWord(true);
			textArea.setEditable(false);
			JScrollPane textAreaScroller = new JScrollPane(textArea);
			textAreaScroller.setVerticalScrollBarPolicy(ScrollPaneConstants.VERTICAL_SCROLLBAR_ALWAYS);
			textAreaScroller.setHorizontalScrollBarPolicy(ScrollPaneConstants.HORIZONTAL_SCROLLBAR_NEVER);

			frame = new JFrame("Chat");

			frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
			frame.setSize(360, 400);

			topPanel.add(BorderLayout.CENTER, textField);
			topPanel.add(BorderLayout.CENTER, sendButton);
			bottomPanel.add(BorderLayout.SOUTH, textAreaScroller);

			frame.getContentPane().add(BorderLayout.CENTER, topPanel);
			frame.getContentPane().add(BorderLayout.SOUTH, bottomPanel);

			frame.setLocation(screenSize.width/2-frame.getSize().width/2, screenSize.height/2-frame.getSize().height/2 - 100);

			setUpNetWorking();

			Thread readerThread = new Thread(new IncomingReader());
			readerThread.start();

			frame.setVisible(true);
	}

	public Color randomColor(){
		int red = (int) (Math.random() * 254);
		int green = (int) (Math.random() * 254);
		int blue = (int) (Math.random() * 254);

		return new Color(red, green, blue);
	}




	public void setUpNetWorking() {
		try {
			socket = new Socket("192.168.1.114", 4242);
			InputStreamReader streamReader = new InputStreamReader(socket.getInputStream());
			reader = new BufferedReader(streamReader);
			writer = new PrintWriter(socket.getOutputStream());
			System.out.println("Networking established.");
		} catch (Exception ex) {ex.printStackTrace(); }

	}

	class ButtonSendActionListener implements ActionListener {
		public void actionPerformed(ActionEvent event){
			try {
			  writer.println(textField.getText());
				writer.flush();
			} catch (Exception ex) {ex.printStackTrace(); }
			textField.setText("");
			textField.requestFocus();
		}
	}

	public class IncomingReader implements Runnable {
		public void run(){
			String message;
			try {
			  while((message = reader.readLine()) != null) {
					System.out.println("read: " + message);
					textArea.append(message + "\n");
				}
			} catch (Exception ex) {ex.printStackTrace(); }
		}
	}
}
