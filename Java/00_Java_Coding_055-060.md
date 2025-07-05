## 055.Java-coding-problems-introduction.md
💡 **Coding Problems: Approach and Best Practices**

- → **Interactive Problem Solving:** Instead of directly jumping into code, explain your thought process, how you approach the problem, and how you arrive at a solution. This demonstrates communication skills and logical thinking.

- → **Adhere to Conventions:**
  - → Use standard naming conventions for classes, methods, and variables.
  - → Ensure your code is readable and well-structured.
  - → Validate input data and throw appropriate exceptions where necessary.
  - → Consider non-functional requirements like security and performance.

- → **Multiple Solutions:** Be prepared to discuss different approaches to solving a problem. Present your primary solution and then explain alternative methods, highlighting their trade-offs (e.g., time complexity, space complexity, readability).
## 056.Java-coding-problem-1.md
## 📝 Coding Problem 1: Word Frequency Counter

**Problem Statement:**
Given a `List<String>` (representing words), write a Java program to count the frequency of each word in the list. The output should be a `Map<String, Integer>` where the key is the word and the value is its count.

**Example:**
Input: `List<String> words = Arrays.asList("apple", "banana", "apple", "orange", "banana", "apple");`
Output: `{"apple": 3, "banana": 2, "orange": 1}`

---

### Approach 1: Pre-Java 8 (Imperative Style)

**Thought Process:**
Before Java 8, the most straightforward way to solve this problem would be to iterate through the list of words and use a `HashMap` to store the counts. For each word, we would check if it already exists as a key in the map. If it does, we increment its count; otherwise, we add it to the map with a count of 1.

**Implementation:**
```java
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Arrays;

public class WordFrequencyPreJava8 {

    public static Map<String, Integer> countWordFrequencies(List<String> words) {
        Map<String, Integer> wordFrequencies = new HashMap<>();

        for (String word : words) {
            // Get the current count, or 0 if the word is not yet in the map
            Integer count = wordFrequencies.get(word);
            if (count == null) {
                wordFrequencies.put(word, 1); // First occurrence
            } else {
                wordFrequencies.put(word, count + 1); // Increment count
            }
        }
        return wordFrequencies;
    }

    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "apple", "orange", "banana", "apple");
        Map<String, Integer> frequencies = countWordFrequencies(words);
        System.out.println("Word Frequencies (Pre-Java 8): " + frequencies);
    }
}
```

---

### Approach 2: Post-Java 8 (Functional Style with Streams)

**Thought Process:**
Java 8's Stream API, especially with `Collectors.groupingBy()`, provides a very concise and declarative way to solve this problem. We can stream the list of words and then group them by their identity, counting the occurrences of each word.

**Implementation:**
```java
import java.util.List;
import java.util.Map;
import java.util.Arrays;
import java.util.function.Function;
import java.util.stream.Collectors;

public class WordFrequencyPostJava8 {

    public static Map<String, Integer> countWordFrequencies(List<String> words) {
        return words.stream()
                    .collect(Collectors.groupingBy(
                        Function.identity(), // Group by the word itself
                        Collectors.summingInt(e -> 1) // Count occurrences
                    ));
    }

    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "apple", "orange", "banana", "apple");
        Map<String, Integer> frequencies = countWordFrequencies(words);
        System.out.println("Word Frequencies (Post-Java 8): " + frequencies);
    }
}
```

---

### Discussion on Approaches:

- → **Readability:** The Java 8 Stream API approach is generally considered more readable and concise for this type of data transformation, as it expresses *what* needs to be done rather than *how* (declarative vs. imperative).
- → **Conciseness:** The Stream API solution requires fewer lines of code.
- → **Performance:** For small to medium datasets, the performance difference is often negligible. For very large datasets, the Stream API can potentially leverage parallel processing (using `parallelStream()`), though this comes with its own overhead and considerations.
- → **Maintainability:** Both are maintainable, but the Stream API can be easier to reason about for complex transformations once familiar with the paradigm.
- → **Error Handling:** Both approaches handle basic cases. For more robust solutions (e.g., handling null words, case-insensitivity), additional logic would be added to both.
## 057.Java-coding-problem-2.md
## 📝 Coding Problem 2: Find Duplicates in a List

**Problem Statement:**
Given a `List<String>`, find all the duplicate elements in the list. The output should be a `List<String>` containing only the duplicate words.

**Example:**
Input: `List<String> words = Arrays.asList("apple", "banana", "apple", "orange", "banana", "grape");`
Output: `["apple", "banana"]` (order may vary)

---

### Approach 1: Pre-Java 8 (Imperative Style)

**Thought Process:**
To find duplicates, we can iterate through the list and use a `HashSet` to keep track of elements we've already seen. If `add()` to the `HashSet` returns `false` (meaning the element was already present), then it's a duplicate. We'll use another `HashSet` to store the duplicates to ensure uniqueness among them.

