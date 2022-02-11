import java.io.*;
import java.net.*;
import java.util.*;

public class ChatServer {
	ArrayList clientOutputStream;

	public class ClientHandler implements Runnable {
		BufferedReader reader;
		Socket socket;

		public ClientHandler(Socket clientSocket) {
			try {
			  socket = clientSocket;
				InputStreamReader IsReader = new InputStreamReader(socket.getInputStream());
				reader = new BufferedReader(IsReader);
			} catch (Exception ex) {ex.printStackTrace(); }
		}
		public void run() {
			String message;
			try {
			  while((message = reader.readLine()) != null) {
					System.out.println("Read: " + message);
					tellEveryone(message);
				}
			} catch (Exception ex) {ex.printStackTrace(); }
		}
	}

	 public static void main(String[] args) {
	 	new ChatServer().go();
	 }

	 public void go() {
		 System.out.println("Starting up..");
		 clientOutputStream = new ArrayList();
		 try {
		   ServerSocket serverSocket = new ServerSocket(4242);

			 while(true) {
				 Socket clientSocket = serverSocket.accept();
				 PrintWriter writer = new PrintWriter(clientSocket.getOutputStream());
				 clientOutputStream.add(writer);

				 Thread t = new Thread(new ClientHandler(clientSocket));
				 t.start();
				 System.out.println("Someone connected.");
			 }
		 } catch (Exception ex) {ex.printStackTrace(); }
		 System.out.println("Running...");
	 }

	 public void tellEveryone(String message) {
		 Iterator it = clientOutputStream.iterator();
		 while(it.hasNext()) {
			 try {
			   PrintWriter writer = (PrintWriter) it.next();
				 writer.println(message);
				 writer.flush();
			 } catch (Exception ex) {ex.printStackTrace(); }
		 }
	 }


}
