# Greedy Algorithms ki Detailed Explanation

## Introduction to Greedy Algorithms

Greedy algorithm kya hai? Simple words mein, ye ek aisa approach hai jisme hum step-by-step solution banate hain, aur har step pe wo decision lete hain jo *us moment pe* sabse best lagta hai. Matlab, hum "local optimal choice" karte hain ye umeed karte hue ki ye humein "global optimal solution" tak pahuncha dega.

Iska matlab samajhte hain: Maan lo aap ek pahad par chadh rahe hain. Greedy approach ka matlab har kadam pe aap wahi direction choose karenge jo aapko sabse steep (tezi se upar) le jaye. Lekin dhyan rahe, har problem mein ye approach kaam nahi karega!

## Kab Greedy Use Karna Chahiye?

Greedy approach tab use karna chahiye jab:
1. **Optimal Substructure** - Agar bade problem ka optimal solution chote subproblems ke optimal solutions se ban sakta hai
2. **Greedy Choice Property** - Agar local optimal choices eventually global optimal solution deti hain

## Fractional Knapsack Problem in Depth

Fractional Knapsack problem ko samajhte hain:

Aapke paas kuch items hain with values aur weights. Aapko ek knapsack (bag) mein maximum value ke items daalne hain, lekin bag ka weight capacity limited hai. Fractional Knapsack mein aap items ke fractions (tukde) bhi le sakte hain.

Ye example socho:
- Items: {(60₹, 10kg), (100₹, 20kg), (120₹, 30kg)}
- Knapsack capacity: 50kg

```java
import java.util.Arrays;
import java.util.Comparator;

// Item class to store value and weight of an item
class Item {
    int value, weight;
    
    // Constructor
    Item(int value, int weight) {
        this.value = value;
        this.weight = weight;
    }
}

public class FractionalKnapsack {
    
    // Comparator to sort items based on value/weight ratio in descending order
    static class ItemComparator implements Comparator<Item> {
        public int compare(Item a, Item b) {
            // Ye important step hai - ratio calculate karo
            double r1 = (double) a.value / a.weight;  // Pehle item ka ratio
            double r2 = (double) b.value / b.weight;  // Dusre item ka ratio
            
            // Descending order mein sort karne ke liye r2 ko r1 se compare karo
            return Double.compare(r2, r1);
        }
    }
    
    // Main greedy function to solve the problem
    public static double fractionalKnapsack(int W, Item[] arr) {
        // Step 1: Sabse pehle items ko value/weight ratio ke basis pe sort karo (descending)
        // Ye greedy choice hai - highest value per weight wale items ko pehle lena
        Arrays.sort(arr, new ItemComparator());
        
        // Final result ko track karne ke liye variable
        double finalValue = 0.0;
        int remainingCapacity = W;  // Bache hue capacity ko track karne ke liye
        
        // Step 2: Sorted items pe loop chalao
        for (Item item : arr) {
            // Agar pure item ko bag mein daal sakte hain, to daal do
            if (item.weight <= remainingCapacity) {
                remainingCapacity -= item.weight;
                finalValue += item.value;
                // Debug ke liye print
                // System.out.println("Added complete item: value=" + item.value + ", weight=" + item.weight);
            }
            // Agar pure item nahi daal sakte, to fraction (tukda) daal do
            else {
                // Fraction calculate karo - kitna percent daal sakte hain
                double fraction = (double) remainingCapacity / item.weight;
                finalValue += item.value * fraction;
                
                // Debug ke liye print
                // System.out.println("Added fraction of item: " + fraction + " of value=" + item.value + ", weight=" + item.weight);
                
                // Ab bag full ho gaya, loop se bahar niklo
                break;
            }
        }
        
        return finalValue;
    }
    
    public static void main(String[] args) {
        // Example 1
        int W = 50;  // Knapsack ka capacity
        Item[] arr = { 
            new Item(60, 10),   // 60₹ value, 10kg weight - ratio = 6
            new Item(100, 20),  // 100₹ value, 20kg weight - ratio = 5
            new Item(120, 30)   // 120₹ value, 30kg weight - ratio = 4
        };
        
        // Function call karo
        double maxValue = fractionalKnapsack(W, arr);
        System.out.println("Maximum value we can obtain = " + maxValue);
        
        // Expected output: 240
        // Calculation: 
        // 1. First item (60₹, 10kg) - full - 60₹ (10kg used, 40kg left)
        // 2. Second item (100₹, 20kg) - full - 100₹ (30kg used, 20kg left)
        // 3. Third item (120₹, 30kg) - 2/3 fraction - 80₹ (50kg used, 0kg left)
        // Total: 60 + 100 + 80 = 240₹
        
        // Example 2
        int W2 = 10;
        Item[] arr2 = { new Item(500, 30) };  // 500₹ value, 30kg weight
        double maxValue2 = fractionalKnapsack(W2, arr2);
        System.out.println("Example 2 - Maximum value = " + maxValue2);
        // Expected: 166.67 (1/3 of the item)
    }
}
```
### Greedy Solution ka Logic:

