/* Online Java Compiler and Editor */
```
public class HelloWorld{

     public static void main(String []args){
        // System.out.println("Hello, World!");
        int[] ids = {1, 2, 3, 4, 5};
        // int result = a1(ids);
        // First answe
        // System.out.println(result);
        
        // // Second answer
        // int[] id2 = {3, 3, 4, 4};
        // int result2 = a2(id2);
        
        // System.out.println(result2);
        
        // Third answer
        char[] id3 = {'a', 'b', 'c'};
        char[] result3 = a3(id3, 0, 3);
        
        System.out.println(result3);
        
        
     }
     
    //  Third answer
    static char[] a3(char[] a, int start, int length)
        {
        if (length < 0 || start < 0 || start+length-1>=a.length)
            {
                return null;
            }
            
        char[] sub = new char[length];
        for (int i=start, j=0; j<length; i++, j++)
            {
             sub[j] = a[i];
            }
        return sub;
    }
     
     // Second answer
     static int a2(int[] a)
        {
            int sumEven = 0;
            int sumOdd = 0;
            for (int i=0; i<a.length; i++)
                {
                    if (a[i]%2 == 0)
                    sumEven += a[i];
                    else
                    sumOdd += a[i];
                }
            return sumOdd - sumEven;
        }
     
    //  First answe
     static int a1(int[] a)
        {
            if (a == null || a.length % 2 == 0) return 0;
            int midIndex = a.length / 2 ;
            System.out.println(midIndex);
            
            int middleItem = a[midIndex];
            System.out.println(middleItem);
            
            for (int i=0; i<a.length; i++)
                {
                    if (i != midIndex && middleItem >= a[i])
                    return 0;
                }
                    return 1;
        }
}
```


Compile: 
https://www.programiz.com/java-programming/online-compiler/
https://www.tutorialspoint.com/online_java_compiler.php
