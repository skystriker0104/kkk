import java.io.*;
import java.net.*;
import java.nio.charset.StandardCharsets;
import java.util.*;

public class UDP_All {
}

/** JNP-802: UDP - Data Type (cổng 2207) - tìm các số còn thiếu trong khoảng [1..n] */
class UDP_DataType1 {
    public static void main(String[] args) {
        String host = "36.50.135.242";
        int port = 2207;

        String studentCode = "B23DCCN551";
        String qCode = "Xt52GcTl";

        try(DatagramSocket socket = new DatagramSocket()){
            socket.setSoTimeout(5000);
            InetAddress address = InetAddress.getByName(host);

            String request = ";" + studentCode + ";" + qCode;
            byte[] sendData = request.getBytes(StandardCharsets.UTF_8);
            socket.send(new DatagramPacket(sendData, sendData.length, address, port));

            byte[] buf = new byte[4096];

            DatagramPacket receivePacket = new DatagramPacket(buf, buf.length);
            socket.receive(receivePacket);
            String response = new String(receivePacket.getData(), 0, receivePacket.getLength(), StandardCharsets.UTF_8).trim();

            System.out.println("res: " + response);
            
            String[] parts = response.split(";");
            String requestId = parts[0];
            int n = Integer.parseInt(parts[1]);

            // c. Tim cac so con thieu trong [1..n]
            boolean[] exists = new boolean[n + 1];
            if (parts.length > 2 && !parts[2].isEmpty()) {
                for (String s : parts[2].split(",")) {
                    int v = Integer.parseInt(s.trim());
                    if (v >= 1 && v <= n) exists[v] = true;
                }
            }

            StringBuilder sb = new StringBuilder();
            for (int i = 1; i <= n; i++) {
                if (!exists[i]) {
                    if (sb.length() > 0) sb.append(",");
                    sb.append(i);
                }
            }

            String ans = requestId + ";" + sb;
            byte[] ansData = ans.getBytes(StandardCharsets.UTF_8);
            socket.send(new DatagramPacket(ansData, ansData.length, address, port));
            System.out.println("ans: " + ans);

        } catch (Exception e){
            e.printStackTrace();
        }
    }
}

/** JNP-931: UDP - Data Type (cổng 2207) - tìm giá trị lớn nhất (max) và nhỏ nhất (min) */
class UDP_DataType2 {
    public static void main(String[] args){
        String host = "36.50.135.242";
        int port = 2207;

        String sCode = "B23DCCN551";
        String qCode = "kZTgs5pr";
        try(DatagramSocket socket = new DatagramSocket()){
            socket.setSoTimeout(5000);
            InetAddress address = InetAddress.getByName(host);
            
            String req = ";" + sCode + ";" + qCode;
            byte[] sendData = req.getBytes(StandardCharsets.UTF_8);
            socket.send(new DatagramPacket(sendData, sendData.length, address, port));
        
            byte[] buf = new byte[4096];
            DatagramPacket receivePacket = new DatagramPacket(buf, buf.length);
            socket.receive(receivePacket);
            String response = new String(receivePacket.getData(), 0, receivePacket.getLength(), StandardCharsets.UTF_8).trim();
        
            System.out.println("res: " + response);

            String[] parts = response.split(";");
            String reqId = parts[0];
            String[] content = parts[1].split(",");
            
            int max = 0;
            int min = Integer.MAX_VALUE;
            for(int i = 0; i<content.length; i++){
                int num = Integer.parseInt(content[i]);
                if(max < num){
                    max = num;
                }
                if(min > num){
                    min = num;
                }
            }
            String ans = reqId + ";" + max + "," + min;
            byte[] ansData = ans.getBytes(StandardCharsets.UTF_8);
            socket.send(new DatagramPacket(ansData, ansData.length, address, port));
        } catch (Exception ex){
            ex.printStackTrace();
        }
    }
}

/** JNP-803: UDP - String (cổng 2208) - ký tự xuất hiện nhiều nhất + vị trí (đánh số từ 1) */
class UDP_String1 {
    public static void main(String[] args){
        String host = "36.50.135.242";
        int port = 2208;

        String sCode = "B23DCCN551";
        String qCode = "LXf3vOcq";
        try(DatagramSocket socket = new DatagramSocket()){
            socket.setSoTimeout(5000);
            InetAddress address = InetAddress.getByName(host);
            
            String req = ";" + sCode + ";" + qCode;
            byte[] sendData = req.getBytes(StandardCharsets.UTF_8);
            socket.send(new DatagramPacket(sendData, sendData.length, address, port));

            byte[] buf = new byte[4096];
            DatagramPacket recei = new DatagramPacket(buf, buf.length);
            socket.receive(recei);
            String res = new String(recei.getData(), 0, recei.getLength(), StandardCharsets.UTF_8).trim();

            System.out.println("res: " + res);
            String[] parts = res.split(";");
            String tmp = parts[1];
            HashMap<Character, Integer> countMap = new HashMap<>();
            int max = 0;
            char maxc = ' ';
            for(char c : tmp.toCharArray()){
                countMap.put(c, countMap.getOrDefault(c, 0) + 1);
                if(max < countMap.get(c)){
                    max = countMap.get(c);
                    maxc = c;
                }                
            }
            StringBuilder viTri = new StringBuilder();
            for(int i=0; i<tmp.length(); i++){
                if(tmp.charAt(i) == maxc){
                    viTri.append(i+1 + ",");
                }
            }
            String ans = parts[0] + ";" + maxc + ":" + viTri.toString();

            byte[] ansData = ans.getBytes(StandardCharsets.UTF_8);
            socket.send(new DatagramPacket(ansData, ansData.length, address, port));
            System.out.println("ans: " + ans);
        } catch (Exception ex){
            ex.printStackTrace();
        }
    }
}

