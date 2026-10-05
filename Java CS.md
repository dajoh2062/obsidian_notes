Konvensjoner:
- Klasser, filnavn og interface: PascalCase. Eksempel: Person, BankKonto, osv.
- Metoder og variabler: camelCase. Eksempel: getNavn(), antallForsøk osv.
- Konstanter: UPPER_SNAKE_CASE. Eksempel: MAX_SIZE.

```Java
import java.util.*;

public class Main {

    // ── METODER ──
    static int summer(int a, int b) {
        return a + b;
    }

    static void hils(String navn) {
        System.out.println("Hei " + navn);
    }

    public static void main(String[] args) {

        // ── TYPER ──
        int alder = 23;
        long stortTall = 1_000_000L;
        double pris = 99.5;
        boolean aktiv = true;
        char bokstav = 'A';
        String navn = "Ola";
        final int MAKS = 100;

        // ── OPERATORER ──
        // +  -  *  /  %       regning (% = rest)
        // == != < > <= >=    sammenligning
        // && || !            og, eller, ikke
        int rest = 7 % 2;       // 1
        double halv = 5 / 2.0;  // 2.5 (5 / 2 gir 2)

        // ── STRING ──
        String tekst = "Hei Java";
        tekst.equals("Hei Java");       // true (bruk ikke ==)
        tekst.equalsIgnoreCase("hei java");
        tekst.length();                 // 8
        tekst.charAt(0);                 // 'H'
        tekst.substring(0, 3);          // "Hei"
        tekst.contains("Java");         // true
        tekst.startsWith("Hei");        // true
        tekst.split(" ");               // String[]
        tekst.replace("Java", "Ola");    // Ny streng
        tekst = tekst.toLowerCase();    // Strenger endres ikke på stedet
        tekst = tekst.trim();           // Fjern mellomrom i endene

        // ── IF / ELSE ──
        if (alder >= 18 && aktiv) {
            System.out.println("Voksen");
        } else {
            System.out.println("Ellers");
        }
        String status = aktiv ? "På" : "Av";

        // ── ARRAY: fast størrelse, indeks fra 0 ──
        int[] tall = {3, 1, 2};
        int[] tomArray = new int[5];     // Fem nuller
        tall[0] = 10;
        int lengde = tall.length;
        Arrays.sort(tall);
        System.out.println(Arrays.toString(tall));

        // ── LIST / ARRAYLIST: ordnet liste, tillater duplikater ──
        // List = grensesnitt, ArrayList = implementasjon
        List<String> liste = new ArrayList<>();
        liste.add("Ola");
        liste.add("Kari");
        liste.get(0);                   // "Ola"
        liste.set(0, "Per");
        liste.contains("Per");          // true
        liste.size();                   // 2
        liste.isEmpty();                // false
        liste.remove("Kari");           // Fjern verdi
        liste.remove(0);                // Fjern indeks
        Collections.sort(liste);

        // Generiske typer bruker Integer, Double, Boolean – ikke int osv.
        List<Integer> nummer = new ArrayList<>(List.of(3, 1, 2));
        nummer.remove(Integer.valueOf(3)); // Fjern verdien 3

        // ── MAP / HASHMAP: nøkkel → verdi, unike nøkler ──
        Map<String, Integer> poeng = new HashMap<>();
        poeng.put("Ola", 10);
        poeng.get("Ola");                // 10, eller null hvis mangler
        poeng.getOrDefault("Per", 0);    // 0
        poeng.containsKey("Ola");        // true
        poeng.put("Ola", poeng.getOrDefault("Ola", 0) + 1);
        poeng.keySet();                  // Alle nøkler
        poeng.values();                  // Alle verdier
        poeng.remove("Ola");

        // ── SET / HASHSET: unike verdier ──
        Set<String> unike = new HashSet<>();
        unike.add("Ola");
        unike.add("Ola");                // Fortsatt bare ett element
        unike.contains("Ola");           // true
        unike.remove("Ola");
        // HashMap/HashSet: ingen garantert rekkefølge
        // TreeMap/TreeSet: sortert rekkefølge

        // ── KØ / STAKK ──
        Queue<String> koe = new ArrayDeque<>();
        koe.offer("A");
        koe.peek();                     // Se første
        koe.poll();                     // Hent og fjern første

        Deque<String> stakk = new ArrayDeque<>();
        stakk.push("A");
        stakk.pop();                    // Hent og fjern sist lagt til

        // ── LØKKER ──
        for (int i = 0; i < tall.length; i++) {
            System.out.println(tall[i]);
        }

        for (String person : liste) {
            System.out.println(person);
        }

        for (Map.Entry<String, Integer> entry : poeng.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }

        int i = 0;
        while (i < 3) {
            i++;
            // continue; → neste runde
            // break;    → avslutt løkken
        }

        // ── NYTTIGE METODER ──
        Math.max(3, 7);                  // 7
        Math.min(3, 7);                  // 3
        Math.abs(-5);                    // 5
        Integer.parseInt("42");         // String → int
        String.valueOf(42);             // int → String
        Objects.equals(navn, "Ola");    // Tåler null

        // ── METODEKALL OG OBJEKTER ──
        int svar = summer(2, 3);
        hils("Ola");

        Person person = new Person("Kari");
        person.hils();
        System.out.println(person.getNavn());
    }
}

// ── KLASSE: oppskrift på objekter ──
class Person {
    private String navn;                // Felt

    public Person(String navn) {        // Konstruktør
        this.navn = navn;
    }

    public String getNavn() {           // Instansmetode
        return navn;
    }

    public void hils() {
        System.out.println("Hei, jeg heter " + navn);
    }
}
```