**Main idea:** Hum items ko value/weight ratio ke basis pe sort karenge (descending order mein) aur phir highest ratio wale items ko pehle lenge.

Chalo ab visually samajhte hain kaise Fractional Knapsack algorithm work karta hai:

```
Items: (value, weight)
A: (60₹, 10kg) → Ratio: 6₹/kg
B: (100₹, 20kg) → Ratio: 5₹/kg  
C: (120₹, 30kg) → Ratio: 4₹/kg
```

Step-by-step process:

1. Sabse pehle items ko ratio ke hisab se sort karo:
   ```
   Sorted: A(6₹/kg) > B(5₹/kg) > C(4₹/kg)
   ```

2. Highest ratio wale items se knapsack bharna start karo:
   ```
   - Item A: (60₹, 10kg) - Full item liya
     (Used capacity: 10kg, Remaining: 40kg, Value so far: 60₹)
   
   - Item B: (100₹, 20kg) - Full item liya
     (Used capacity: 30kg, Remaining: 20kg, Value so far: 160₹)
   
   - Item C: (120₹, 30kg) - Only 20kg liya (fraction: 20/30 = 2/3)
     Value added: 120₹ × (2/3) = 80₹
     (Used capacity: 50kg, Remaining: 0kg, Total value: 240₹)
   ```

Final value: 240₹

## Why Greedy is Better than Dynamic Programming here?

### Greedy vs Dynamic Programming for Fractional Knapsack

1. **Fractional Knapsack mein Greedy kyun better hai?**

   Fractional Knapsack mein, hum items ke fractions le sakte hain. Iska matlab, hum humesha highest value/weight ratio wale item ko prefer kar sakte hain kyunki hum exactly utna hi fraction le sakte hain jitna hamari bag mein fit ho jayega.

   Greedy approach mein:
   - Time complexity: O(N log N) - sirf sorting ke liye
   - Space complexity: O(N)
   - Simple implementation

2. **Dynamic Programming (DP) kyun zaruri nahi hai?**

   DP tab useful hota hai jab humein subproblems ko solve karna hota hai aur "optimal substructure" hota hai. Lekin Fractional Knapsack mein hum fractions le sakte hain, isliye local optimization (highest ratio items ko prefer karna) global optimization bhi de deta hai.

   DP approach agar use karein:
   - Unnecessarily complex ho jayega
   - Time complexity: O(N × W) - jahan W capacity hai - ye generally worse hai O(N log N) se
   - Extra memory use hogi for memoization

3. **Simple example to understand:**
   
   Imagine aapke paas gold, silver aur copper ke bars hain, sabka different weight aur value hai. Aap greedy approach use karke sabse pehle gold lenge (highest value/weight), phir silver, phir copper. Agar koi bar pura nahi fit hota, aap uska fraction le lenge. Ye approach guaranteed optimal solution dega.

## 0/1 Knapsack vs Fractional Knapsack

| 0/1 Knapsack | Fractional Knapsack |
|--------------|---------------------|
| Item ya to pura lo ya bilkul na lo | Item ka fraction bhi le sakte hain |
| Greedy approach optimal nahi hai | Greedy approach optimal hai |
| DP approach use karna padta hai | Simple greedy sorting approach kaam karta hai |
| Time Complexity: O(N × W) | Time Complexity: O(N log N) |

