# Programs Related to Networking

- [Network Methods](#network-methods)
  - [Distance Vector Routing](#distance-vector-routing)
  - [Error Correcting Code Hamming Code](#error-correcting-code-hamming-code)
  - [Error Detecting Cyclic Redundancy Check Method](#error-detecting-cyclic-redundancy-check-method)
  - [Error Detecting Checksum Method](#error-detecting-checksum-method)
  - [Dijkstra’s Algorithm (Static Routing Algorithm)](#dijkstras-algorithm-static-routing-algorithm)
- [Client-Server Programs](#client-server-programs)
  - [Connection Oriented](#connection-oriented)
    - [Echo Hello](#echo-hello-connection-oriented)
    - [Chat Application](#chat-application)
    - [File Sharing](#file-sharing)
    - [Multiple Clients](#multiple-clients)
  - [Connectionless](#connectionless)
    - [Echo Hello](#echo-hello-connectionless)

## Network Methods

### Distance Vector Routing

Write a program to implement distance vector routing.

```Java
import java.util.*;

class Node {
    int[] from, distance;

    static void PrintArray(Node[] route, int noOfNodes, boolean incrementNodeNumber) {
        final int INCREMENT = (incrementNodeNumber) ? 1 : 0;
        final String STR_DESTINATION = "Destination";
        final String STR_NEXT_HOP = "Next Hop";
        final String STR_DISTANCE = "Distance";
        final String STR_FORMAT = "%" + STR_DESTINATION.length() + "s\t%" + STR_NEXT_HOP.length() + "s\t%"
                + STR_DISTANCE.length() + "s\n";
        for (int i = 0; i < noOfNodes; i++) {
            System.out.format("Information of router %d:\n", (i + 1));
            System.out.format(STR_FORMAT, STR_DESTINATION, STR_NEXT_HOP, STR_DISTANCE);
            for (int j = 0; j < noOfNodes; j++) {
                System.out.format(STR_FORMAT, (j + INCREMENT), (route[i].from[j] + INCREMENT), route[i].distance[j]);
            }
            if (i != (noOfNodes - 1)) {
                System.out.println();
            }
        }
    }
}

class DistanceVectorRouter extends Node {

    public static Node[] Calculate(int noOfNodes, Node[] route) {
        boolean flag = true;
        do {
            flag = false;
            for (int i = 0; i < noOfNodes; i++) {
                for (int j = 0; j < noOfNodes; j++) {
                    for (int k = 0; k < noOfNodes; k++) {
                        if (route[i].distance[j] > (route[i].distance[k] + route[k].distance[j])) {
                            route[i].distance[j] = route[i].distance[k] + route[k].distance[j];
                            route[i].from[j] = k;
                            flag = true;
                        }
                    }
                }
            }

        } while (flag);
        return route;
    }
}

public class Main {

    static void printArray(int[][] arr) {
        for (int[] is : arr) {
            for (int x : is) {
                System.out.print(x + " ");
            }
            System.out.println();
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter number of nodes: ");
        int noOfNodes = sc.nextInt();

        Node[] route = new Node[noOfNodes];
        System.out.println("\nEnter the edges details. Enter 99 if nodes do not have any edge.\n");
        for (int i = 0; i < noOfNodes; i++) {
            route[i] = new Node();
            route[i].distance = new int[noOfNodes];
            route[i].from = new int[noOfNodes];
            for (int j = 0; j < noOfNodes; j++) {
                if (i != j) {
                    System.out.print("Enter the distance from the node " + (i + 1) + " to the node " + (j + 1) + ": ");
                    route[i].distance[j] = sc.nextInt();
                    route[i].from[j] = j;
                } else {
                    route[i].distance[j] = 0;
                    route[i].from[j] = j;
                }
            }
            System.out.println();
        }

        DistanceVectorRouter.PrintArray(DistanceVectorRouter.Calculate(noOfNodes, route), noOfNodes, true);
        sc.close();
    }
}
```

Output

```Text
Enter number of nodes: 4

Enter the edges details. Enter 99 if nodes do not have any edge.

Enter the distance from the node 1 to the node 2: 3
Enter the distance from the node 1 to the node 3: 5
Enter the distance from the node 1 to the node 4: 99

Enter the distance from the node 2 to the node 1: 3
Enter the distance from the node 2 to the node 3: 99
Enter the distance from the node 2 to the node 4: 1

Enter the distance from the node 3 to the node 1: 5
Enter the distance from the node 3 to the node 2: 4
Enter the distance from the node 3 to the node 4: 2

Enter the distance from the node 4 to the node 1: 99
Enter the distance from the node 4 to the node 2: 1
Enter the distance from the node 4 to the node 3: 2

Information of router 1:
Destination     Next Hop        Distance
          1            1               0
          2            2               3
          3            3               5
          4            2               4

Information of router 2:
Destination     Next Hop        Distance
          1            1               3
          2            2               0
          3            4               3
          4            4               1

Information of router 3:
Destination     Next Hop        Distance
          1            1               5
          2            4               3
          3            3               0
          4            4               2

Information of router 4:
Destination     Next Hop        Distance
          1            2               4
          2            2               1
          3            3               2
          4            4               0
```

### Error Correcting Code Hamming Code

Write a program to implement Error Correcting Code Hamming Code.

```Java
public class Main {
    final static short DATAWORD_LENGTH = 4;
    final static short CODEWORD_LENGTH = 3;
    final static short MAX_LENGTH_CODE_WORD = DATAWORD_LENGTH + CODEWORD_LENGTH;

    // Converts string each character to byte and returns as an array
    static byte[] stringToByteArray(String str) {
        byte[] x = new byte[str.length()];
        for (int i = 0; i < str.length(); i++)
            x[i] = Byte.parseByte(str.charAt(i) + "");
        return x;
    }

    // Reverse a given string to byte array in reverse
    static byte[] stringReverseToByte(String str, int newLength) {
        byte[] x = new byte[newLength];
        for (int i = 0, j = (str.length() - 1); i < str.length(); i++, j--)
            x[i] = Byte.parseByte(str.charAt(j) + "");
        return x;
    }

    static byte[] stringReverseToByte(String str) {
        return stringReverseToByte(str, str.length());
    }

    // Concate every element into empty string and returns that string
    static String byteArrayToString(byte[] array) {
        String str = "";
        for (int i = 0; i < array.length; i++)
            str += array[i];
        return str;
    }

    public static void main(String[] args) {
        byte redundant[] = new byte[CODEWORD_LENGTH];
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter four bit data word: ");
        String dataWordStr = sc.next();
        // String dataWordStr = "0111";

        // Removing extra bits after 4 bits entred by user, and copying first 4 bits
        dataWordStr = dataWordStr.substring(0, DATAWORD_LENGTH);
        byte codeWord[] = stringReverseToByte(dataWordStr, MAX_LENGTH_CODE_WORD);

        // Calculating redundant bit
        redundant[0] = Byte.valueOf(String.valueOf((codeWord[0] + codeWord[2] + codeWord[3]) % 2));
        redundant[1] = Byte.valueOf(String.valueOf((codeWord[0] + codeWord[1] + codeWord[2]) % 2));
        redundant[2] = Byte.valueOf(String.valueOf((codeWord[1] + codeWord[2] + codeWord[3]) % 2));

        // Appending redundant bits to codeWord
        for (int i = DATAWORD_LENGTH; i < codeWord.length; i++)
            codeWord[i] = redundant[i - DATAWORD_LENGTH];
        System.out.println("Codeword: " + byteArrayToString(codeWord));

        System.out.print("Enter received codeword: ");
        String dataWordStrReceived = sc.next();
        
        // String dataWordStrReceived = "0011001";
        byte[] cwRec = stringToByteArray(dataWordStrReceived);

        byte[] syndrome = new byte[CODEWORD_LENGTH];
        
        // Calculating syndrome bits
        syndrome[0] = Byte.parseByte(String.valueOf((cwRec[1] + cwRec[2] + cwRec[3] + cwRec[6]) % 2));
        syndrome[1] = Byte.parseByte(String.valueOf((cwRec[0] + cwRec[1] + cwRec[2] + cwRec[5]) % 2));
        syndrome[2] = Byte.parseByte(String.valueOf((cwRec[0] + cwRec[2] + cwRec[3] + cwRec[4]) % 2));

        // Printing syndrome array in reverse
        System.out.println("Syndrome: " + byteArrayToString(stringReverseToByte(byteArrayToString(syndrome))));
        
        // Calculation to find error bit, if any
        int s = syndrome[2] * 4 + syndrome[1] * 2 + syndrome[0];
        System.out.println(((s == 0) ? "System.out.println" : "Error on position " + (s - 1)));
        
        // Swaping error bit
        cwRec[s - 2] = (byte) ((cwRec[s - 2] == 0) ? 1 : 0);
        System.out.print("Final data: " + byteArrayToString(cwRec));
        sc.close();
    }
}
```

Output

```Text
Codeword: 1110010
Enter received codeword: 0011001
Syndrome: 011
Error on position 2
Final data: 0111001
```

### Error Detecting Cyclic Redundancy Check Method

Write a program to implement Error detecting Cyclic Redundancy Check Method.

```Java

class ArrayMethods {
    // will retuns string to byte array
    static byte[] strToByteArray(String str) {
        byte[] array = new byte[str.length()];
        for (int i = 0; i < str.length(); i++)
            array[i] = Byte.parseByte(str.charAt(i) + "");
        return array;
    }

    // Converts byte array to string
    static String byteArrayToString(byte[] array) {
        String str = "";
        for (int i = 0; i < array.length; i++)
            str += array[i];
        return str;
    }

    // Byte array sum
    static byte byteArraySum(byte[] array) {
        byte sum = 0;
        for (byte b : array)
            sum += b;
        return sum;
    }
}

class CRCMethods {

    private byte[] divisor;

    CRCMethods(String divisor) {
        this.divisor = ArrayMethods.strToByteArray(divisor);
    }

    private byte exor(byte a, byte b) {
        return (byte) ((a == b) ? 0 : 1);
    }

    private byte[] divide(byte[] dividend, byte[] divisor) {
        byte[] remainder = new byte[divisor.length];
        byte[] _remainder = new byte[divisor.length - 1];
        byte[] _dividend = new byte[dividend.length + divisor.length];
        System.arraycopy(dividend, 0, _dividend, 0, dividend.length);
        System.arraycopy(dividend, 0, remainder, 0, divisor.length);
        for (int i = 0; i < dividend.length; i++) {
            boolean flag = (remainder[0] == 1);
            for (int j = 1; j < divisor.length; j++) {
                byte x = (byte) ((flag) ? divisor[j] : 0);
                remainder[j - 1] = exor(remainder[j], x);
            }
            remainder[divisor.length - 1] = _dividend[i + divisor.length];
        }
        System.arraycopy(remainder, 0, _remainder, 0, remainder.length - 1);
        return _remainder;
    }

    // Returns generated codeword
    String sender(String data) {
        byte[] remainder = divide(ArrayMethods.strToByteArray(data), divisor);
        // Appending remainder with data, which will be our generated codeword.
        String codeword = data + ArrayMethods.byteArrayToString(remainder);
        // codeword will be sent on receiver side
        return codeword;
    }

    // Will return false if there is no error, else it will return true
    boolean receiver(String codeword) {
        byte[] remainder = divide(ArrayMethods.strToByteArray(codeword), divisor);
        return (ArrayMethods.byteArraySum(remainder) != 0);
    }
}

public class Main {
    public static void main(String[] args) {
        java.util.Scanner sc = new java.util.Scanner(System.in);
        System.out.print("Enter data to send: ");
        String data = sc.next();
        System.out.print("Enter divisor: ");
        String divisor = sc.next();
        CRCMethods obj = new CRCMethods(divisor);
        String codeword = obj.sender(data);
        System.out.println("\nRemainder: " + codeword.substring(data.length()));
        System.out.println("Generated codeword: " + codeword);
        System.out.print("\nEnter received dataword: ");
        String receivedDataword = sc.next();
        if (obj.receiver(receivedDataword)) {
            System.out.println("There is an error");
        } else {
            System.out.println("There is no error, data received successfully");
        }   

        sc.close();
    }
}
```

Output (No change in data)

```Text
Enter data to send: 1001
Enter divisor: 1011

Remainder: 110
Generated codeword: 1001110

Enter received dataword: 1001110
There is no error, data received successfully
```

Output (Change in data)

```Text
Enter data to send: 1001
Enter divisor: 1011

Remainder: 110
Generated codeword: 1001110

Enter received dataword: 1100110
There is an error
```

### Error detecting Checksum Method

```Java
class CheckSum {
    private static final int DECIMAL_OF_1111 = 15;

    private int complementBase16(int number) {
        return (DECIMAL_OF_1111 - number);
    }

    private int sum(int[] array) {
        int sum = 0;
        for (int x : array) {
            sum += x;
        }
        return sum;
    }

    private int wrappedSum(int[] array) {
        int sum = sum(array);
        int wrappedSum = sum % DECIMAL_OF_1111;
        int checkSum = complementBase16(wrappedSum);
        return checkSum;
    }

    int[] sender(int[] packet) {
        int packetLength = packet.length;
        int[] packetToSend = new int[packetLength + 1];
        System.arraycopy(packet, 0, packetToSend, 0, packetLength);
        int x = wrappedSum(packet);
        packetToSend[packetLength] = x;
        return packetToSend;
    }

    // If no error in packet, it will return true
    boolean receiver(int[] packet) {
        return (complementBase16(wrappedSum(packet)) == 0);
    }
}

public class Main {
    private static void printArray(int[] array) {
        for (int i = 0; i < array.length; i++) {
            if (i != array.length - 1) {
                System.out.print(array[i] + ", ");
            } else {
                System.out.println(array[i]);
            }
        }
    }

    public static void main(String[] args) {
        java.util.Scanner sc = new java.util.Scanner(System.in);
        CheckSum obj = new CheckSum();
        System.out.print("Enter size of the packet: ");
        int n = sc.nextInt();
        int[] packet = new int[n];
        System.out.print("\nEnter " + n + " values to packet\n");
        for (int i = 0; i < n; i++) {
            packet[i] = sc.nextInt();
        }
        int[] dataToSend = obj.sender(packet);
        System.out.print("Data to send: ");
        printArray(dataToSend);
        int[] dataReceived = new int[n + 1];
        System.out.print("\nEnter " + (dataReceived.length) + " data received on receiver side\n");
        for (int i = 0; i < dataReceived.length; i++) {
            dataReceived[i] = sc.nextInt();
        }
        if (obj.receiver(dataReceived)) {
            System.out.println("There is no error, so data is accepted");
        } else {
            System.out.println("There is an error, so data is discarded");
        }
        sc.close();
    }
}
```

Output (No change in data)

```Text
Enter size of the packet: 5

Enter 5 values to packet
7
11
12
0
6
Data to send: 7, 11, 12, 0, 6, 9

Enter 6 data received on receiver side
7
11
12
0
6
9
There is no error, so data is accepted
```

Output (Change in data)

```Text
Enter size of the packet: 5

Enter 5 values to packet
7
11
12
0
6
Data to send: 7, 11, 12, 0, 6, 9

Enter 6 data received on receiver side
6
11
12
0
6
9
There is an error, so data is discarded
```

### Dijkstra’s Algorithm (Static Routing Algorithm)

```Java
class Dijkstra {
    // Returns the index of a node having minimum value
    private static int findMinimumDistanceIndex(int[] distance, Boolean[] visited) {
        int minDistance = Integer.MAX_VALUE;
        int minDistanceIndex = -1;
        for (int i = 0; i < distance.length; i++) {
            if (!visited[i] && distance[i] < minDistance) {
                minDistance = distance[i];
                minDistanceIndex = i;
            }
        }
        return minDistanceIndex;
    }

    // returns the shortest distance from the source for each nodes
    public static int[] solve(int[][] graph, int sourceNode) {
        int count = graph.length;
        int[] distance = new int[count];
        Boolean[] visited = new Boolean[count];
        Arrays.fill(distance, Integer.MAX_VALUE);
        Arrays.fill(visited, false);
        distance[sourceNode] = 0;
        for (int i = 0; i < count; i++) {
            int u = findMinimumDistanceIndex(distance, visited);
            visited[u] = true;
            for (int v = 0; v < count; v++) {
                if (!visited[v] && graph[u][v] != 0 && ((distance[u] + graph[u][v]) < distance[v])) {
                    distance[v] = distance[u] + graph[u][v];
                }
            }
        }
        return distance;
    }

    public static void printSolution(int[] distance, int sourceNode) {
        System.out.println("Distance from source node " + sourceNode);
        System.out.println("Node\tDistance");
        for (int i = 0; i < distance.length; i++) {
            System.out.println(i + "\t" + distance[i]);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter number of nodes in the network: ");
        int nodes = sc.nextInt();
        int[][] graph = new int[nodes][nodes];
        System.out.println("\nEnter distance between nodes (bidirected graph)");
        for (int i = 0; i < nodes; i++) {
            for (int j = i; j < nodes; j++) {
                if (i != j) {
                    System.out.format("Enter distace between node %d and %d: ", i, j);
                    graph[i][j] = sc.nextInt();
                    graph[j][i] = graph[i][j];
                } else {
                    graph[i][j] = 0;
                }
            }
            System.out.println();
        }
        System.out.print("Enter the source node: ");
        int sourceNode = sc.nextInt();
        System.out.println();
        Dijkstra.printSolution(Dijkstra.solve(graph, sourceNode), sourceNode);
        sc.close();
    }
}
```

Output

```Text
Enter number of nodes in the network: 6

Enter distance between nodes (bidirected graph)
Enter distace between node 0 and 1: 2
Enter distace between node 0 and 2: 5
Enter distace between node 0 and 3: 1
Enter distace between node 0 and 4: 0
Enter distace between node 0 and 5: 0

Enter distace between node 1 and 2: 3
Enter distace between node 1 and 3: 2
Enter distace between node 1 and 4: 0
Enter distace between node 1 and 5: 0

Enter distace between node 2 and 3: 0
Enter distace between node 2 and 4: 1
Enter distace between node 2 and 5: 5

Enter distace between node 3 and 4: 1
Enter distace between node 3 and 5: 0

Enter distace between node 4 and 5: 2


Enter the source node: 0
Distance from source node 0
Node    Distance
0       0
1       2
2       3
3       1
4       2
5       4
```

## Client-Server Programs

### Connection Oriented

#### Echo Hello (Connection Oriented)

In this program server replies with the same message client has sent.

Server.java

```Java
public class Server {
    private static int PORT_NO = 8888;

    public static void main(String[] args) throws Exception {
        ServerSocket server = new ServerSocket(PORT_NO);
        Socket client = server.accept();
        BufferedReader serverStream = new BufferedReader(new InputStreamReader(client.getInputStream()));
        String string = serverStream.readLine();
        System.out.println("Client says: " + string);
        PrintWriter serverWriter = new PrintWriter(client.getOutputStream(), true);
        serverWriter.println(string);
        serverStream.close();
        client.close();
        server.close();
    }
}
```

Client.java

```Java
public class Client {
    private static String SERVER_IP = "localhost";
    private static int PORT_NO = 8888;

    public static void main(String[] args) throws Exception {
        Socket server = new Socket(SERVER_IP, PORT_NO);
        BufferedReader userInput = new BufferedReader(new InputStreamReader(System.in));
        PrintWriter serverWriter = new PrintWriter(server.getOutputStream(), true);
        System.out.print("Enter a message to be send to the server: ");
        String line = userInput.readLine();
        BufferedReader serverBufferedReader = new BufferedReader(new InputStreamReader(server.getInputStream()));
        serverWriter.println(line);
        System.out.println("Server replies: '" + serverBufferedReader.readLine() + "'");
        serverBufferedReader.close();
        serverWriter.close();
        userInput.close();
        server.close();
    }
}
```

#### Chat Application

Server.java

```Java
public class Server {
    private static int PORT_NO = 8888;

    public static void main(String[] args) throws Exception {
        ServerSocket server = new ServerSocket(PORT_NO);
        Socket client = server.accept();
        Scanner sc = new Scanner(System.in);
        BufferedReader clientReader = new BufferedReader(new InputStreamReader(client.getInputStream()));
        PrintWriter clientWriter = new PrintWriter(client.getOutputStream(), true);
        if (client.isConnected()) {
            System.out.println("Enter 'quit' or 'exit' to exit from this application\n");
            while (true) {
                String receivedMessage = clientReader.readLine();
                System.out.println("Client: " + receivedMessage);
                System.out.print("Enter a message: ");
                String messageToSend = sc.nextLine();
                clientWriter.println(messageToSend);
                String conditionString = messageToSend.toLowerCase().trim().replaceAll("'", "");
                if (conditionString.equals("quit") || conditionString.equals("exit")) {
                    break;
                }
            }
        }
        sc.close();
        client.close();
        server.close();
    }
}
```

Client.java

```Java
public class Client {
    private static int PORT_NO = 8888;
    private static String SERVER_IP = "localhost";

    public static void main(String[] args) throws Exception {
        Socket server = new Socket(SERVER_IP, PORT_NO);
        BufferedReader serverReader = new BufferedReader(new InputStreamReader(server.getInputStream()));
        PrintWriter serverWriter = new PrintWriter(server.getOutputStream(), true);
        Scanner sc = new Scanner(System.in);
        System.out.println("");
        if (server.isConnected()) {
            System.out.println("Connected to the server!");
            System.out.println("Enter 'quit' or 'exit' to exit from this application\n");
            while (true) {
                System.out.print("Enter a message: ");
                String messageToSend = sc.nextLine();
                serverWriter.println(messageToSend);
                System.out.println("Server: " + serverReader.readLine());
                String conditionString = messageToSend.toLowerCase().trim().replaceAll("'", "");
                if (conditionString.equals("quit") || conditionString.equals("exit")) {
                    break;
                }
            }
        }
        sc.close();
        server.close();
    }
}
```

#### File Sharing

Server.java

```Java
public class Server {
    private static int PORT_NO = 8888;

    public static void main(String[] args) throws Exception {
        String fileName = "file.txt";
        ServerSocket server = new ServerSocket(PORT_NO);
        Socket client = server.accept();
        System.out.println(client.getInetAddress().getHostName() + " connected!");

        FileInputStream fileReader = new FileInputStream(fileName);
        DataOutputStream clientWriter = new DataOutputStream(client.getOutputStream());

        System.out.println("File (" + fileName + ") sharing began!");
        int bit;
        while ((bit = fileReader.read()) != -1) {
            clientWriter.write(bit);
        }
        System.out.println("File sharing completed!");
        client.close();
        fileReader.close();
        server.close();
    }
}
```

Client.java

```Java
public class Client {
    private static int PORT_NO = 8888;
    private static String SERVER_IP = "localhost";

    public static void main(String[] args) throws Exception {
        String fileName = "output.txt";
        Socket server = new Socket(SERVER_IP, PORT_NO);
        FileOutputStream fileWriter = new FileOutputStream(fileName);
        DataInputStream serverReader = new DataInputStream(server.getInputStream());
        int bit;
        System.out.println("File writing began!");
        while (((bit = serverReader.read()) != -1)) {
            fileWriter.write(bit);
        }
        System.out.println("File writing completed!");
        fileWriter.close();
        server.close();
    }
}
```

#### Multiple Clients

Implement Concurrent TCP Server programming in which more than one client can connect and communicate with Server for sending the string and server returns the reverse of string to each of client.

Server.java

```Java
import java.net.*;
import java.io.*;

public class Server {
    private static int PORT_NO = 8888;
    private static ServerSocket server;

    public static void main(String[] args) throws Exception {
        server = new ServerSocket(PORT_NO);
        Socket client = null;
        int clientsCounter = 0;
        while (true) {
            client = server.accept();
            ++clientsCounter;
            System.out.println("Client " + clientsCounter + " connected!");
            new ServerThread(client, clientsCounter).start();
        }
    }
}

class ServerThread extends Thread {
    private int id;
    private Socket client;
    private BufferedReader serverStream;
    private PrintWriter serverWriter;

    ServerThread(Socket client, int id) {
        this.id = id;
        this.client = client;
        try {
            this.serverStream = new BufferedReader(new InputStreamReader(client.getInputStream()));
            this.serverWriter = new PrintWriter(client.getOutputStream(), true);
        } catch (Exception e) {
        }
    }

    public void run() {
        try {
            while (true) {
                String msg = serverStream.readLine();
                String conditionString = msg.toLowerCase().trim();
                String revMsg = new StringBuilder(msg).reverse().toString();
                System.out.println("Client " + this.id + ": " + msg);
                System.out.println("Server (you): " + revMsg);
                this.serverWriter.println(revMsg);
                if (conditionString.equals("quit") || conditionString.equals("exit")) {
                    break;
                }
            }
            this.client.close();
        } catch (Exception e) {
            System.out.println(e.toString());
        }
    }
}
```

Client.java

```Java
import java.net.*;
import java.io.*;

public class Client {
    private static String SERVER_IP = "localhost";
    private static int PORT_NO = 8888;

    public static void main(String[] args) throws Exception {
        Socket server = new Socket(SERVER_IP, PORT_NO);
        BufferedReader userInput = new BufferedReader(new InputStreamReader(System.in));
        PrintWriter serverWriter = new PrintWriter(server.getOutputStream(), true);
        BufferedReader serverReader = new BufferedReader(new InputStreamReader(server.getInputStream()));
        if (server.isConnected()) {
            System.out.println("Connected to the server!");
            System.out.println("Enter 'quit' or 'exit' to exit from this application\n");
            while (true) {
                System.out.print("Enter a message: ");
                String msg = userInput.readLine();
                String conditionString = msg.toLowerCase().trim().replaceAll("'", "").replaceAll("\"", "");
                serverWriter.println(msg);
                System.out.println("Server: " + serverReader.readLine());
                if (conditionString.equals("quit") || conditionString.equals("exit")) {
                    break;
                }
            }
        }
        serverReader.close();
        serverWriter.close();
        userInput.close();
        server.close();
    }
}
```

### Connectionless

#### Echo Hello (Connectionless)

In this program server replies with the same message client has sent.

Server.java

```Java
public class Server {

    public static void main(String[] args) throws Exception {
        InetAddress ia = InetAddress.getByName("localhost");
        DatagramSocket client = new DatagramSocket(8888);
        byte[] buffer = new byte[1024];
        DatagramPacket dpReceived = new DatagramPacket(buffer, buffer.length);
        client.receive(dpReceived);
        String data = new String(dpReceived.getData()).trim();
        String dataToSend = "'" + data + "'";
        byte[] buf = dataToSend.getBytes();
        System.out.println("Client says: "+ data);
        DatagramPacket dpToSend = new DatagramPacket(buf, buf.length, ia, dpReceived.getPort());
        client.send(dpToSend);
        client.close();
    }
}
```

Client.java

```Java
public class Client {
    public static void main(String[] args) throws Exception {
        DatagramSocket server = new DatagramSocket();
        java.util.Scanner sc = new java.util.Scanner(System.in);
        System.out.print("Enter a message to be send to the server: ");
        byte[] dataToSend = sc.nextLine().toString().getBytes();
        InetAddress ia = InetAddress.getLocalHost();
        DatagramPacket dpToSend = new DatagramPacket(dataToSend, dataToSend.length, ia, 8888);
        server.send(dpToSend);
        byte[] dataReceivedBuffer = new byte[1024];
        DatagramPacket dpReceived = new DatagramPacket(dataReceivedBuffer, dataReceivedBuffer.length);
        server.receive(dpReceived);
        System.out.println("Server says: " + new String(dpReceived.getData()).trim());
        sc.close();
        server.close();
    }
}
```