``` Java

import java.util.Objects;

public class Person {
    // Felt
    private String navn;
    private int alder;

    // Konstruktør uten argumenter
    public Person() {
        this("Ukjent", 0);
    }

    // Konstruktør med argumenter
    public Person(String navn, int alder) {
        this.navn = navn;
        setAlder(alder);
    }

    // Gettere
    public String getNavn() {
        return navn;
    }

    public int getAlder() {
        return alder;
    }

    // Settere
    public void setNavn(String navn) {
        this.navn = navn;
    }

    public void setAlder(int alder) {
        if (alder < 0) {
            throw new IllegalArgumentException("Alder kan ikke være negativ");
        }
        this.alder = alder;
    }

    // Vanlig instansmetode
    public boolean erVoksen() {
        return alder >= 18;
    }

    // Tekstrepresentasjon: brukes av System.out.println(person)
    @Override
    public String toString() {
        return "Person{navn='" + navn + "', alder=" + alder + "}";
    }

    // Sammenligner innhold: person.equals(annenPerson)
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;

        Person annen = (Person) obj;
        return alder == annen.alder && Objects.equals(navn, annen.navn);
    }

    // Like objekter må ha lik hashCode (HashMap/HashSet)
    @Override
    public int hashCode() {
        return Objects.hash(navn, alder);
    }
}

// Bruk:
// Person p = new Person("Ola", 23);
// p.setAlder(24);
// System.out.println(p.getNavn()); // Ola
// System.out.println(p);          // Person{navn='Ola', alder=24}
// p.equals(new Person("Ola", 24)); // true

// Unngå å endre felt brukt i equals/hashCode mens objektet
// er en nøkkel i HashMap eller et element i HashSet.
```

```Java

// twosum


class Solution {
    public int[] twoSum(int[] nums, int target) {

        Map<Integer, Integer> hashMap = new HashMap<>();


        int[] result = new int [2];

        for(int i = 0; i<nums.length; i++){
            int x = target - nums[i];
            if(hashMap.containsKey(x)){
                result[0] = hashMap.get(x);
                result[1] = i;
                break;

            }
            else if(!hashMap.containsKey(nums[i])){
                hashMap.put(nums[i], i);
            }
        }

        return result;
        
        
    }
}



```

```Java

// Intersection two arrays
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> unique1 = new HashSet<>();
        Set<Integer> uniqueJoint = new HashSet<>();

        for (int number : nums1) {
            unique1.add(number);
        }

        for (int number : nums2) {
            if (unique1.contains(number)) {
                uniqueJoint.add(number);
            }
        }

        int[] result = new int[uniqueJoint.size()];
        int index = 0;

        for (int number : uniqueJoint) {
            result[index++] = number;
        }

        return result;
    }
}
```

```Java
// Best time to buy and sell stock
class Solution {
    public int maxProfit(int[] prices) {
        int minPrice = prices[0];
        int maxProfit = 0;

        for (int i = 1; i < prices.length; i++) {
            int profit = prices[i] - minPrice;
            maxProfit = Math.max(maxProfit, profit);
            minPrice = Math.min(minPrice, prices[i]);
        }

        return maxProfit;
    }
}
```

```Java
// Merge two sorted linked lists
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        
        ListNode dummy = new ListNode();
        ListNode current = dummy;

        while (list1 != null && list2 != null) {
            if (list1.val <= list2.val) {
                current.next = list1;
                list1 = list1.next;
            } else {
                current.next = list2;
                list2 = list2.next;
            }

            current = current.next;
        }

        // Resten er allerede sortert
        current.next = (list1 != null) ? list1 : list2;

        return dummy.next;
    }
}
```


```Java
// Valid parenthesis
import java.util.Stack;

class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();

        for (char c : s.toCharArray()) {
            switch (c) {
                case '(':
                case '{':
                case '[':
                    stack.push(c);
                    break;

                case ')':
                    if (stack.isEmpty() || stack.pop() != '(') {
                        return false;
                    }
                    break;

                case '}':
                    if (stack.isEmpty() || stack.pop() != '{') {
                        return false;
                    }
                    break;

                case ']':
                    if (stack.isEmpty() || stack.pop() != '[') {
                        return false;
                    }
                    break;

                default:
                    return false;
            }
        }

        return stack.isEmpty();
    }
}
```