/** JNP-932: UDP - String (cổng 2208) - chuẩn hóa chuỗi theo Title Case (viết hoa chữ cái đầu) */
class UDP_String2 {
    public static void main(String[] args){
        String host = "36.50.135.242";
        int port = 2208;

        String sCode = "B23DCCN551";
        String qCode = "E9Lodv0y";
        try(DatagramSocket socket = new DatagramSocket()){
            socket.setSoTimeout(5000);
            InetAddress address = InetAddress.getByName(host);
            
            String req = ";" + sCode + ";" + qCode;
            byte[] sendData = req.getBytes(StandardCharsets.UTF_8);
            socket.send(new DatagramPacket(sendData, sendData.length, address, port));

            byte[] buf = new byte[4096];
            DatagramPacket receivePacket = new DatagramPacket(buf, buf.length);
            socket.receive(receivePacket);
            String res = new String(receivePacket.getData(), 0, receivePacket.getLength(), StandardCharsets.UTF_8);

            System.out.println("res " + res);
            
            String[] parts = res.split(";");
            String[] tmp = parts[1].split(" ");

            StringBuilder ans = new StringBuilder();
            
            for(int i=0; i<tmp.length; i++){
                StringBuilder sb = new StringBuilder();
                sb.append(tmp[i].substring(0,1).toUpperCase());
                sb.append(tmp[i].substring(1, tmp[i].length()).toLowerCase());
                ans.append(sb.toString() + " ");
            }

            String ans1 = parts[0] + ";" + ans.toString().trim();
            System.out.println("ans " + ans.toString());
            
            byte[] ansData = ans1.getBytes(StandardCharsets.UTF_8);
            socket.send(new DatagramPacket(ansData, ansData.length, address, port));
        } catch (Exception ex){
            ex.printStackTrace();
        }
    }
}

/** JNP-933: UDP - Object (cổng 2209) - Student: chuẩn hóa tên + tạo email ptit.edu.vn */
class UDP_Object1 {
    public static void main(String[] args){
        String host = "36.50.135.242";
        int port = 2209;
        String studentCode = "B23DCCN551";
        String qCode = "RiKL0H6A";
        
        try (DatagramSocket socket = new DatagramSocket()){
            socket.setSoTimeout(5000);
            InetAddress address = InetAddress.getByName(host);
            
            String request = ";" + studentCode + ";" + qCode;
            byte[] data = request.getBytes(StandardCharsets.UTF_8);
            socket.send(new DatagramPacket(data, data.length, address, port));
            
            byte[] buffer = new byte[8192];
            DatagramPacket packet = new DatagramPacket(buffer, buffer.length);
            socket.receive(packet);
            
            byte[] received = packet.getData();
            int offset = packet.getOffset();
            int length = packet.getLength();
            
            byte[] requestId = Arrays.copyOfRange(received, offset, offset+8);
            
            ByteArrayInputStream bais = new ByteArrayInputStream(received, offset + 8, length - 8);
            ObjectInputStream ois = new ObjectInputStream(bais);
            
            Student student = (Student) ois.readObject();
            
            student.updateName();
            student.updateEmail();
            
            ByteArrayOutputStream baos = new ByteArrayOutputStream();
            ObjectOutputStream oos = new ObjectOutputStream(baos);
            
            oos.writeObject(student);
            oos.flush();
            
            byte[] objectBytes = baos.toByteArray();
            byte[] result = new byte[8 + objectBytes.length];
            System.arraycopy(requestId, 0, result, 0, 8);
            System.arraycopy(objectBytes, 0, result, 8, objectBytes.length);
            
            socket.send(new DatagramPacket(result, result.length, packet.getAddress(), packet.getPort()));
            
            ois.close();
            oos.close();
        } catch (Exception e){
            System.out.println(e);
        }
    }
}

/** Model Student cho bài JNP-933 (UDP.Student, serialVersionUID = 20171107L) */
class Student implements Serializable{
    private static final long serialVersionUID = 20171107;
    
