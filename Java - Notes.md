Konvensjoner:
- Klasser, filnavn og interface: PascalCase. Eksempel: Person, BankKonto, osv.
- Metoder og variabler: camelCase. Eksempel: getNavn(), antallForsøk osv.
- Konstanter: UPPER_SNAKE_CASE. Eksempel: MAX_SIZE.


``` Java

import java.util.*;

class Person{
	private int age;
	private String name;
	
	public int getAge(){
		return age;
	}
	
   public void setAlder(int age) {
		if (age < 0) {
			throw new IllegalArgumentException("Alder kan ikke være negativ");
		}
		this.age=age;
	}

	public String getName(){
		return name;
	}
	
	public Person(){
		this.name = "ukjent"
		this.age = 0;
	}
	
	public Person (String name, int age){
		this.name = name;
		this.age = age;
	}
	
	public boolean isAdult() {
        return age >= 18;
    }
    
    @Override
    public String toString(){
	    return "Person{name='" + name + "', age=" + age + "}";
    }
    @Override
    public int hashCode() {
	    return Objects.hash(name, age);
    }
	
	
	
	
}


public class test{

	
	// Metoder
	static int sum(int a, int b) {
	    return a + b;
    }
    
	public static void main(String [] args){
	
		// Types
		int number = 10;
		long largeNumber = 1_000_000L;
		double decimalNumber = 99.55;
		char letterC = 'A';
		String name = "Adam";
		final int max = 100: 
		
		
		// ── OPERATORER ──
        // +  -  *  /  %       regning (% = rest)
        // == != < > <= >=    sammenligning
        // && || !            og, eller, ikke
        
        
        // String
        String text = "Hello world";
        boolean match = text.equals("Hello");// false
        int l = text.length();
        char i = charAt(0);
        
        // loops
         for (int i = 0; i < tall.length; i++) {
            System.out.println(tall[i]);
        }
        for (String person : liste) {
            System.out.println(person);
        }
        for (Map.Entry<String, Integer> entry : poeng.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
        
        
        // If else
        if(x == 5 && y == 6){
	        // do something
        }
        else if (x == 5){
	        // do something else
        }
        else{
	        // do something else again
        }
        String status = active ? "On": "Off";
        
        
        // Array
        int [] nums = {1, 2, 3};
        int [] empty = new int[3];// three zeros
        nums[0] = 2;
        int length = nums.length;
        Arrays.sort(nums);
        System.out.println(Arrays.toString(nums));
        
        
        // List / arraylist
        List<String> listName = new ArrayList<>();
        listName.add("something");
        listName.get(0);
        listName.add(0, "stringvalue");
        listName.set(0, "replacment");
        listName.remove("value"); // removes first of this
        listName.remove(0); // removes first value
        
        
        // Hashmap
        Map<String, Integer> poeng = new HashMap<>();
        poeng.put("Ola", 10);
        poeng.get("Ola"); // 10 or null
        poeng.getOrDefault("Per", 0);
        poeng.containsKey("Ola"); // true
        poeng.keySet();
        poeng.values();
        poeng.remove("Ola");   
        
        
        // Hashset
        Set<String> unike = new HashSet<>();
        unike.add("Ola");
        unike.add("Ola");                // Fortsatt bare ett element
        unike.contains("Ola");           // true
        unike.remove("Ola");
        
        
        // Queue
        Queue<Integer> queueName = new LinkedList<>();
        queue.offer(10);
        queue.offer(20);
        queue.peek();
        queue.poll();
        queue.size();
        queue.isEmpty;
        
        
        // Stack
        Stack<Integer> stackName = new Stack <>();
        stack.push(10);  // stack = {10}
        stack.push(20);  // stack = {20, 10}
        stack.peek();    // 20
        stack.pop();     // 20. stack = {10}
        stack.size();    // 1
        stack.isEmpty(); // false
        
        
        // String builder
        StringBuilder sb = new StringBuilder();
        sb.append("Hello ");
        sb.append("world");
        sb.charAt("0");
        sb.length();
        String result = sb.toString();
        sb.setLength(0);
        
        
        
        // Other
        Math.max(3, 7);                  // 7
        Math.min(3, 7);                  // 3
        Math.abs(-5);                    // 5
        
        Integer.parseInt("42");         // String → int
        String.valueOf(42);             // int → String
        
        
        // Trycatch
        
    try{
	    int number = Integer.parseInt("string");
	    System.out.println(number);
    }
    catch(NumberFormatException e){
	    System.out.println("Not a valid integer")
    }
    finally{
	    System.out.println("prints no matter what")
    }

	
	}
}
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

