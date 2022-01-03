# Console Based Programs

- [Meter to Feet](#meter-to-feet)
- [BMI Calculator](#bmi-calculator)
- [Numbers in Decreasing order](#numbers-in-decreasing-order)
- [Entered character is vowel or not](#entered-character-is-vowel-or-not)
- [Random Number Plate Generator](#random-number-plate-generator)
- [Prime factors of a number](#prime-factors-of-a-number)
- [GCD of two numbers](#gcd-of-two-numbers)
- [Array Reverse](#array-reverse)
- [6x6 Random Matrix of 0s and 1s and counting of 1s](#6x6-random-matrix-of-0s-and-1s-and-counting-of-1s)
- [Expression Evaluation (2 operands, 1 operator)](#expression-evaluation-2-operands-1-operator)
- [Draw 4 random cards from a deck](#draw-4-random-cards-from-a-deck)
- [Two Roots of Quadratic](#two-roots-of-quadratic)
- [Pyramid of entered string (Left)](#pyramid-of-entered-string-left)
- [Diamond Pattern](#diamond-pattern)

## Meter to Feet

Write a program that reads a number in meters, converts it to feet, and displays the result.

```Java
public class Main {
   public static void main(String[] args) {
      double feet, meter;
      Scanner sc = new Scanner(System.in);
      System.out.print("Enter meters: ");
      meter = sc.nextDouble();
      feet = meter * 3.28084;
      System.out.println(meter + " meter = " + feet + " feet");
   }
}
```

## BMI Calculator

*Body Mass Index (BMI) is a measure of health on weight. It can be calculated by taking your weight in kilograms and dividing by the square of your height in meters.*

Write a program that prompts the user to enter a weight in pounds and height in inches and displays the BMI.

```Java
public class Main {
   public static void main(String[] args) {
      double BMI, wPounds, wKgs, hInches, hMeters;
      Scanner sc = new Scanner(System.in);
      System.out.print("Enter weight (in pounds): ");
      wPounds = sc.nextDouble();
      System.out.print("Enter height (in inches): ");
      hInches = sc.nextDouble();
      hMeters = hInches * 0.0254;
      wKgs = wPounds * 0.45359237;
      BMI = wKgs / (hMeters * hMeters);
      System.out.println("Your BMI: " + BMI);
   }
}
```

## Numbers in Decreasing order

Write a program that prompts the user to enter three integers and display the integers in decreasing order.

```Java
public class Main {
   public static void main(String[] args) {
      int n1, n2, n3, temp;
      Scanner sc = new Scanner(System.in);
      System.out.print("Enter number 1: ");
      n1 = sc.nextInt();
      System.out.print("Enter number 2: ");
      n2 = sc.nextInt();
      System.out.print("Enter number 3: ");
      n3 = sc.nextInt();
      if (n1 < n2) {
         temp = n1;
         n1 = n2;
         n2 = temp;
      }
      if (n1 < n3) {
         temp = n3;
         n3 = n1;
         n1 = temp;
      }
      if (n2 < n3) {
         temp = n3;
         n3 = n2;
         n2 = temp;
      }
      System.out.println("Numbers in decreasing order: " + n1 + " " + n2 + " " + n3);
   }
}
```

## Entered character is vowel or not

Write a program that prompts the user to enter a letter and check whether a letter is a vowel or constant.

```Java
public class Main {
   public static void main(String[] args) {
      char ch;
      String ans;
      Scanner sc = new Scanner(System.in);
      System.out.print("Enter char: ");
      ch = sc.next().toLowerCase().charAt(0);
      ans = (ch == 'a' || ch == 'e' || ch == 'i' || ch == 'o' || ch == 'u') ? "vowel" : "constant";
      System.out.println("Character is " + ans);
   }
}
```

## Random Number Plate Generator

Assume a vehicle plate number consists of three uppercase letters followed by four digits. Write a program to generate a plate number.

```Java
public class Main {
   public static void main(String[] args) {
      String NumberPlate = "", x = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
      int number = (int) (Math.random() * 10000);
      NumberPlate += x.charAt((int) (Math.random() * x.length()));
      NumberPlate += x.charAt((int) (Math.random() * x.length()));
      NumberPlate += x.charAt((int) (Math.random() * x.length()));
      NumberPlate += " " + String.valueOf(number);
      System.out.println("Random Number plate: " + NumberPlate);
   }
}
```

## Prime factors of a number

```Java
public class Main {
   public static void main(String[] args) {
      ArrayList<Integer> factors = new ArrayList<>();
      int x, temp, i = 2, mul = 1;
      Scanner sc = new Scanner(System.in);
      System.out.print("Enter number: ");
      x = sc.nextInt();
      temp = x;
      while (temp % i == 0) {
         factors.add(i);
         temp /= i;
      }
      for (int n : factors) {
         mul *= n;
      }
      for (i = 3; i <= (x / mul); i += 2) {
         while (temp % i == 0) {
            factors.add(i);
            temp /= i;
         }
      }
      System.out.print("Prime factors of " + x + ": ");
      for (i = 0; i < factors.size(); i++) {
         if (i != factors.size() - 1) {
            System.out.print(factors.get(i) + ", ");
         } else {
            System.out.println(factors.get(i));
         }
      }
   }
}
```

## GCD of two numbers

Write a method with following method header

`public static int gcd(int num1, int num2)`

Write a program that prompts the user to enter two integers and compute the gcd of two integers.

```Java
public class Main {
   public static int gcd(int number1, int number2) {
      int max = (number1 >= number2) ? number1 : number2;
      int gcd = 1;
      for (int i = 2; i <= max; i++) {
         while ((number1 % i == 0) && (number2 % i == 0)) {
            gcd *= i;
            number1 /= i;
            number2 /= i;
         }
      }
      return gcd;
   }

   public static void main(String[] args) {
      Scanner sc = new Scanner(System.in);
      int n1, n2, gcd;
      System.out.print("Enter number 1: ");
      n1 = sc.nextInt();
      System.out.print("Enter number 2: ");
      n2 = sc.nextInt();
      gcd = gcd(n1, n2);
      System.out.println("Greatest Comman Divisor of " + n1 + " and " + n2 + " is " + gcd);
   }
}
```

## Array Reverse

Write a test program that prompts the user to enter ten numbers, invoke a method to reverse the numbers, display the numbers.

```Java
public class Main {
   static void ReverseArray(int[] array) {
      for (int i = 0, temp; i < (array.length / 2); i++) {
         temp = array[i];
         array[i] = array[(array.length - (1 + i))];
         array[(array.length - (1 + i))] = temp;
      }
   }

   public static void main(String[] args) {
      Scanner sc = new Scanner(System.in);
      int size = 10;
      int[] array = new int[size];
      System.out.println("***Enter " + size + " Numbers***");
      for (int i = 0; i < size; i++) {
         array[i] = sc.nextInt();
      }
      ReverseArray(array);
      System.out.println("***Reversed Array***");
      for (int i = 0; i < size; i++) {
         if (i == (size - 1))
            System.out.println(array[i]);
         else
            System.out.print(array[i] + ", ");
      }
   }
}
```

## 6x6 Random Matrix of 0s and 1s and counting of 1s

Write a program that generate 6*6 two-dimensional matrix, filled with 0's and 1's, display the matrix, check every raw and column have an odd number's of 1's.

```Java
public class Main {
   static int[][] generateMatrixWithZerosAndOnes(int x, int y) {
      int[][] matrix = new int[x][y];
      for (int i = 0; i < x; i++) {
         for (int j = 0; j < y; j++) {
            matrix[i][j] = (int) (Math.random() * 10) % 2;
         }
      }
      return matrix;
   }

   static int numberOfOnesInRow(int[][] matrix, int row) {
      int count = 0;
      for (int i = 0; i < matrix.length; i++) {
         if (matrix[row][i] == 1) {
            count++;
         }
      }
      return count;
   }

   static int numberOfOnesInColumn(int[][] matrix, int column) {
      int count = 0;
      for (int i = 0; i < matrix.length; i++) {
         if (matrix[i][column] == 1) {
            count++;
         }
      }
      return count;
   }

   public static void main(String[] args) {
      final int size = 6;
      int[][] matrix = new int[size][size];
      matrix = generateMatrixWithZerosAndOnes(size, size);
      System.out.println("***Generated Matrix***");
      for (int i = 0; i < matrix.length; i++) {
         for (int j = 0; j < matrix[i].length; j++) {
            System.out.print(matrix[i][j] + " ");
         }
         System.out.println();
      }
      System.out.println();
      int row1s = 0, col1s = 0;
      for (int i = 0; i < matrix.length; i++) {
         if ((row1s = numberOfOnesInRow(matrix, i)) % 2 == 1) {
            System.out.println("Row " + (i + 1) + " has odd number of 1s, which are " + row1s);
         }
      }
      for (int i = 0; i < matrix.length; i++) {
         if ((col1s = numberOfOnesInColumn(matrix, i)) % 2 == 1) {
            System.out.println("Column " + (i + 1) + " has odd number of 1s, which are " + col1s);
         }
      }
   }
}
```

## Expression Evaluation (2 operands, 1 operator)

Write a program for calculator to accept an expression as a string in which the operands and operator are separated by zero or more spaces.

```Java
public class Main {
    public static void main(String[] args) {
        java.util.Scanner sc = new java.util.Scanner(System.in);
        System.out.print("Enter basic arithmetic expression: ");
        String expressionString = sc.nextLine();
        int number = 0, operatorIndex = 0;
        char operator = ' ';
        expressionString = expressionString.replaceAll(" ", "");
        for (int i = 0; i < expressionString.length(); i++) {
            char currentChar = expressionString.charAt(i);
            if (Character.isDigit(currentChar)) {
                int currentNumber = Integer.parseInt(currentChar + "");
                if (number == 0) {
                    number += currentNumber;
                } else {
                    number = (number * 10) + currentNumber;
                }
            } else if (currentChar == '+' || currentChar == '-' || currentChar == 'x' || currentChar == '*'
                    || currentChar == '/' || currentChar == '%') {
                operator = currentChar;
                operatorIndex = i;
                break;
            } else {
                continue;
            }
        }
        operatorIndex++;
        int number2 = Integer.parseInt(expressionString.substring(operatorIndex));
        int ans = 0;
        if (operator == '+') {
            ans = number + number2;
        } else if (operator == '-') {
            ans = number - number2;
        } else if (operator == 'x' || operator == '*') {
            ans = number * number2;
        } else if (operator == '/') {
            ans = number / number2;
        } else if (operator == '%') {
            ans = number % number2;
        }
        sc.close();
        System.out.println(number + " " + operator + " " + number2 + " = " + ans);
    }
}
```

## Draw 4 random cards from a deck

```Java
class Main {
   public static void main(String[] a) {
      final int MAX_CARDS = 52, DRAW_N_CARDS = 4;
      int[] deck = new int[MAX_CARDS];
      for (int i = 0; i < deck.length; i++) {
         deck[i] = (i + 1);
      }
      int[] shuffledDeck = shuffle(deck);
      for (int i = 0; i < DRAW_N_CARDS; i++) {
         System.out.println(getCard(shuffledDeck[i]) + " of " + getCardType(shuffledDeck[i]));
      }
   }

   static int[] shuffle(int[] a) {
      final int SRC_START_POS = 0, DES_START_POS = 0;
      int arrayLength = a.length;
      int[] aCopy = new int[arrayLength];
      System.arraycopy(a, SRC_START_POS, aCopy, DES_START_POS, arrayLength);
      arrayLength = aCopy.length;
      for (int i = 0; i < arrayLength; i++) {
         int random = (int) (Math.random() * arrayLength);
         if (a[i] != aCopy[random] && i != random) {
            int temp = aCopy[i];
            aCopy[i] = aCopy[random];
            aCopy[random] = temp;
         }
      }
      return aCopy;
   }

   static void print(int[] a) {
      int arrayLength = a.length;
      for (int i = 0; i < arrayLength; i++) {
         if (i != (arrayLength - 1)) {
            System.out.print(a[i] + ", ");
         } else {
            System.out.print(a[i]);
         }
      }
      System.out.println();
   }

   static String getCard(int card) {
      final int NO_OF_CARDS = 52, NO_OF_TYPES = 4, NO_OF_CARDS_IN_A_TYPE = (NO_OF_CARDS / NO_OF_TYPES);
      final int CARD_ACE = 1, CARD_JOKER = 11, CARD_QUEEN = 12, CARD_KING = 13;
      int temp = card % NO_OF_CARDS_IN_A_TYPE;
      if (temp == CARD_JOKER) {
         return "J";
      } else if (temp == CARD_QUEEN) {
         return "Q";
      } else if (temp == CARD_KING) {
         return "K";
      } else {
         if (temp == CARD_ACE) {
            return "A";
         } else {
            return String.valueOf(temp);
         }
      }
   }

   static String getCardType(int card) {
      if (card >= 1 && card <= 13) {
         return "Diamond";
      } else if (card >= 14 && card <= 26) {
         return "Heart";
      } else if (card >= 27 && card <= 39) {
         return "Spade";
      } else if (card >= 40 && card <= 52) {
         return "Club";
      }
      return "";
   }
}
```

## Two Roots of Quadratic

The two roots of a quadratic equation `ax^2 + bx + c = 0` can be obtained using the following formula: `b2 - 4ac` is called the discriminant of the quadratic equation. If it is positive, the equation has two real roots. If it is zero, the equation has one root. If it is negative, the equation has no real roots. Write a program that prompts the user to enter values for a, b, and c and displays the result based on the discriminant. If the discriminant is positive, display two roots. If the discriminant is 0, display one root. Otherwise, display "The equation has no real roots".

```Java
public class ep01 { 
   public static void main(String[] args) {
      Scanner sc = new Scanner(System.in);
      System.out.println("Insert for quadratic equation ax^2 + bx + c = 0");
      System.out.print("a: ");
      int a = sc.nextInt();
      System.out.print("b: ");
      int b = sc.nextInt();
      System.out.print("c: ");
      int c = sc.nextInt();  
      double temp = (b * b) - (4 * a * c);
      if (temp < 0) {
         System.out.println("The equation has no real roots");
      } else if (temp == 0) {
         System.out.println("It has one root");
         double r1 = ((-b) + Math.sqrt(temp)) / (2 * a);
         System.out.println("Root 1: " + r1);
      } else if (temp > 0) {
         System.out.println("It has two roots");
         double r1 = ((-b) + Math.sqrt(temp)) / (2 * a);
         double r2 = ((-b) - Math.sqrt(temp)) / (2 * a);
         System.out.println("Root 1: " + r1 + "\nRoot 2: " + r2);
      }
   }
}
```

## Pyramid of entered string (Left)

Write an interactive program to print a string entered in a right angled pyramid form.

```Java
public class Main {
    public static void main(String[] args) {
        System.out.print("Enter String: ");
        String str = new Scanner(System.in).next();
        for (int i = 0; i < str.length(); i++) {
            for (int j = 0; j <= i; j++) {
                System.out.print(str.charAt(j) + " ");
            }
            System.out.println();
        }
    }
}
```

Output

```Text
Enter String: abc
a
a b
a b c
```

## Pyramid of entered string (Center)

Write an interactive program to print a string entered in a centered pyramid form.

```Java
public class Main {
    public static void main(String[] args) {
        System.out.print("Enter String: ");
        String str = new Scanner(System.in).next();
        int strLength = str.length();
        for (int i = 0; i < strLength; i++) {
            for (int j = strLength - i; j > 0; j--) {
                System.out.print(" ");
            }
            for (int j = 0; j <= i; j++) {
                System.out.print(str.charAt(j) + " ");
            }
            System.out.println();
        }
    }
}
```

Output

```Text
Enter String: abc
  a
 a b
a b c
```

## Diamond Pattern

Write an interactive program to print a diamond shape.

```Java
public class ep12 {
    public static void main(String[] args) {
        System.out.print("Enter max number for pattern: ");
        int maxStars = new Scanner(System.in).nextInt();
        for (int i = 0; i < maxStars; i++) {
            for (int j = (maxStars - i); j > 0; j--) {
                System.out.print(" ");
            }
            for (int j = 0; j <= i; j++) {
                System.out.print("* ");
            }
            System.out.println();
        }
        for (int i = (maxStars - 1); i >= 0; i--) {
            for (int j = 0; j <= (maxStars - i); j++) {
                System.out.print(" ");
            }
            for (int j = i; j > 0; j--) {
                System.out.print("* ");
            }
            System.out.println();
        }
    }
}
```

Output

```Text
Enter max number for pattern: 4
    *
   * *
  * * *
 * * * *
  * * *
   * *
    *
```