    private String id;
    private String code;
    private String name;
    private String email;
    
    public Student(String id, String code, String name, String email){
        this.id = id;
        this.code = code;
        this.name = name;
        this.email = email;
    }
    
    public void updateName(){
        String[] s = this.name.toLowerCase().split("\\s+");
        StringBuilder res = new StringBuilder();
        for (String x: s){
            res.append(x.substring(0, 1).toUpperCase() + x.substring(1) + " ");
        }
        this.name = res.toString().trim();
    }
    
    public void updateEmail(){
        String[] s = this.name.toLowerCase().split("\\s+");
        StringBuilder res = new StringBuilder(s[s.length - 1]);
        for (int i = 0; i < s.length - 1; i++){
            res.append(s[i].charAt(0));
        }
        
        res.append("@ptit.edu.vn");
        this.email = res.toString();
    }
}

/** JNP-938: UDP - Object (cổng 2209) - Customer: chuẩn hóa tên (TÊN, Họ Đệm), ngày sinh (dd/mm/yyyy), tạo userName */
class UDP_Object2 {
    public static void main(String[] args){
        String host = "36.50.135.242";
        int port = 2209;
        String studentCode = "B23DCCN551";
        String qCode = "SZRpAEea";
        
        try (DatagramSocket socket = new DatagramSocket()){
            socket.setSoTimeout(5000);
            InetAddress address = InetAddress.getByName(host);
            
            String request = ";" + studentCode + ";" + qCode;
            byte[] data = request.getBytes(StandardCharsets.UTF_8);
            socket.send(new DatagramPacket(data, data.length, address, port));
            
            byte[] buffer = new byte[8192];
            DatagramPacket packet = new DatagramPacket(buffer, buffer.length);
            socket.receive(packet);
            
            byte[] received = packet.getData();
            int offset = packet.getOffset();
            int length = packet.getLength();
            
            byte[] requestId = Arrays.copyOfRange(received, offset, offset+8);
            
            ByteArrayInputStream bais = new ByteArrayInputStream(received, offset + 8, length - 8);
            ObjectInputStream ois = new ObjectInputStream(bais);
            
            Customer customer = (Customer) ois.readObject();
            
            customer.updateUserName();
            customer.updateName();
            customer.updateBirth();
            
            
            ByteArrayOutputStream baos = new ByteArrayOutputStream();
            ObjectOutputStream oos = new ObjectOutputStream(baos);
            
            oos.writeObject(customer);
            oos.flush();
            
            byte[] objectBytes = baos.toByteArray();
            byte[] result = new byte[8 + objectBytes.length];
            System.arraycopy(requestId, 0, result, 0, 8);
            System.arraycopy(objectBytes, 0, result, 8, objectBytes.length);
            
            socket.send(new DatagramPacket(result, result.length, packet.getAddress(), packet.getPort()));
            
            ois.close();
            oos.close();
        } catch (Exception e){
            System.out.println(e);
        }
    }
}

/** Model Customer cho bài JNP-938 (UDP.Customer, serialVersionUID = 20151107L) */
class Customer implements Serializable{
    private static final long serialVersionUID = 20151107;

    String id;
    String code;
    String name;
    String dayOfBirth;
    String userName;

    public Customer(){

    }
    public Customer(String id, String code, String name, String dayOfBirth, String userName){
        this.id = id;
        this.code = code;
        this.name = name;
        this.dayOfBirth = dayOfBirth;
        this.userName = userName;
    }
    public String getId(){
        return this.id;
    }
    public String getCode(){
        return this.code;
    }
    public String getName(){
        return this.name;
    }
    public String getDayOfBirth(){
        return this.dayOfBirth;
    }
    public String getUserName(){
        return this.userName;
    }

    public void updateName(){
        String[] tmp = this.name.split(" ");
        StringBuilder newName = new StringBuilder(); 
        newName.append(tmp[tmp.length-1].toUpperCase()+",");
        for(int i=0; i<tmp.length-1; i++){
            newName.append(" " + tmp[i].substring(0,1).toUpperCase() + tmp[i].substring(1).toLowerCase());
        }
        this.name = newName.toString();
    }
    public void updateBirth(){
        String[] tmp = this.dayOfBirth.split("-");
        StringBuilder newBirth = new StringBuilder();
        newBirth.append(tmp[1]+ "/" + tmp[0] + "/" + tmp[2]);
        this.dayOfBirth = newBirth.toString();

    }
    public void updateUserName(){
        String[] parts = this.name.split(" ");
        StringBuilder newUserName = new StringBuilder();
        for(int i=0; i<parts.length; i++){
            if(i!=parts.length-1){
                newUserName.append(parts[i].substring(0,1).toLowerCase());
            }
            else{
                newUserName.append(parts[i].toLowerCase());
            }
        }
        this.userName = newUserName.toString();
    }
}
