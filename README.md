import RMI.ByteService;
import java.rmi.registry.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws Exception {
        String host = "36.50.135.242", sc = "B23DCCN005", qc = "eFGTWz6z"; 
        Registry reg = LocateRegistry.getRegistry(host, 1099);
        ByteService svc = (ByteService) reg.lookup("RMIByteService");

        byte[] data = svc.requestData(sc, qc);

        LinkedHashMap<Byte, Integer> cnt = new LinkedHashMap<>(); 
        for (byte b : data) cnt.merge(b, 1, Integer::sum);

        byte best = 0; int min = Integer.MAX_VALUE;
        for (Map.Entry<Byte, Integer> e : cnt.entrySet()) {
            if (e.getValue() < min) { min = e.getValue(); best = e.getKey(); }
        }
        svc.submitData(sc, qc, new byte[]{best, (byte) min});
    }
}


import RMI.ByteService;
import java.rmi.registry.*;
import java.util.*;

public class b2 {
    public static void main(String[] args) throws Exception {
        String host = "36.50.135.242", sc = "B23DCCN005", qc = "S0aTmErl";
        Registry reg = LocateRegistry.getRegistry(host, 1099);
        ByteService svc = (ByteService) reg.lookup("RMIByteService");

        byte[] data = svc.requestData(sc, qc);

        LinkedHashMap<Byte, Integer> cnt = new LinkedHashMap<>();
        for (byte b : data) cnt.merge(b, 1, Integer::sum);

        byte best = 0; int max = 0;
        for (Map.Entry<Byte, Integer> e : cnt.entrySet()) {
            if (e.getValue() > max) { max = e.getValue(); best = e.getKey(); }
        }
        svc.submitData(sc, qc, new byte[]{best, (byte) max});
    }
}



import RMI.CharacterService;
import java.rmi.registry.*;
import java.net.URI;

public class b3 {
    public static void main(String[] args) throws Exception {
        String host = "36.50.135.242", sc = "B23DCCN005", qc = "ADkXFtdt";
        Registry reg = LocateRegistry.getRegistry(host, 1099);
        CharacterService svc = (CharacterService) reg.lookup("RMICharacterService");

        String s = svc.requestCharacter(sc, qc);
        String encoded = new URI(null, null, s, null).toASCIIString();
        svc.submitCharacter(sc, qc, encoded);
    }
}


import RMI.CharacterService;
import java.rmi.registry.*;

public class b4 {
    static String toRoman(int n) {
        int[] v = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        String[] r = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < v.length; i++)
            while (n >= v[i]) { sb.append(r[i]); n -= v[i]; }
        return sb.toString();
    }

    public static void main(String[] args) throws Exception {
        String host = "36.50.135.242", sc = "B23DCCN005, qc = "im3Ux7YL";
        Registry reg = LocateRegistry.getRegistry(host, 1099);
        CharacterService svc = (CharacterService) reg.lookup("RMICharacterService");

        String s = svc.requestCharacter(sc, qc).trim();
        int n = Integer.parseInt(s.replaceAll("[^0-9]", "")); 
        svc.submitCharacter(sc, qc, toRoman(n));
    }
}


import RMI.DataService;
import java.rmi.registry.*;
import java.util.*;
 
public class b5 {
    static int[] toIntArray(Object o) {
        if (o instanceof int[]) return (int[]) o;
        if (o instanceof Integer[]) { Integer[] a = (Integer[]) o; int[] r = new int[a.length]; for (int i = 0; i < a.length; i++) r[i] = a[i]; return r; }
        if (o instanceof List) { List<?> l = (List<?>) o; int[] r = new int[l.size()]; for (int i = 0; i < r.length; i++) r[i] = ((Number) l.get(i)).intValue(); return r; }
        throw new IllegalArgumentException("Kiểu dữ liệu không hỗ trợ: " + o.getClass());
    }
 
    public static void main(String[] args) throws Exception {
        String host = "36.50.135.242", sc = "B23DCCN005", qc = "WmyOlxy6";
        Registry reg = LocateRegistry.getRegistry(host, 1099);
        DataService svc = (DataService) reg.lookup("RMIDataService");
 
        int[] a = toIntArray(svc.requestData(sc, qc));
        int n = a.length;
        List<Integer> res = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            boolean ok = (i == 0 || a[i] > a[i - 1]) && (i == n - 1 || a[i] > a[i + 1]);
            if (ok) res.add(i + 1); // chỉ số từ 1
        }
        svc.submitData(sc, qc, (java.io.Serializable) res);
    }
}
 



 import RMI.DataService;
import java.rmi.registry.*;
import java.util.*;
 
public class b5 {
    static int[] toIntArray(Object o) {
        if (o instanceof int[]) return (int[]) o;
        if (o instanceof Integer[]) { Integer[] a = (Integer[]) o; int[] r = new int[a.length]; for (int i = 0; i < a.length; i++) r[i] = a[i]; return r; }
        if (o instanceof List) { List<?> l = (List<?>) o; int[] r = new int[l.size()]; for (int i = 0; i < r.length; i++) r[i] = ((Number) l.get(i)).intValue(); return r; }
        throw new IllegalArgumentException("Kiểu dữ liệu không hỗ trợ: " + o.getClass());
    }
 
    public static void main(String[] args) throws Exception {
        String host = "36.50.135.242", sc = "B23DCCN005", qc = "KTwj7UlY";
        Registry reg = LocateRegistry.getRegistry(host, 1099);
        DataService svc = (DataService) reg.lookup("RMIDataService");
 
        int[] a = toIntArray(svc.requestData(sc, qc));
        int n = a.length;
        List<Integer> res = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            boolean ok = (i == 0 || a[i] > a[i - 1]) && (i == n - 1 || a[i] > a[i + 1]);
            if (ok) res.add(i + 1); 
        }
        svc.submitData(sc, qc, (java.io.Serializable) res);
    }
}