**Implementation:**
```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Set;
import java.util.Arrays;

public class FindDuplicatesPreJava8 {

    public static List<String> findDuplicates(List<String> words) {
        Set<String> seen = new HashSet<>();
        Set<String> duplicates = new HashSet<>();

        for (String word : words) {
            if (!seen.add(word)) { // If add returns false, it's a duplicate
                duplicates.add(word);
            }
        }
        return new ArrayList<>(duplicates);
    }

    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "apple", "orange", "banana", "grape");
        List<String> duplicateWords = findDuplicates(words);
        System.out.println("Duplicate Words (Pre-Java 8): " + duplicateWords);
    }
}
```

---

### Approach 2: Post-Java 8 (Functional Style with Streams)

**Thought Process:**
With Java 8 Streams, we can achieve this by first streaming the elements. We can use `Collectors.groupingBy` to group words and count their occurrences. Then, we filter these entries to find those with counts greater than 1.

**Implementation:**
```java
import java.util.List;
import java.util.Map;
import java.util.Set;
import java.util.Arrays;
import java.util.function.Function;
import java.util.stream.Collectors;

public class FindDuplicatesPostJava8 {

    public static List<String> findDuplicates(List<String> words) {
        return words.stream()
                    .collect(Collectors.groupingBy(Function.identity(), Collectors.counting())) // Group and count
                    .entrySet().stream()
                    .filter(entry -> entry.getValue() > 1) // Filter for counts > 1
                    .map(Map.Entry::getKey) // Get the word (key)
                    .collect(Collectors.toList()); // Collect to a list
    }

    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "apple", "orange", "banana", "grape");
        List<String> duplicateWords = findDuplicates(words);
        System.out.println("Duplicate Words (Post-Java 8): " + duplicateWords);
    }
}
```

---

### Discussion on Approaches:

- → **Readability:** The Java 8 Stream approach is more declarative and can be more concise for this type of transformation, especially for developers familiar with functional programming concepts. The pre-Java 8 approach is explicit and easy to follow for those less familiar with streams.
- → **Conciseness:** The Stream API solution is more compact.
- → **Performance:** Both approaches have similar time complexities. The pre-Java 8 approach might be slightly more performant for very large lists due to less overhead, but the difference is often negligible. The Stream API can be parallelized for very large datasets.
- → **Memory Usage:** Both use auxiliary data structures (`HashSet` or `Map`) to track elements, so memory usage is comparable.
## 058.Java-coding-problem-3.md
## 📝 Coding Problem 3: Reverse a String

**Problem Statement:**
Given a string, return a new string with the characters in reverse order.

**Example:**
Input: `String str = "hello";`
Output: `"olleh"`

---

### Approach 1: Pre-Java 8 (Imperative Style - Using StringBuilder)

**Thought Process:**
The most efficient and common way to reverse a string in Java, even before Java 8, is to use the `StringBuilder` class. `StringBuilder` is mutable, unlike `String`, and provides a convenient `reverse()` method. Alternatively, one could iterate through the string from end to beginning and append characters to a new `String` or `char[]`.

**Implementation:**
```java
public class ReverseStringPreJava8 {

    public static String reverseString(String str) {
        if (str == null || str.isEmpty()) {
            return str;
        }
        return new StringBuilder(str).reverse().toString();
    }

    public static void main(String[] args) {
        String original = "hello";
        String reversed = reverseString(original);
        System.out.println("Original: " + original + ", Reversed (Pre-Java 8): " + reversed);

        original = "Java";
        reversed = reverseString(original);
        System.out.println("Original: " + original + ", Reversed (Pre-Java 8): " + reversed);
    }
}
```

---

### Approach 2: Post-Java 8 (Functional Style - Character Stream)

**Thought Process:**
While `StringBuilder.reverse()` remains the most idiomatic and performant way to reverse a string, for demonstration purposes of Java 8 features, one could convert the string to a stream of characters, process them, and then collect them. This approach is generally less efficient for simple string reversal but showcases stream operations.

**Implementation:**
```java
import java.util.stream.Collectors;

public class ReverseStringPostJava8 {

    public static String reverseString(String str) {
        if (str == null || str.isEmpty()) {
            return str;
        }
        // Convert string to a stream of characters, then reverse and collect
        return str.chars() // Returns an IntStream of char values
                  .mapToObj(c -> (char) c) // Convert int to Character object
                  .collect(StringBuilder::new, StringBuilder::append, StringBuilder::append)
                  .reverse()
                  .toString();
    }

    public static void main(String[] args) {
        String original = "hello";
        String reversed = reverseString(original);
        System.out.println("Original: " + original + ", Reversed (Post-Java 8): " + reversed);

        original = "Java";
        reversed = reverseString(original);
        System.out.println("Original: " + original + ", Reversed (Post-Java 8): " + reversed);
    }
}
```