## How to Think About Greedy Problems in Interviews?

Interview mein Greedy problems ko approach karne ke liye follow karo:

1. **Pehchano** ki problem greedy approach se solve ho sakti hai ya nahi
   - Kya local optimal choices global optimal solution degi?
   - Kya hum fractions le sakte hain ya binary decisions hain?

2. **Greedy Choice** identify karo:
   - Fractional Knapsack: value/weight ratio
   - Activity Selection: finish time
   - Huffman Coding: frequency
   - Etc.

3. **Prove karo** (ya at least explain) ki greedy approach optimal solution dega
   - Example ke through samjhao
   - Counter-examples search karo to make sure

4. **Time & Space Complexity** analyze karo
   - Usually sorting ki wajah se O(N log N) time complexity hogi

## Activity Selection Problem Example

Ek aur famous greedy problem samajhte hain - Activity Selection:

```
Activities: (start time, finish time)
A: (10, 20)
B: (12, 25)
C: (20, 30)
```

Greedy approach: Activities ko finish time ke basis pe sort karo, aur earliest finish time wale activities ko prefer karo.

1. Sort by finish time:
   ```
   A(10, 20), B(12, 25), C(20, 30)
   ```

2. First activity (A) select karo:
   ```
   Selected: A(10, 20)
   ```

3. Next activity tabhi select karo jab uska start time >= previous selected activity ka finish time:
   ```
   B's start time (12) < A's finish time (20) ❌
   C's start time (20) >= A's finish time (20) ✅
   Selected: A(10, 20), C(20, 30)
   ```

Maximum possible activities: 2 (A and C)

## Conclusion

Greedy algorithms kuch specific types ke problems ko solve karne ka ek powerful approach hai. Yaad rakhein:

1. Har problem greedy se solve nahi ho sakta
2. Agar problem mein "local optimal = global optimal" property hai, to greedy approach try karein
3. Fractional Knapsack jaise problems mein greedy approach best hai, while 0/1 Knapsack mein DP better hai
4. Interviews mein, approach explain karo, prove karo ki approach optimal hai, aur complexity analyze karo

I hope ye explanation aapko greedy algorithms ko samajhne mein help kari hogi! Koi bhi question ho to zaroor puchiye.


### Code using Lambda expression for learning -

```java
import java.util.Arrays;
import java.util.Comparator;

class Item {
    int value, weight;
    
    Item(int value, int weight) {
        this.value = value;
        this.weight = weight;
    }
}

public class FractionalKnapsackWithLambda {
    
    public static double fractionalKnapsack(int W, Item[] arr) {
        // Traditional way with inner class
        /*
        Arrays.sort(arr, new Comparator<Item>() {
            @Override
            public int compare(Item a, Item b) {
                double r1 = (double) a.value / a.weight;
                double r2 = (double) b.value / b.weight;
                return Double.compare(r2, r1);
            }
        });
        */
        
        // Lambda expression way - much shorter and cleaner!
        // Ye line samjho: (a, b) ye parameters hain, -> ke baad logic hai
        Arrays.sort(arr, (a, b) -> Double.compare(
            (double) b.value / b.weight,  // Pehla ratio (descending order)
            (double) a.value / a.weight   // Dusra ratio
        ));
        
        // Alternative lambda with method reference (more advanced)
        // Arrays.sort(arr, Comparator.comparingDouble((Item item) -> 
        //    (double) item.value / item.weight).reversed());
        
        double finalValue = 0.0;
        int remainingCapacity = W;
        
        for (Item item : arr) {
            if (item.weight <= remainingCapacity) {
                remainingCapacity -= item.weight;
                finalValue += item.value;
            } else {
                double fraction = (double) remainingCapacity / item.weight;
                finalValue += item.value * fraction;
                break;
            }
        }
        
        return finalValue;
    }
    
    public static void main(String[] args) {
        int W = 50;
        Item[] arr = { 
            new Item(60, 10),
            new Item(100, 20),
            new Item(120, 30)
        };
        
        double maxValue = fractionalKnapsack(W, arr);
        System.out.println("Maximum value with lambda: " + maxValue);
    }
}
```