---

### Discussion on Approaches:

- → **Efficiency:** The `StringBuilder.reverse()` approach (Pre-Java 8) is significantly more efficient and is the recommended way to reverse a string in Java. The Stream API approach, while demonstrating functional programming, involves more overhead due to boxing/unboxing and stream operations, making it less performant for this specific task.
- → **Readability:** For simple string reversal, `StringBuilder.reverse()` is very clear. The Stream API approach is more verbose for this particular problem but can be elegant for more complex character-by-character transformations.
- → **Idiomatic Java:** `StringBuilder.reverse()` is the idiomatic Java solution for string reversal.
- → **Use Case:** The Stream API approach for string manipulation is more suited for scenarios where you need to filter, transform, or aggregate characters based on certain criteria, rather than a simple reversal.
## 059.Java-coding-problem-4.md
## 📝 Coding Problem 4: Check if a String is a Palindrome

**Problem Statement:**
Given a string, determine if it is a palindrome. A palindrome is a word, phrase, number, or other sequence of characters which reads the same backward as forward, ignoring cases.

**Example:**
Input: `String str = "madam";` Output: `true`
Input: `String str = "hello";` Output: `false`
Input: `String str = "Racecar";` Output: `true`

---

### Approach 1: Pre-Java 8 (Imperative Style - Two Pointers)

**Thought Process:**
The most common and efficient way to check for a palindrome is to use a two-pointer approach. One pointer starts at the beginning of the string, and the other starts at the end. We compare characters at these pointers, moving inwards. If at any point the characters don't match (ignoring case), it's not a palindrome. We also need to handle non-alphanumeric characters if the problem statement implies ignoring them.

**Implementation:**
```java
public class PalindromeCheckerPreJava8 {

    public static boolean isPalindrome(String str) {
        if (str == null || str.isEmpty()) {
            return true; // Empty or null string is considered a palindrome
        }

        String cleanedStr = str.toLowerCase(); // Ignore case
        int left = 0;
        int right = cleanedStr.length() - 1;

        while (left < right) {
            // Skip non-alphanumeric characters (if required by problem, otherwise remove these lines)
            while (left < right && !Character.isLetterOrDigit(cleanedStr.charAt(left))) {
                left++;
            }
            while (left < right && !Character.isLetterOrDigit(cleanedStr.charAt(right))) {
                right--;
            }

            if (cleanedStr.charAt(left) != cleanedStr.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }

    public static void main(String[] args) {
        System.out.println("madam is palindrome: " + isPalindrome("madam")); // true
        System.out.println("hello is palindrome: " + isPalindrome("hello")); // false
        System.out.println("Racecar is palindrome: " + isPalindrome("Racecar")); // true
        System.out.println("A man, a plan, a canal: Panama is palindrome: " + isPalindrome("A man, a plan, a canal: Panama")); // true
    }
}
```

---

### Approach 2: Post-Java 8 (Functional Style - Using Streams and StringBuilder)

**Thought Process:**
While the two-pointer approach is more efficient, we can demonstrate Java 8 features by first cleaning and normalizing the string using streams, then reversing it using `StringBuilder` (which is still the most efficient reversal method), and finally comparing the original and reversed strings.

**Implementation:**
```java
import java.util.stream.Collectors;

public class PalindromeCheckerPostJava8 {

    public static boolean isPalindrome(String str) {
        if (str == null || str.isEmpty()) {
            return true;
        }

        // Clean and normalize the string using streams
        String cleanedStr = str.toLowerCase().chars()
                               .filter(Character::isLetterOrDigit)
                               .mapToObj(c -> String.valueOf((char) c))
                               .collect(Collectors.joining());

        // Reverse the cleaned string using StringBuilder
        String reversedStr = new StringBuilder(cleanedStr).reverse().toString();

        return cleanedStr.equals(reversedStr);
    }

    public static void main(String[] args) {
        System.out.println("madam is palindrome: " + isPalindrome("madam")); // true
        System.out.println("hello is palindrome: " + isPalindrome("hello")); // false
        System.out.println("Racecar is palindrome: " + isPalindrome("Racecar")); // true
        System.out.println("A man, a plan, a canal: Panama is palindrome: " + isPalindrome("A man, a plan, a canal: Panama")); // true
    }
}
```

---

### Discussion on Approaches:

- → **Efficiency:** The two-pointer approach (Pre-Java 8) is generally more efficient as it avoids creating intermediate `String` objects for the reversed string and can stop early if a mismatch is found. The Post-Java 8 approach involves more overhead due to stream operations and string concatenation.
- → **Readability:** Both approaches are reasonably readable. The two-pointer method is a classic algorithm, while the Stream API approach is more declarative for the cleaning step.
- → **Idiomatic Java:** For simple palindrome checks, the two-pointer method or `StringBuilder.reverse()` (if the problem allows for direct reversal without character filtering) are more idiomatic and performant.
- → **Flexibility:** The Stream API approach offers more flexibility if the cleaning/filtering logic becomes more complex, as it leverages the power of streams for transformations.
## 060.Java-coding-problem-5.md
## 📝 Coding Problem 5: Find the First Non-Repeated Character in a String

**Problem Statement:**
Given a string, find the first character that does not repeat anywhere in the string.

**Example:**
Input: `String str = "swiss";` Output: `w`
Input: `String str = "teeter";` Output: `r`
Input: `String str = "aabbcc";` Output: `null` (or throw exception, or return special char)

---

### Approach 1: Pre-Java 8 (Imperative Style - Using HashMap)

**Thought Process:**
To find the first non-repeated character, we can iterate through the string and count the occurrences of each character using a `HashMap<Character, Integer>`. After populating the map, we iterate through the string again. The first character encountered whose count in the map is 1 is our answer.

**Implementation:**
```java
import java.util.HashMap;
import java.util.Map;

public class FirstNonRepeatedCharPreJava8 {

    public static Character findFirstNonRepeatedChar(String str) {
        if (str == null || str.isEmpty()) {
            return null;
        }

        Map<Character, Integer> charCounts = new HashMap<>();

        // First pass: Populate character counts
        for (char c : str.toCharArray()) {
            charCounts.put(c, charCounts.getOrDefault(c, 0) + 1);
        }

        // Second pass: Find the first character with count 1
        for (char c : str.toCharArray()) {
            if (charCounts.get(c) == 1) {
                return c;
            }
        }
        return null; // No non-repeated character found
    }

    public static void main(String[] args) {
        System.out.println("swiss -> " + findFirstNonRepeatedChar("swiss")); // w
        System.out.println("teeter -> " + findFirstNonRepeatedChar("teeter")); // r
        System.out.println("aabbcc -> " + findFirstNonRepeatedChar("aabbcc")); // null
        System.out.println("programming -> " + findFirstNonRepeatedChar("programming")); // p
    }
}
```

---

### Approach 2: Post-Java 8 (Functional Style - Using Streams and Collectors)

**Thought Process:**
With Java 8 Streams, we can first group characters by their identity and count their occurrences using `Collectors.groupingBy` and `Collectors.counting()`. Then, we can filter these entries to find those with a count of 1 and extract the first one.

**Implementation:**
```java
import java.util.LinkedHashMap;
import java.util.Map;
import java.util.Optional;
import java.util.function.Function;
import java.util.stream.Collectors;

public class FirstNonRepeatedCharPostJava8 {

    public static Character findFirstNonRepeatedChar(String str) {
        if (str == null || str.isEmpty()) {
            return null;
        }

        // Use LinkedHashMap to preserve insertion order for the first non-repeated character
        Map<Character, Long> charCounts = str.chars()
                                            .mapToObj(c -> (char) c)
                                            .collect(Collectors.groupingBy(
                                                Function.identity(),
                                                LinkedHashMap::new, // Important: maintain order
                                                Collectors.counting()
                                            ));

        Optional<Character> firstNonRepeated = charCounts.entrySet().stream()
                                                        .filter(entry -> entry.getValue() == 1)
                                                        .map(Map.Entry::getKey)
                                                        .findFirst();

        return firstNonRepeated.orElse(null);
    }

    public static void main(String[] args) {
        System.out.println("swiss -> " + findFirstNonRepeatedChar("swiss")); // w
        System.out.println("teeter -> " + findFirstNonRepeatedChar("teeter")); // r
        System.out.println("aabbcc -> " + findFirstNonRepeatedChar("aabbcc")); // null
        System.out.println("programming -> " + findFirstNonRepeatedChar("programming")); // p
    }
}
```

---

### Discussion on Approaches:

- → **Efficiency:** Both approaches have a time complexity of O(N) where N is the length of the string, as they typically involve two passes (one for counting, one for finding). The space complexity is O(K) where K is the number of unique characters.
- → **Readability:** The pre-Java 8 approach is very explicit and easy to follow for those new to Java. The post-Java 8 approach is more concise and declarative, leveraging the power of the Stream API, but requires familiarity with collectors and functional programming.
- → **Order Preservation:** For the post-Java 8 approach, it's crucial to use `LinkedHashMap::new` in `groupingBy` to ensure the order of insertion is preserved, which is necessary to find the *first* non-repeated character.
- → **Edge Cases:** Both handle null/empty strings. The return type `Character` allows returning `null` if no such character is found.
